---
layout: post
title: Smaller is Better
---

"OMG, can we stop worrying about these stories and get to work?"

"Yeah, I'm tired of this."

"Really, another story?"

"Isn't this one small enough."

I know that we have all been here and we have been in meetings like
this. Someone on the team is pushing for tiny stories and we just want
to get to work. I agree with getting to work if you are using
[Continuous Flow](https://en.wikipedia.org/wiki/Continuous_Flow_Manufacturing).
Although that isn't saving you from breaking down your stories. They
should still be small so you should be constantly breaking down and
adding stories to your priority queue.

What is up with all this obsession over tiny stories?

Think back to the last few stories that your team was asked to estimate
and complete. How accurate where your estimates? I'm willing to bet that
your estimates where not accurate, or you had to cut corners to meet
them. How accurate is the last story that you estimated at two weeks?
How accurate was the last story that you estimated at two hours? I'm
not sure that I have to convince you that the two hour estimate is going
to be closer to reality. The further out that we have to predict
anything the harder it is to be right.

Small stories are far more focused. Let's just look at a small story,
shall we?

```
  Given I attempt to register
  When my email is not propperly formatted
  Then my registration cannot be completed
```

Now, I admit that this story could go into different format issues, but
I would stop here. This story is the right focused size. I estimate it
at less than a day. There isn't much left open to interpretation. There
is a little wiggle room on how the registration is not completed, but
there isn't room to run. It would be very difficult for the scope
creeper to rear his ugly head. Scope creeper says, "I just saw that we
need to make sure the user provides a password." You can respond with,
"Yeah, we should make another story and throw it in the queue and get
back to the EMAIL ADDRESS!" Then move on.

Code reviews are one of my favorite practices. I love that someone else
on the team is going to take a look at the code I wrote with my pair. I
also like to look at other code. This is a great learning experience for
all of us. Oh, wait I meant until the 12000 line diff pops up. Who wants
to code review 12000 lines. Are you more likely to look at it
objectively or skim right through? With small and focused stories it is
simpler to see the design and understand what trade offs might have been
taken. You probably even have a commit message that makes sense.

Your code made it past code review and it is on to QA to take a stab at
it. How many times has QA asked you why your story doesn't do x? How
many times has good functionality been held up because of a vague story?
With the focused story above QA can concentrate on what matters. How
many different ways can I not properly format and email?

The penultimate advantage to small stories is the flexibility
provided to the customer in planning and priority discussions. When small
pieces of functionality are constantly provided to the customer they can
more intelligently decide what the priorities should be. Let's say that
the original design of our registration had the user verify their email.
I know I love typing my email twice on every registration.

```
  Given I attempt to register
  When I fat finger my email
  Then my registration cannot be completed
```

This story would more than likely be prioritized much lower on the board
in a time constrained situation. We know that we are always time
constrained. Now we get it out the door and find out that very few users
are fat fingering and the customer would much rather do the game
changing feature that we are excited about. We are also so happy that
another customer didn't make our users jump through some arbitrary hoop
in order to use the product. We saved the day with a small story.

Now, the moment you've been waiting for, the ultimate fun in small
stories, throwing them away. Wait, what? Doesn't that mean we wasted our
time writing the story. NO! A story is a promise of a conversation. It
is not a promise of a delivered piece of functionality. Scope creeper
came in and said what about:

```
  Give I attempt to register
  When I don't provide an email
  Then my registration cannot be complete
```

When we get around to coding this we notice that this is already halted
by the email format story we already completed. The customer decides that
the error message we provided earlier is good enough for a blank email.
SCORE! We can now toss that story in the trash and get back to that game
changer.

One last and quick advantage. If we move forward with these tiny stories
we give plenty of room for parallelization to be done. One pair finishes
the email format while another team is checking the password length. We
just can't convince the customer to let the password be as long as
the user wants.

Please, take the time to make your stories as tiny as you can. It will
make the world a better place, one verification field at a time.
