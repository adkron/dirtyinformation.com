---
  layout: post
  title: Testing Named Agents
---

*TLDR; When testing named agents that are started by an Elixir
applicaiton inject a name for the agent to eleminate intermitent
failures. [Screencast](https://youtu.be/gfI4LVS7pqM)*

We have been tasked to work on our company's trade secret incrementing
counter. It must be running at the startup of the application and we
don't want to keep passing the name of the agent around since there
should be only one.

First we have an application that starts up a named agent.

~~~elixir
defmodule AgentTest do
  use Application

  def start(_, _) do
    import Supervisor.Spec

    children = [
      worker(AgentTest.Counter, [], restart: :permanent)
    ]

    Supervisor.start_link(children, strategy: :one_for_one, name: AgentTest.Supervisor)
  end
end
~~~

Done, we are well on our way to winning.

Time to write some tests first because we are awesome developers.
Our tests utilize the counter that we started in the application so
there is no need to worry about starting the link on our own. This makes
for a nice clutter free test.

*TDD style of red/green/refactor is being skipped here for the article,
but not for the video or in real life.*

~~~elixir
#Test
defmodule AgentTest.CounterTest do
  use ExUnit.Case, async: true
  alias AgentTest.Counter

  test "initial value" do
    assert Counter.get == 0
  end

  test "increment" do
    Counter.increment
    assert Counter.get == 1
  end
end
~~~

Here we have our actual counter agent. Since we don't want to have to
worry about the name of the agent we use `__MODULE__` to make the name
`AgentTest.Counter` and not worry about passing it to get and increment
every time. The reason we use `__MODULE__` is so that if we refactor the
name of our module all the names of our processes also change.

~~~elixir
defmodule AgentTest.Counter do

  def start_link do
    Agent.start_link(fn -> 0 end, name: __MODULE__)
  end

  def get do
    Agent.get(__MODULE__, fn(count) -> count end)
  end

  def increment do
    Agent.update(__MODULE__, fn(count) -> count + 1 end)
  end
end
~~~

Time to run our tests.

~~~bash
amos@Noggin:~/workspace/$ mix test
Compiling 1 file (.ex)
...

Finished in 0.02 seconds
3 tests, 0 failures

Randomized with seed 565235
~~~

What a fantastic day! We ship the code off to the rest of the team as we
saved the day with trade secret counter implementation. A few minutes later
we notice our build is broke and like any good teammate we jump in to
fix the problem. We open up the build log.

~~~bash
Running tests
.

1) test increment (Invoyer.DataAccess.ClientTest)

    Assertion with == failed
    code: Counter.get() == 0
    left: 1
    right: 0
    stacktrace:
      test/counter_test.exs:6 (test)

.

Finished in 0.02 seconds
3 tests, 1 failure

Randomized with seed 196352
~~~

What happened?!? Everything was working when we pushed. I wonder who
broke the code? Hold off, cowboy, it isn't a good idea to start pointing
fingers, but we can check `git praise`. Hmm, it looks like the last
commit was us. Let's run the tests locally a few times.
times produces a different result. This is Elixir and it can't be a
threading issue. Now what do I do?

~~~bash
amos@Noggin:~/workspace/$ mix test
...

Finished in 0.02 seconds
3 tests, 0 failures

Randomized with seed 565233

amos@Noggin:~/workspace/$ mix test
.

1) test increment (Invoyer.DataAccess.ClientTest)

    Assertion with == failed
    code: Counter.get() == 0
    left: 1
    right: 0
    stacktrace:
      test/counter_test.exs:6 (test)

.

Finished in 0.02 seconds
3 tests, 1 failure

Randomized with seed 296355
~~~

As we dig in we start to realize that we have one named agent that is
being utilized for every test. Since we have one agent storing the
state for multiple tests there is a posibility that the tests are
stepping on each other. It looks like our incementation test is
incrementing the value before our inital `get` test is running
assertions. How do we fix this?

We could start up the agent we need in the test and make
sure we run our specs with `mix test --no-start`. Then we would have to
change the build and let the rest of the team know how to run the build.
We might have to change multiple tests later to handle their own
processes. We might be able to tag some tests and run some that start
the application and others that don't. This all seems quite complicated
and has a lot of cognitive overhead for the long haul. There has to be a
better way.

I guess we are just going to have to give it a name with every call, but
that is going to be annoying and we don't want to deal with that.

What if we inject a name just for our specs? We defualt the name in out
application, but we allow some options to be passed into our counter
that include a name. In the future we could kick off another counter
with a name too, but that is YAGNI for now. Lets try it out.

First we update our specs to pass along a name at each point. Let's use
the test module as the name of our counter. That way debugging is
simple. We will have to add a setup that starts a new agent and passing
in the name of the agent. We don't want to forget that our setup needs
to return `:ok` since we don't need any of the state to be passed along.
We also need to make sure each spec is using the same agent. We can just
pass in the name to our `get` and `incement` functions.

~~~elixir
defmodule AgentTest.CounterTest do
  use ExUnit.Case, async: true
  alias AgentTest.Counter

  setup do
    Counter.start_link(name: __MODULE__)
    :ok
  end

  test "initial value" do
    assert Counter.get(__MODULE__) == 0
  end

  test "increment" do
    Counter.increment(__MODULE__)
    assert Counter.get(__MODULE__) == 1
  end
end
~~~

Running our tests now leads to many failures because we don't have any
matching functions. So, lets go ahead a fix those. We will first pull up
the `__MODULE__` to an attribute called `@name`. This makes the code
look a little nicer and lets us easily change the default. Next we need
to make sure that we provide the name as a default to all our functions
except `start_link`. Since `start_link` will take an options map we will
use `Keyword.put_new/3` to good work in setting up a default name for
us.

~~~elixir
defmodule AgentTest.Counter do
  @name __MODULE__

  def start_link(opts \\ []) do
    opts = Keyword.put_new(opts, :name, @name)
    Agent.start_link(fn -> 0 end, opts)
  end

  def get(name \\ @name) do
    Agent.get(name, fn(count) -> count end)
  end

  def increment(name \\ @name) do
    Agent.update(name, fn(count) -> count + 1 end)
  end
end
~~~

With all this code in place we run our tests one last time, or fifty
times because we are scared of the Heisenbug coming back.

~~~bash
amos@Noggin:~/workspace/$ mix test
Compiling 1 file (.ex)
...

Finished in 0.02 seconds
3 tests, 0 failures

Randomized with seed 1238712

#...
~~~

Everything seems like it is working well. We better push this up and
then share our new knowledge with the rest of the team.

The actual code can be found on
[github](https://github.com/BinaryNoggin/agent_test) and if you would
like here is the [screencast](https://youtu.be/gfI4LVS7pqM).
