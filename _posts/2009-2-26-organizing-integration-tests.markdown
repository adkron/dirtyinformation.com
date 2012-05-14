--- 
layout: post
title: Organizing Integration Tests
---
<p>
I've been in meetings all day so I don't have a lot to write about, but I have to keep up with my blog post a day.  So without further ado we need to organize our integration tests.
</p>
<p>
We've been trying to find out how to better organize our integration testing for a while.  We started out by having each test file organized by the controller involved.  This works fine for small controllers that aren't interacting with other controllers.
</p>
<p>
My next idea was to organize them by controller method.  That helped slim down a lot of the test files, and put them in a more readable format.  This was a great step but when pages have a lot of functionality this too can become a burden, and make test file become lng and unruly.
</p>
<p>
Tonight I began work on converting one such test file into functional sections.  We have a page in the project that has a lot of information, forms, links, et al.  I decided that the integration tests for this page should be broken up by functional areas.  Anything outside of a functional area will go into a test file for the overall page.  The overall test file will contain things like breadcrumb link tests, and tests for user access.
</p>
<p>
Right now we are using <a href='http://thoughtbot.com/projects/shoulda/'>shoulda</a> and <a href='http://github.com/brynary/webrat'>webrat</a> for our integration testing.  I'm hoping that soon we will have <a href='http://github.com/aslakhellesoy/cucumber/tree/master'>cucumber</a> in the mix.
</p>
