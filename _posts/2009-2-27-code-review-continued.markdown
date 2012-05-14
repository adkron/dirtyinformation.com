--- 
layout: post
title: Code Review Continued
---
<p>
I just sent my coworker and email tried out some of the ideas out of my post from two days ago.  I sent it to him with a little blurb at the top saying that I'm trying out a new method of code review, and I would love his feedback on it.  I would also like some feedback from all of you.  Should I word parts differently?  Should I scrap the whole idea, and start over?  Where can I improve?  Do you have any techniques that you find useful?
</p>

<p>
The files and names have been changed to protect the innocent.
</p>



<pre>
FooController

Lines 235, 236, 237 is there a reason why we are raising exceptions here?
When finding by id why did you choose ActiveRecord::Base#find and not ActiveRecord::Base#first?
Is there a reason for choosing a polymorphic relationship and not Single Table Inheritance(STI)?
How could add_comment(265 to 282) differ if you used STI?
Is it possible to utilize both polymorphic and STI, are there any advantages to doing that?

CommentNotificationTemplate

Line 13 - Any reason why this isn't using some kind of url helper method?
Line 13 - Do we need this line at all if there is a helper method to be used in the template?
Lines 28 - 30: Why is this static method even around?

html.erb
Line 12 - Can we use a helper for this?  Or make a route?

text.erb
Line 8 - Same questions about the url that is assigned in the controller.

In the tests there is a factory being used, but everything the factory includes is overridden.  Is there a reason?

The story template test could really benefit from a url helper.

Francis, this is great work.  I love to see that you got rid of the comment fixture.  I think this is a big step in the direction we need to go.  Your code is getting better all the time.  Keep up the good work.
</pre>



<p>
The follow question was used to provoke thought from the reviewed "is it possible to utilize both polymorphic and STI, are there any advantages to doing that?"  I just want him to think with that one.  Is this a workable tactic?  I would love to help him continue his amazing growth.
</p>
