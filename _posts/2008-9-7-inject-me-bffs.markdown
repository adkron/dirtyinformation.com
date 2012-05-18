--- 
layout: post
title: Inject & Me - BFFs
---

I find myself turning to [Enumberable#inject](http://apidock.com/ruby/Enumerable/inject) more and more.  It is such a powerful method, yet I rarely see it used in others' code.  Here are a couple of examples of the power of inject.

h2. Adding Facorial to All Integers

'''ruby
class Integer
  def factorial
    raise 'Cannot take a factorial of a negative number' if self < 0
    return 1 if self == 0
    (1..self).inject { |total, element| total * element }
  end
end
'''

h2. Removing Touching Matching Elements in an Array Until No Touching Elements Match( thanks Eric )

'''ruby
class Array
  def remove_touchers
    self.inject([]) { |final, element| final[-1] == element ? final[0..-2] : final << element }
  end
end
'''

So, why aren't there more people using this method?  Is it just forgotten?  Are you using [Enumberable#inject](http://apidock.com/ruby/Enumerable/inject)?
How are you using it?
