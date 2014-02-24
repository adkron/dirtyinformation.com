---
layout: post
title: Story Splitting Revisited
---

A few weeks ago [@joescii](http://twitter.com/joescii) and I started
tweeting each other about splitting stories into smaller chunks. This
lead to a [blog post by
me](http://dirtyinformation.com/blog/2014/02/02/smaller-is-better/).
Then came a full episode of [This Agile
Life](http://www.thisagilelife.com/35/), and another blog post by
[Joe](http://proseand.co.nz/2014/02/20/i-spammed-amos/) who apparently
took the episode's advice and spammed me. After all this there should be
almost nothing left to say. That is what I thought, but after reading
Joe's post I thought I would take my own crack at splitting up the story
and then Joe and I could SPAM the world a little more while we compare
notes.

Joe's article is all about splitting a larger feature into smaller
stories and what those stories became. The large feature, as Joe
described it:

> As an engineer, I would like the ability to page through the results.

A simple example of paging a list of results. If you've been doing
development for any length of time you've more than likely had to deal
with this exact situation. We'd probably say, "That is good enough. I
think we'll stick with that story and move on." Really what you are
thinking is, "I know all there is to know about paging, and I am tired
of sitting in this meeting." What you really should be saying is, "I
want to give my customer great flexibility. QA and Code Reviewers should
have something a little more focused. If that new kid has to work on
this I hope he doesn't miss something important."

That high level feature leaves a lot of interpretation surrounding what
exactly the functionality is that the customer wants.

Before we move forward, please go read Joe's
[post](http://proseand.co.nz/2014/02/20/i-spammed-amos/). The rest of
this I think will be a lot better if you understand the back story a
little more. I'm coming into this with about the same level of knowledge
that you just got out of Joe's post. In the words of [Peter
Pan](https://en.wikipedia.org/wiki/Peter_Pan_(1953_film\)),
"Here we go!"

```Cucumber
  Given a search result containing a large number of pages
  When the current page does not display what I am looking for
  Then I can move to the next page of results

  Given a search result containing a large number of pages
  When I pass the page that I wanted
  Then I can move to the previous page of results

  When on a search result containing only one page
  Then only one result page is available

  Given a search result containing a large number of pages
  When browsing the results
  Then the total number of result pages is diplayed

  Given a search result contaning a large number of pages
  When browsing the results
  Then I can tell what the current page is

  Given a search result containg a large number of pages
  When the current page does not display what I am looking for
  Then I can jump to any result page
```

One of the scenarios doesn't have a Given, Amos.

That was intentional. Not every scenario needs a gigantic setup or a
whole lot of action before getting where you need. I try to make my
stories concise.

You may also notice that I tried to capture a little of the "why" within the
scenario. I also tried to leave a little of the how up to the person
creating the final product. This allows a little expandability within
the scope of the application. Maybe the user as buttons and page numbers
all over the page? Maybe there are keyboard shortcuts? How about mouse
gestures like the Mac? By leaving the story a little vague in that area
we are allowing for a whole host of ideas to come out. I guess this
means that I'm concise, but not precise.

If you are making these Scenarios executable you could execute each story
under multiple "personae" and each one uses a different idea from the
above. You could have a power user persona that uses keyboard shortcuts,
a mouser who uses mouse gestures, and a clicker who loves the buttons. I
would suggest each persona also be its own story. I think I just jumped
to a new topic? I guess we'll save that idea for another day.
