--- 
layout: post
title: Shoulda Refactorred
---
<p>
<a href=''>Shoulda</a> has been great for testing, and is really easy to condense.  So let's refactor a shoulda test.
</p>
<pre>
class PostTest < Test::Unit::TestCase
  should_ensure_length_in_range :zip, (6..10)
  should_ensure_length_in_range :title, (3..20)
  should_ensure_lenght_in_range :phone, (7..10)
end
</pre>
<p>
Now refactored:
</p>
<pre>
class PostTest < Test::Unit::TestCase
  {:zip => (6..10), :title => (3..20), :phone => (7..10)}.each_pair do |field, range|
    should_ensure_length_in_range field, range
  end
end
</pre>
<p>
And there we have it.  Condensed code, and if you have enough of these fields it can really save some key strokes.
</p>
