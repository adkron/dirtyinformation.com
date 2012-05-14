--- 
layout: post
title: In Place Editor and Collection Partials
---
<p>I've noticed a lot of individuals have been having problems with the in_place_editor function and using a collection partial.  The problem comes down to an in_place_editor looking for an variable beginning with an @.  So here is the quick fix.</p>
<br/>
The partial function:
<br/>
<p><pre>
&lt%= render :partial => "item", :collection => @items %&gt
</pre></p>
<br/>
Inside the Partial:
<br/>
<p><pre>
&lt%@item = item%&gt
&lt%= in_place_editor_field :item, 'on_hand' %&gt
</p></pre>
<p>
I know it is a bit of a hack, but it is a very quick fix.  Happy Coding!
</p>
