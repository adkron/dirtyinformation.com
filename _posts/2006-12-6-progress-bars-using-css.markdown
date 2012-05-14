--- 
layout: post
title: Progress Bars using css
---
<p>
I spend a little time trying to get css to make a progress bar for me.  It took some thought and time, but everything worked out well.  I thought I would put it here for everyone to see.
</p>
<p>
The CSS
<pre>

.prog-empty {
  width: 400px;
  height: 15px;
  background: #247;
  padding: 0;
  margin: 20px;
  border: 5px;
}

.prog-bar {
  height: 15px;
  background: #f70;
  padding: 0;
  margin: 0;
}

</pre>
</p>
<p>
The html
<pre>

&lt div class="prog-empty"&gt
  &lt div class="prog-bar" style="width: 63%"&gt
  &lt/div&gt
&lt/div&gt

</pre>
</p>
<p>
Now the width percentage needs to be replaced with a function.  With the exact code from above I get something like:
</p>

<div style="width: 400px;
                   height: 15px;
                   background: #274;
                   padding: 1px;
                   margin: 20px;
                   border: 5px;">
  <div style="width: 63%;
                     height: 15px;
                     background: #f70;
                     padding: 0;
                     margin: 0;">
  </div>
</div>

<p>
I hope everyone can find this useful.  Remember that you can use url("image.file") in place of the colors.
</p>
