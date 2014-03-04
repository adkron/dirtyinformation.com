---
layout: post
title: Lost in Translation
---

I have had a few conversations about how to design Rails controllers
over the last seven years. I've also heard a lot of really great
developers say, "Why aren't controllers extracted away by now?" This
problems seems to stem from the ActiveRecord API. Once there is a tool
that takes a hash it can do anything. With this hammer great developers
go drive nails, screws and fluffy bunnies. Just in case that doesn't
grab you take a look at these two controller methods I found in the same
application.

```ruby
  def update
    article = Article.find(params[:id])
    article.update_attributes params[:article]
    respond_with article
  end
```

That one was straight forward enough.

```ruby
  def update
    article = Article.find(params[:id])
    article.update_attributes params[:article]
    respond_with article
  end
```

Oh, that one two. Make sure to take a really close look at those two
methods. Can you tell what the difference is? Can you tell me what they
do? Hint: I copy and pasted them. They have the exact same code. So why
do they appear in two different places in the code?

Oh, I know, we have this hammer so we can just abstract the controllers
away and they should all look like this! Then all the nails are taken
care of. Oh, and the screws. We mustn't forget about the bunnies that
are now bleeding from the hammer smashing them to bits.

I want to talk about the security flaw in this code, but I think we
should discuss what they do first. These two controllers had very
distinct responsibilities and where used by two distinct types of users.
Can you tell me what the responsibilities of these controllers are? How
about I give you the names of the controllers? The first one is the
`ArticlesController`, and the second is the `SubmissionsController`.
Wait that isn't right at all. The first is the `SubmissionsController`
and the second...Well I think you get the confusion here.

In order to try to make this code a little more clear the team working
on it wanted to add this to the code:

```ruby
  class SubmissionsController
    Submission = Article

    def update
      submission = Submission.find(params[:id])
      #...
    end
```

They thought this would provide some expandability in the future if
Submissions and Articles became separate classes. I really appreciated
their ability to find the noun, but I also wanted to cry. Do you know what
these controllers do with that change? Neither did I.

You see there is nothing to reveal intention in this code. If you've
known me a while you will also know that I am not one for comments,
which the team also had in place. Oh, and the comments weren't quite
accurate we found out.

Here is the code that I ended up writing.

```ruby
  def update
    article = Article.find(params[:id])
    article.submit(params[:article][:title], params[:article][:body])
    respond_with article
  end
```

```ruby
  def update
    article = Article.find(params[:id])
    article.publish_on(params[:article][:published_on])
    respond_with article
  end
```

There is still a little repetition, but this is a very basic form. I
didn't go find the team I helped and ask them for the code. Now can you
tell me what these controller do?

The first controller is used by a journalists/community to submit
articles/letters to the editor to a newspaper. The second is used by
the editor to accept and pick a publishing date for the article.

The change above also solved a security issue that they were a little
concerned about. The original code had white listed attributes in the
Article class. This was great until they realized that with a little
params hack someone could publish an article when they submitted it. The
whole point of the software was to make the work flow of getting news to
the presses.

ActiveRecord has a very broad interface. I don't know if it was [Avdi](http://devblog.avdi.org/) or
[Ernie](http://erniemiller.org/) who said it is an infinite interface. This is very true in methods
like `update_attributes`. The intention of its use is determined by its
inputs. This leads to a lot of power, but lacks fine grained controls.
This is why the Rails core team had to implement [strong parameters](http://weblog.rubyonrails.org/2012/3/21/strong-parameters/). I
think these are great in the battle field of security, but still not
showing the intention of our code.


After many examples like this I'm asking you to put away the hammer.
Pull out your screwdrivers, air guns and multitude of other tools. Pick
the one that reveals what it is for. Hide ActiveRecord away as soon as
the prototyping is over. I'm not saying to not use ActiveRecord, but
treat it like a powerful set of private methods.
