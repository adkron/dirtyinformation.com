--- 
layout: post
title: Inject & Me - BFFs
---
<p>
I find myself turning to <a href='http://apidock.com/ruby/Enumerable/inject'>Enumberable#inject</a> more and more.  It is such a powerful method, yet I rarely see it used in others' code.  Here are a couple of examples of the power of inject.
</p>
<h3>Adding Facorial to All Integers</a>
<pre>
class Integer
  def factorial
    raise 'Cannot take a factorial of a negative number' if self < 0
    return 1 if self == 0
    (1..self).inject { |total, element| total * element }
  end
end
</pre>
<h3>Removing Touching Matching Elements in an Array Until No Touching Elements Match( thanks Eric )</h3>
<pre>
class Array
  def remove_touchers
    self.inject([]) { |final, element| final[-1] == element ? final[0..-2] : final << element }
  end
end
</pre>
<p>
So, why aren't there more people using this method?  Is it just forgotten?  Are you using <a href='http://apidock.com/ruby/Enumerable/inject'>Enumberable#inject</a>?  How are you using it?
</p>
