---
layout: post
title: Open Closed Ruby Woes
---

Recently I received an email from a good friend, [Jessica
Kerr](http://jessitron.com/) about how the [Open/Closed
Principle](http://en.wikipedia.org/wiki/Open/closed_principle) is
applied in the Ruby community. It took me way to long to get back to
her, but this topic was going to take some time to properly answer.

The Open/Closed Principle is one of the most misunderstood [SOLID Design
Prinicples](
http://en.wikipedia.org/wiki/SOLID_%28object-oriented_design%29).
Misunderstood might not be the right term. Maybe I should say that it
puts most people into a nonplus state. Open/Closed isn't something that
most people strive towards when writing their code. I believe
Open/Closed is a place you end up when you've been working on a good
design for a while. After many passings of refactoring you get a class
into a position where it meets all of the other principles and suddenly
you notice that it is easily extended.

I guess I should take a moment to define Open/Closed in case you missed
the wiki links which tend to punch holes all over my posts. Open/Closed
is the that classes are open to extension, but closed to modification. I
think you might see why this is a little harder to notice than the other
SOLID principles.

Ok, now let's get back on track like nothing happened. Once your system
has been matured and refactored, with direction, many times you can find
classes in your system that fit this wonderful idea. This is really nice
when you find it, but there is a sweet spot. This is an easy principle
to take way too far, much like the [Single Responsibility
Principle](http://en.wikipedia.org/wiki/Single_responsibility_principle).
When is a class at just the right time to stop? When has it become
nothing more than a method class. When is there too much abstraction
(that should be another post)? When is this class able to be extended,
but doesn't require extension to not be trivial and a pain to utilize?
Now that I've pummeled you with questions I'm not going to answer any of
them. Enjoy!

In searching for more ideas about how to answer
[@jessitron](https://twitter.com/jessitron) I went back to the source
and asked her to clarify what she really wanted to know. She talked
about "has-a" vs "is-a" relationships and how we in the Ruby community
utilize libraries. This made me a little sad.

A large majority of our gems and libraries utilize inheritance as in
[ActiveRecord](http://api.rubyonrails.org/classes/ActiveRecord/Base.html)
or a
[mixin](http://www.ruby-doc.org/docs/ProgrammingRuby/html/tut_modules.html).
Sit down because this is going to be a big shock, mixins are
inheritance! Take a moment to get all of your arguments out of the way.
You can even email me about them if you feel so inclined.

Now that you've taken a deep breath an considered the way that Ruby
looks up methods you've realized that a mixin is in fact inheritance. I
personaly would like to see more libraries that are not utilizing this
way of thinking. It is sometimes convinent and great to work with, but
it can limit you and encourage [monkey
patching](http://en.wikipedia.org/wiki/Monkey_patch). This doesn't come
from any real gem, but I'm sure there is one out there that does just
this.

```ruby
class User
  include Geloationable

  # Other awesome codes
end
```

At this point out User class would have all the methods and powers to
handle geolocation, and this would include the idea of [latitude and
longitude](http://en.wikipedia.org/wiki/Geographic_coordinate_system).
It would contain methods to find distances between points and many other
geographical mappin tools. We have just expanded the responsibility of
our class and violated the open closed principle. I believe a much
better approach would be to have a Geolation class that handles all of
these responsibilities. Some will say that is what the above does, and
then delegates everything to the Geolocation class. Yes, that is
possible, but it still expands the User class and gives it a whole new
realm of responsibility. The User model would have to be changed to have
a Geolocation inside of it, but that is a much smaller change to the
original. Wait maybe instead the geolocation would have a reference to
the User, and then the User classes stays Closed. That thought just
occurred, right there. You just witnessed that.

The nice thing is that the geolocation class can now stand alone. It can
be used with more than just a User. It has a minimal impact on the
interface of a User.

Another approach would be to make a GelocationableUser class that
decorated the User. Please forgive me on my class naming. Maybe I shoul
d grab a [thesaurus](http://thesaurus.com). Using this approach you can
add a single method to the original User to give you back the wrapped
class. This is still changing the interface of User, but again it is
minimal. Gregory Brown has some more about this in his SOLID Design
Principles
[post](http://blog.rubybestpractices.com/posts/gregory/055-issue-23-solid-design.html).
I would take a read through it.

One last little bit. Before the end of our conversation Jessia asked,
"How do you devide your own code into concrete and abstract?" My mind
went immediately to abstract classes in Java. I'm not sure if it applies
to the post, but I'm going to give you a simple guideline that I
personally like. If I am going to use inheritance and the class can
stand on it's own I will utilize a class and note a module. If the
parent cannot stand alone and requires a child to implement another
piece of functionality in order to be used without throwing exceptions
everywhere, I will utilize a mixin. If it isn't an object in its own
right then don't make it a class. Would you be happy if you found the
Geolocation class from above and when you sent it a message it blew up
with a not implemented or method missing exception? Also I don't do the
not implemented route. I like to provide a module(Test::Unit) or a
shared example group(RSpec) that is included in the tests for the
extending class. These mostly test that the inheriter responds to the
proper methods.

Now it is up to you to find your own way. If it can stand alone is it
worth of being a class? Well, that depends on the situation. You will
know it when you've felt it enough times. You still have to strive for
it to truly feel it. Should you inherit? Should you decorate? Should it
be a property of another class? These I will also answer with "well...it
depends." Enjoy...
