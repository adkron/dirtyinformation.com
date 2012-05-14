--- 
layout: post
title: Where To Test What
---
<p>
<img src='http://dirtyinformation.com/assets/2008/11/4/testing_thumb.jpg' alt='volt meter' style='float: right;'/>
Testing is very important to all of us, but where should our tests go, and what should they test?  When jumping into an existing Rails application you should run all the tests, and then start looking through them.  Developers should have certain expectation of where certain things are tested, but many tests aren't where they should be.  So where should they go in a Rails application?
</p>
<p>
Unit tests are designed for you models.  This doesn't seem to be a problem for most applications.  There is a well defined line here.
</p>
<p>
Functional tests are for testing controllers.  This line is a little less defined.  The interactions between the controller, the model, and the view trip people up.  The biggest problems fall in testing the view, and retesting the model from the controller perspective.
</p>
<p>
Model tests are not functional test.  When you test the path of a validation for instance.  It has already been tested in the unit test so stop and delete the test.  Think about what the controllers actual job is.  Test one happy model, and one sad(failed validations) model, but don't retest every single model validation through the controller tests.
</p>
<p>
STOP.  Don't test your view here.  The occasional assert_tag or assert_no_tag isn't bad, but remember that isn't really testing the controller.  Minimize what you are testing to the class under test.  This way the view can change styles and all your functional tests are still only testing the controller itself.
</p>
<p>
Integration testing is where the views should jump into the action.  The integration tests should drive the application through the view and assert the interactions and the correct information is presented to the user.
</p>
<p>
<a href='http://github.com/thoughtbot/shoulda/tree/master'>Shoulda</a> is a great testing framework that will help guide where things should be tested.  The should methods can be your guide to testing.  Then find a great integration test framework like <a href='http://github.com/brynary/webrat/tree/master'>Webrat</a>.  Get all these in your application and start writing tests where they belong.
</p>
