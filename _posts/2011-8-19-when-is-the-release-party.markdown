--- 
layout: post
title: When is the Release Party?
---

Releasing a new product can get a person into a flat spin and make their head hurt like crazy. The last two weeks has been a whirlwind of excitement and nausea. I was thrown head first into the first time I have ever been the person in charge of a production release of something someone else was paying me for.

We set out on this journey almost a year ago. The client has been up and down in what they wanted. We have a lot of crufty code, but things have been looking up lately. Everyone is excited to get something out to the public. The developers desperately want feedback from real users, and a stronger guiding voice than the wishy washy customer we have had.

h2. Monday - the official day we are supposed to release…

“WTF? This has never happened before. What is going on?” Certain interactive data seems to be missing small parts here and there. The worst part is that there seems the be no consistency. When we try to replicate the issue everything is fine. It looks like we are going to need another day to figure this out. I hope that it is just this one day.

Chase down some issues with connecting to the database. We believe this is a hosting company issue. We can let them deal with it, but right now we need to add some logging so we can tell when and where this is happening.

* think about logging before there is a problem

h2. Tuesday - crossing our fingers…

Rushing into the room I shouted, “I understand what happened. Mongoid is not performing atomic operations on arrays so when two users touch an array at near the same time, the second one wins the update.” “That makes no sense,” the others say rolling their eyes. “Have you tested this?”

“No,” I sigh. “I just know that is what is going on. I’m looking through the code, and timestamps and it is only happening when people are both touching the same data. I also looked at the db logs and the queries on array operation are completely overwriting the arrays and not using the atomic operations.”

“We believe that is an issue, but there has to be something else going on. We’ll fix what you are talking about and then see what happens.” No one believes me, but at least they are willing to prove it, and go along with the fix. The tests took some time, but after the first test we flew threw the rest of the code. The code slims down and gets faster with the fixes in place. Now we just have to wait and see.

* test users interacting in parallel
* watch for data that can be touched by more than one user at a time
* don’t say “Your idea makes no sense.” Nicely sound surprised and ask them to show you.

h2. Wednesday - About time…
The beta site is launched. People start signing up and giving us feedback. There are a few tiny bugs found and squashed. We can finally start to plan for the next release, and clean up some of the technical debt we took on to meet this aggressive deadline.

First, make the rescue jobs take in the data they need, and not pull from the db. This will make sure that even if the db is down that the background emails can still go out. This was done quickly, and actually fixed a 404 issue that was coming up.

On second thought we should turn our logging back down, first. Not only do we know it is slowing things down, but it is annoying when we are trying to look at just errors that may appear in the logs.

* too much logging can be annoying
* logging slows things down more than you think
* give background jobs all the information they need up front, and try not to have them touch the db

h2. Thursday - I can’t believe God made it 6 days before resting…
I’ve put in close to 100 hours in the last week and a half. My family is missing me and I should really take a day off. I took Wednesday through Friday off, but of course I can’t keep my nose out of it. I am still talking, planning, and working on the site. Most of the things are not that important, and I know the team is smart and can function without me.

I have a lot of pride in my work, and the work of the team. It is so hard to walk away while they are still hard at it. I know they weren’t working over the weekend, and I was, but this is my team. They need me, or do they.

The team gets through the day, mostly on their own. I only shoved my nose in for my own ego.

* you made sure your team was filled with smart, self motivators for a reason
* let your team shine
* if something goes wrong there is always a way to fix it later

h2. Friday - customers, users, and project managers can be a pain…
The product manager seems upset because people decided to take the day off or to work from home. I’m happy that someone is finally resting. The customers want something changed ASAP, and all the developers are at home.

Isn’t it always ASAP? It is a small change, but was something that they specifically asked for, and now are realizing how misguided the request was. That really is my fault.

One developer working from home can’t get in to the server to deploy. His ssh key is on the jump box, but he can’t get to the production servers from there. Now a team member calls me when they need me. I appreciate that I have had to do very little today.

Little is an understatement. My youngest daughter (3 yrs old) is throwing up and running a fever. Please don’t let me catch it.

* upmanage misguided decisions through the project manager or the customer directly
* the team will call you if they need you
* even success is met with grumpy bosses…don’t be one of them