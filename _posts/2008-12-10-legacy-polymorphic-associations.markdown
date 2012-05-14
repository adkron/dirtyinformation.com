--- 
layout: post
title: Legacy Polymorphic Associations
---
<p>
Did I mention I hate legacy databases.
</p>
<p>
I'm working on a legacy project that uses a polymophic associations where the type column is lower case.  The thing that sucks is I can't change the database because I still have an untested  Java side that we don't want to change.  So here comes the solution, and maybe some flames.
</p>
<p>
ActiveRecord::AttributeMethods#read_attribute is called on the type field, which we will call foo, because it isn't called type in our database.  So in our model we have this:
</p>
<pre>
#Disclaimer
def read_attribute(attr_name)
  if attr_name == 'foo'
    #code to change to accepted type column values
    foo.camelize
  else
    super
  end
end
</pre>
