---
layout: post
title: Just the Tip
---

When I'm working in a new code base, learning something new, or just
wanting to play I often jump into irb. Like [Ernie
Miller](http://erniemiller.org/2014/02/05/7-lines-every-gems-rakefile-should-have/)
I find it useful when exploring my own code. Although as I pair with
others I've picked up a few things here an there about irb. Many of
these "ticks" I find are unknown to most developers I run into. It is
amazing to find developers who have been exploring Ruby for years, but
when it come to irb they've only touched the tip of the iceberg. I
thought I would share a few tips.

I often find myself typing something into irb to explore and then
wishing I had saved the result to play a little more. I believe this was
the first of these tricks that I learned in irb.

~~~ruby
  irb> Ocelot.where(cost:500, name: /hex/).limit(100).order(:age, :asc).skip(20)
  # tons of output that I want to play with
  irb> ocelots = _
~~~

Before I was show the `_` I would have used the up arrow and then moved
the cursor back to the beginning of the line to add in the variable. The
`_` is a one character time saver that makes me smile every time I get to
show someone new.

The second trick I learned was by accident. I thought I was in the
terminal and I used `CTRL-R` to search back through what I typed before.
If you haven't done that this is a double
[whammy](https://en.wikipedia.org/wiki/Press_Your_Luck). In either the
console or irb pressing `CTRL-R` will then allow you to search through
your command history just by typing. After getting in your search term
you can repeatedly hit `CTRL-R` to cycle backwards through all the
matches.

The last one here I know will make my
[pact](http://wiki.boochtek.com/pact)
[partner](http://blog.boochtek.com/) smile. I always forget about this,
but it can be a big time saver when working with the same object over
and over. Normally when you are in irb `self` is set to `main`. `main`
is just an instance of `Object` that is there to be your context. So
every command you type in irb is evaluated inside of `main`. So let's
switch that up to make out life a little simpler.

~~~ruby
  irb(main):001:0> self
  => main
  irb(main):002:0> foo = "this agile life"
  => "this agile life"
  irb(main):003:0> irb foo
  irb#1(this agile life):001:0> upcase
  => "THIS AGILE LIFE"
  irb#1(this agile life):002:0> downcase
  => "this agile life"
  irb#1(this agile life):003:0> == "this agile life"
  SyntaxError: (irb#1):3: syntax error, unexpected ==
  == "this agile life"
    ^

  irb#1(this agile life):004:0>
  => #<IRB::Irb: @context=#<IRB::Context:0x007fefcb2aafe0>,
  @signal_status=:IN_EVAL, @scanner=#<RubyLex:0x007fefcb2a34c0>>
  irb(main):004:0>
~~~

As you can see most methods will now be called in the context of your
object. Note that `==, `+`, and others will not work. They don't even
work if you add parenthesis. I guess we can't have it all. To get back
up to the `main` object type `exit` or (bonus) hit `CTRL-D`.

I wish I could remember who showed me all of these, but alas the
credit goes to the proverbial "they." I hope these little tricks help
speed up your exploration. I almost forgot the next time your boss
come by while you're "playing" in the console hit `CTRL-L` before
getting caught.

