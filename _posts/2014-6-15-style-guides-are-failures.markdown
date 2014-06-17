---
layout: post
title: Style Guides are Failures
---

Style guides are great for aesthetics, but fall short for what teams
really need. Your team, future you, and the next developer to join your
team thrive on communication of intention, and not the color of your
brush.

The issues with a style guide start with the name that generates a
certain intention. Style makes the style guide sound more like a
personal touch. It is more about appearances than communication.
The style guide is there to let you know what color paint to use and how
wide the brush. It is more of a micromanaging arrangement where you are
given what the ending should look like, but you don't know what the
painting is trying to say. This personal touch of a style guide leads to
teams arguing over what is the best way to do X. The arguments are all
about personal feelings. There is a place for this, but a change of
direction might reduce the fights and increase communication.

Often a change in thought comes with a change in vocabulary. If the
intention of the guide is to communicate then let's start by replacing
style with communication. The easy part is over.

Next, it is time to go through our old style guide and start asking
ourselves "why?" Each decision that can be done in multiple ways needs
to have a communicating reason that doesn't reach into what the look of
the code. Looks are important, but your choices need to help the code to
communicate more. Let's take an example from the ruby world.

```ruby
  #Style Guide
  #blocks

  #single line blocks
  method { |foo| ... }

  #multi line blocks
  method do |foo|
    ...
    ...
  end
```

The why here is based on looks, and not on communicating intention or
anything else. The hardest thing to communicate is intention. We are
communicating the number of lines, but that should be pretty clear
by counting. This also introduces incidental change when we find out we
need to add another line.  So what can we do better? I'm going to reach
back to the great [Jim Weirich](https://en.wikipedia.org/wiki/Jim_Weirich)
for this example.

```ruby
  #Communication Guide
  #blocks

  #functional blocks of code
  config = File.open("config.yml") { |file|
    YAML.load(file)
  }

  #procedural blocks of code
  File.open("foo.txt") do |file|
    file.write("data")
  end

  #in specs
  #setup blocks
  before {
    #...
  }

  #specifications
  specify do
    #...
  end
```

Jim was great about making communication more significant than aesthetics without
losing the appeal of the viewer. Another example is
his use of [fail vs
raise](http://rubyrogues.com/151-rr-the-jim-weirich-tribute-episode/).

Let's all take a lesson from Jim and spend sometime asking "why" when we
make decisions to have a certain style in our code. Squeeze every drop of
intention out of every character your language allows. Are there any
others you can think of?
