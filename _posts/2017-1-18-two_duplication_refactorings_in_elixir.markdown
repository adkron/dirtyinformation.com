---
  layout: post
  title: Two Duplication Refactorings in Elixir
---

The other day I was working on [Elixir](http://elixir-lang.org), and
preparing a [PR](https://github.com/elixir-lang/elixir/pull/5662) when
I noticed a little duplication going on in my PR.

```elixir
  @doc """
  Converts the given string to CamelCase format.

  This function was designed to camelize language identifiers/tokens,
  that's why it belongs to the `Macro` module. Do not use it as a general
  mechanism for camelizing strings as it does not support Unicode or
  characters that are not valid in Elixir identifiers.

  ## Examples

      iex> Macro.camelize "foo_bar"
      "FooBar"

  """
  @spec camelize(String.t) :: String.t
  def camelize(string)

  def camelize(""),
    do: ""

  def camelize(<<?_, t::binary>>),
    do: camelize(t)

  def camelize(<<h, t::binary>>),
    do: <<to_upper_char(h)>> <> do_camelize(t, h)

  defp do_camelize(<<?_, t::binary>>, _),
    do: do_camelize(t, ?_)

  defp do_camelize(<<h, t::binary>>, prev) when prev >= ?a and prev <= ?z and h >= ?A and h <= ?Z,
    do: <<h>> <> do_camelize(t, h)

  defp do_camelize(<<h, t::binary>>, prev) when prev == ?_ or prev == ?.,
    do: <<to_upper_char(h)>> <> do_camelize(t, h)

  defp do_camelize(<<?/, t::binary>>, _),
    do: <<?.>> <> do_camelize(t, ?.)

  defp do_camelize(<<h, t::binary>>, _),
    do: <<to_lower_char(h)>> <> do_camelize(t, h)

  defp do_camelize(<<>>, _),
    do: <<>>
```

The duplication is subtle, but if you take a close look you might notice it too.
There are two instances where the body of the function looks exactly the same.
Here it is with just the two copies that are the same.

```elixir
  def camelize(<<h, t::binary>>),
    do: <<to_upper_char(h)>> <> do_camelize(t, h)

  defp do_camelize(<<h, t::binary>>, prev) when prev == ?_ or prev == ?.,
    do: <<to_upper_char(h)>> <> do_camelize(t, h)
```

These were pretty far apart in that original code, and I probably would not
have noticed the duplication if I didn't originally have a third instance of
the exact same body of code. Originally instead of a guard clause on `do_camelize`
I had two different clauses.

```elixir
  defp do_camelize(<<h, t::binary>>, ?_),
    do: <<to_upper_char(h)>> <> do_camelize(t, h)

  defp do_camelize(<<h, t::binary>>, ?.),
    do: <<to_upper_char(h)>> <> do_camelize(t, h)
```

I went ahead and combined them with a guard since the meat of the algorithm did
not change from one method to another. This type of refactoring I call, "Combined
Pattern with Guard." This is something that I had noticed and done in a few
places in the past so it was immediately apparent to me.

After digging through and finding that first set of duplication I decided
to take a look through the bodies of the clauses and look for clauses
that appeared to be the same. Here is the duplication I found just one
more time.

```elixir
  def camelize(<<h, t::binary>>),
    do: <<to_upper_char(h)>> <> do_camelize(t, h)

  defp do_camelize(<<h, t::binary>>, prev) when prev == ?_ or prev == ?.,
    do: <<to_upper_char(h)>> <> do_camelize(t, h)
```

The two method bodies are identical. The second method head no longer
cared about `prev` and was just drooping it. It was not immediately
apparent to me what to do to "fix" the situation so I started to
rearrange code that worked with the two characters in the guard, `?_ ?/`,
and brought those together with `camelize` since it shared a method body.

```elixir
  def camelize(<<h, t::binary>>),
    do: <<to_upper_char(h)>> <> do_camelize(t, h)

  defp do_camelize(<<h, t::binary>>, prev) when prev == ?_ or prev == ?.,
    do: <<to_upper_char(h)>> <> do_camelize(t, h)

  defp do_camelize(<<?/, t::binary>>, _),
    do: <<?.>> <> do_camelize(t, ?.)

  defp do_camelize(<<?_, t::binary>>, _),
    do: do_camelize(t, ?_)
```

At this point I could see that any time that I had `_` or `/` I would be
restarting the algorithm like nothing else had been processed. This meant
that I didn't need to split the first character off the string in order to
capitalize it. I already had a piece of code that would do that for me. I
did have to take care of the conversion of a `/` to a `.`, but then it was
also starting over with a new split. This moment felt really good. I noticed
a loop in the algorithm that I had not noticed before. Here is the final
result of that change.

```elixir
  def camelize(<<h, t::binary>>),
    do: <<to_upper_char(h)>> <> do_camelize(t, h)

  defp do_camelize(<<?/, t::binary>>, _),
    do: <<?.>> <> camelize(t)

  defp do_camelize(<<?_, t::binary>>, _),
    do: camelize(t)
```

This change was so simple that I just redid it here instead of a copy and
paste. I was able to drop one clause, and simplify the others. It was nice
to be able to cleanly reduce duplication in FP as well as I could in OOP.
I hope these two refactoring help you find and eliminate duplication in
you code.

I want to thank [Devon Estes](https://github.com/devonestes) who made his
own [PR](https://github.com/elixir-lang/elixir/pull/5631). I wish I had
seen it before making mine. Also a big thanks to [Ekspreimental](https://github.com/elixir-lang/elixir/issues/5627#issuecomment-271154924)
for pointing out the issue in the first place.
