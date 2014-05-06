---
layout: post
title: Just the Tip
---

When I'm working in a new code base, learning something new, or just
wanting to play irb is my tool of choice. Like [Ernie
Miller](http://erniemiller.org/2014/02/05/7-lines-every-gems-rakefile-should-have/)
I find it useful when exploring my own code too. Pairing with
others I've picked up a few tricks about irb. Many "ticks" I find are
unknown to a large contingent. It amazes me to find developers who
have been exploring Ruby for years, but when it come to irb they've
only touched the tip of the iceberg. Let's look under the water.

I type an exploratory statement into irb only to find out that I need to
explore the output. Instead of reentering the entire statement there is
a much better way.

~~~ruby
  irb> Ocelot.where(cost:500, name: /hex/).limit(100).order(:age, :asc).skip(20)
  # tons of output that I want to play with
  irb> ocelots = _
~~~

Before I was shown the `_` my exploration was ridden with time consuming
arrows. The `_` is a one character time saver that makes me smile every
time I get to share it with someone new.

The second trick I learned was a
[happy little accident](https://en.wikipedia.org/wiki/Bob_Ross). I
thought I was in the terminal and I used `CTRL-R` to search back
through previous commands. If you have never used `CTRL-R` in the terminal
this is a double [whammy](https://en.wikipedia.org/wiki/Press_Your_Luck).
In either the console or irb pressing `CTRL-R` will then allow you to
search your command history. If you didn't find the droid you were
looking for repeatedly hit `CTRL-R` to cycle backwards
through all the matches.

This last one I know will make my [pact](http://wiki.boochtek.com/pact)
[partner](http://blog.boochtek.com/) smile. I always forget about this
trick, but it can be a big time saver when working with the same object
over and over. Normally when you are in irb `self` is set to `main`.
`main` is an instance of `Object` acting as your context. Every
command you enter in irb is evaluated inside of `main`. So let's
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

Most methods will now be called in the context of your
object. Note that `==`, `+`, and others will not work. They don't even
work if you add parenthesis. I guess we can't have it all. To get back
up to the `main` object type `exit` or (bonus) hit `CTRL-D`. _Note: You
can use `self == 1`_

I wish I could remember who showed me all of these, but alas the
credit goes to the proverbial "they." I hope these little tricks help
speed up your exploration. I almost forgot the next time your boss
comes by while you're "playing" in the console hit `CTRL-L` before
getting caught.

