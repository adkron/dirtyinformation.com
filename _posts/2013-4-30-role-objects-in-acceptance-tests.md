---
layout: post
title: Role Objects in Acceptance Tests
---

I'm a big fan of utilizing [ page
objects ]( https://code.google.com/p/selenium/wiki/PageObjects ) in
acceptance tests. Page objects are used to model sections of your UI
and minimize [ duplication ]( http://c2.com/cgi/wiki?DontRepeatYourself )
within your specs. They also codify the actions which can be taken on
specific pages. This minimizes how many files must be updated by
encapsulating the details of how and where actions occur. All of us would
agree that these are great things to have in your tests and code, but
many of us are missing another hiding spot of behavior.

# In Comes the Role Object

A role object, in this context, is an object that takes on the part of
a system user. These objects maybe an admin, a buyer, a seller, or any
other user of your system. Just as page objects codify the actions that
are taken on a page, role objects codify the actions which a specific
role can perform.

```ruby
  buyer.login
  seller.login

  seller.list_item(@item)

  buyer.purchase(@item)
```

Through these abstractions a nice testing api emerges and the details of
where and how are no longer a concern. In order to find out what actions
are available to a buyer there is only one place to look. How about a
seller? Still one place to look.

The power that you gain is not only in refactoring and organization, but
you are now easily allowed to utilize
[polymorphism](http://en.wikipedia.org/wiki/Polymorphism_%28computer_science%29)
in your step definitions. Setting up a context in given steps allows _When_ steps
to no longer care which role is performing the action. In addition, the _When_
step now no longer needs to care about the differences between how each role
performs the action.

```ruby
  Given "I am a buyer" do
    @user = Buyer.new
  end

  Given "I am a seller" do
    @user = Seller.new
  end

  When "I login" do
    @user.login
  end
```

Let's use the power of OO Design in our testing.  If duplication is found
between these role objects there are many OO design techniques which can
be employed to fix that issue. Code is there to communicate and tell a
story. Tests are used to communicate and tell a story. So let's make sure
that we are clear and concise when telling that story. Don't let yourself
get hung up in gritty details and duplication. You may also find that
roles pop out in your head and help you think of new actions. Maybe you
create a PowerUser role that utlizes keyboard shortcuts instead of
clicking links. The only thing that needs to change in your scenario is
the _Given_ step and you have multiple paths through your UI.

_DISCLAIMER_:

As with all forms of abstraction you have to live with the
levels you create.
