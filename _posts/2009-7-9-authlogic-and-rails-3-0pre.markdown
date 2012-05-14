--- 
layout: post
title: Authlogic and Rails 3.0pre
---
<p>
  With some of the moving around of code in Rails 3 Authlogic is broken.  This is a quick fix, and not the solution.  Add the following to your Session class:
</p>

<pre>
class UserSession < Authlogic::Session::Base
  def self.model_name
    ActiveModel::Name.new(self.to_s)
  end
end
</pre>

<p>
You should be good to go.  I have to read up on the process for patching authlogic.  Right now it is late and I am sick.  I hope this quick fix gets someone going.
</p>
