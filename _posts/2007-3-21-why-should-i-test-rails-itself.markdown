--- 
layout: post
title: Why Should I 'Test Rails Itself?'
---
<p>
I was reading Google Groups' Rails Group the other day when I ran into someone talking about why should I test the validates functions.  Here is a quote from his email:
</p>

<blockquote>
For example, what is the point of writing a unit test that simply
tests a validate statement in the model? Yes, it's interesting (the
first time, at least) that the test works and that, golly-gee, Rails
works as advertised, as well, but is there any real use in doing this?
<br/>
I can imagine a scenario where a I update Rails and suddenly a unit
test that tests a Rails validator fails, but I expect the Rails
development team would find this before me. Or is that too naive? 
</blockquote>

<p>
I just thought I would post my response for anyone who has the same question.  Also, sorry if the format is bad or to quick, but I was trying to hurry when I wrote him the response.
</p>


<blockquote>
<p>
Don't look it as testing Rails.  You are testing your model on testing validates.  Let's say you write this class:
</p>

<pre>
class Person < ActiveRecord::Base
  validates_length_of :phone_number, :within => 7..10
  ...
end
</pre>

<p>
So you write a test:
</p>

<pre>
def test_phone_number_length_incorrect
  person = Person.new(:phone_number => '123456')
  person.valid?
  assert person.errors.invalid?(:phone_number), 'Invalid Phone Number Coming Back as Valid'
end
</pre>

<p>
I think we would all agree that this test makes sure that a valid phone number passes.
</p>

<p>
Now not thinking you do a find and replace and replace all 7s with 8s or maybe number with numbers is more likely.  So you have:
</p>

<pre>
class Person < ActiveRecord::Base
  validates_length_of :phone_numbers, :within => 7..10
  ...
end
</pre>

<p>
Now you can place :phone_number => '12' into your database because you are now validating :phone_numbers.
</p>

<p>
If you have a test you run tests after you completed the find and replace, and this error would be caught right away.  That is why you 'test Rails.'
</p>

</blockquote>
