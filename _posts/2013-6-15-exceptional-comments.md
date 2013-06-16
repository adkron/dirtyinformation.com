---
layout: post
title: Exceptional Comments
---

Once upon a time I was pairing with a friend. This friend really wanted
to put a comment in our code. I explained that I thought if we needed a
comment that we had done a bad job of letting our code speak for itself.
He claimed that comments should be used to explain the 'why' and not the
'how.' This is why his comment was ok. I still disagreed and we quickly
degraded until I made him frustrated enough that he went home. This
was not my proudest moment.

Recently, as he does once in a while, he brought up the comment
discussion again. This time he used this gem of a tweet:


>   Good code comment (how else could you convey?): # Let this raise its
>   exception if the fields don't exist as expected.
>   [@Adkron](http://twitter.com/adkron)
>   [@jessitron](http://twitter.com/jessitron)
>
>   [@Adkron](http://twitter.com/adkron)
>   [@jessitron](http://twitter.com/jessitron) Uh, no. It's not trying to raise an exception.
>   Just a reminder that it might. The code: user =
>   c.get_setting('username')

Well the comment quickly degraded into a discussion about exceptions.
Many of us agreed that there are much better ways to handle most issues.
I'm not saying that you should never use an exception so you can put
away your torches and pitchforks. I will say that most people are using
them wrong. Ok, I'm not really protecting
[CraigBuchek](http://twitter.com/CraigBuchek) since you can just google
for him. Maybe you should so you can see how this conversation went to
the point where many of us decided to write blog posts. As they write
them I will update this post to links to their posts.

Like most examples when we are trying to prove a point, I'm sure Craig
didn't mean for his example to be picked apart about its finer points. I
still can't help but place judgment on the point he tried to make in
less that 140 characters and with only seconds of thought.

So let's take these two tweets and combine the comment with the code.
This way we can view it in all its glory.

```ruby
  # Let this raise its exception if the fields don't exist
  user = c.get_setting('username')
```

WTF! Sorry Craig, but I'm going to change from talking about how crapy
this comment is and I'm going to talk about alternatives to throwing
this exception. I won't even touch the fact that you only took [8
seconds](http://en.wikipedia.org/wiki/Rodeo)
to think about this. I'm going to say the whole method sucks.
First, how many possible settings are in this thing? Is it like a big
hash object? Did we have to make the method so generic? I'm going to
assume that you think like I do in 8 seconds and that you really meant
this:

```ruby
  # Let this raise its exception if the fields don't exist
  user = c.get_user('username')
```

It is still horrible, but we'll bear with it. We need to make small
refactorings.

First reason not to use an exception is that this doesn't seem like an
EXCEPTIONAL CIRCUMSTANCE. I'm going to make another leap of faith and assume that
'username' is not really hard coded and it came from user input. User
input is going to be bad. This is never exceptional. Now you are just
using an exception as [flow
control](http://c2.com/cgi/wiki?DontUseExceptionsForFlowControl).
Actually read the last link. I think it is a good one. Then just go
ahead and read the rest of the [C2 wiki](http://c2.com/cgi/wiki).

Return Nil
----------

Ok I'm going to assume that we are using something that returns nil,
I already hate it, or pulls a user from something.

```ruby
  def get_user(username)
     #...wicked code that finds a user or returns nil
  end
```

I think that I can dig into why returning nil is bad, but that is
another post. I still think this is better than an exception. I believe
that all APIs and languages should use the principle of least suprise.
Throwing an exception and unrolling the stack is a suprise. If you are
reading the code utilizing this, it is not immediately known that this
can throw an exception. I guess that explains Craig's comment. Although
I also don't want to see a comment on every piece of code that throws an
exception.

You see the problem is that comments are lies waiting in the wings.
First every time I write the line of code I have to make sure that I add
the comment. Then when I make it stop throwing exceptions I have to
change all the comments. Most likely they won't get changed. Since they
don't run I will never know. The next reader will think there is an
exception when there isn't. You really should have written a test
instead of a comment. You could also make the name of the method say
that it throws an exception. That is enough of a side track on why
comments suck.

Now the code utilizing this needs something like the following:

```ruby
  user = c.get_user(username)
  if user
    # blah
  else
    # boo
  end
```

That is no better than a try catch. If we don't have that code then we
can possibly get a null pointer exception. The problem with null pointer
exceptions is that the cross boarders of the code silently. Insert very
offensive boarder crossing joke here. Often a null
pointer error occurs 15 calls away from where the null originated. These
can be frustrating to track down. Even more so if you have
chained.method.calls all over your code.

Let's try something different.

Return an Error Designator
--------------------------

Ok I'm going to start only placing the error case in the original
method. It can no longer find users. This is a great frist test case
when you are doing TDD anyway.

```ruby
  def get_user(username)
    :user_not_found
  end
```

Well we've fixed the problem with a null pointer exception. At least now
when we get the exception it will mention :user_not_found and we will be
able to trace it to which thing.in.our.chain.is missing. Although we are
still going to have that 'if' and it looks suspiciously like a begin...rescue.

```ruby
  user = c.get_user(username)
  if user != :user_not_found
    # blah
  else
    # boo
  end
```

So we changed one if for another, but we got something a little nicer
when we do get exceptions.

Null Object Pattern
-------------------

Traditionaly, I've been told, the [Null Object
Pattern](http://c2.com/cgi/wiki?NullObject) returns an object that
returns itself no matter what you call on it. I think that can be
useful, sometimes. Often I believe this could hide many issues. So I
want to return an object that represents the Null user. In fact let's
call it the NullUser.

```ruby
  def get_user(username)
    NullUser.new(username)
  end
```

I decided that we can pass the original username that was used to try
and find a user so that we can look into the object and find out what
user wasn't found. The nice thing about an object is that it can answer
the phone for the user. In fact it is a user. It is just a special kind
of user. Now our code using the user can receive the messages intended
for a user. The best part  NO IFs.

```ruby
  user = c.get_user(username)
  # blah user.first_name
```

Now we can use this user to our hearts content. We can handle calls to
it in a uniform way. When there really is no way to handle a certain
call we can throw an exception from that specific case. I think
this puts us in a better place, and allows us more chances to make
decisions and expand functionality.What happens when we want a guest
user? Well we just add that funcationality to this class. We also passed
in a username. So if a user isn't found in the sytem we can give them
limited access through a special NullUser, but we can still call them by
a username since we tried to find them already. Remmeber the NullUser
was initailized with the failed username attempt.

With this approach we have less error handling code, and we make sure
that we are handling all cases of the missing user in the same way
without duplication. This is because we have wrapped up how to handle a
missing user by creating that class. Infact maybe we should refactor
NullUser to be named MissingUser or GuestUser.

In case you didn't notice, or have never heard of it, this Null Object
Pattern is making heavy use of [Poly
Morphism](http://c2.com/cgi/wiki?PolyMorphism). I thought that was one
word until I just tried to look it up on the C2 wiki. Ok so maybe it is
one word, because I know we just [Replaced a Conditional with
Polymorphism](http://c2.com/cgi/wiki?ReplaceConditionalWithPolymorphism).
Oh well, that debate will have to stand on a different day.

Other Submissions to the Debate
-------------------------------

[Mario Aquino - Styles of Handling Conditional
Logic](http://c2.com/cgi/wiki?ReplaceConditionalWithPolymorphism)

[Jessica Kerr - What's dirtier than comments?
Exceptions!](http://blog.jessitron.com/2013/06/whats-dirtier-than-comments-exceptions.html)

[Yin Wang - Null Reference is Not a
Mistake](http://yinwang0.wordpress.com/2013/06/03/null/)

Let me know if you have any submissions to this fun foray.
