--- 
layout: post
title: Code Review Toolbox
---
<p>
Doing a code review is difficult for me.  I don't always think of the design implications because I'm trying t hurry and get back to coding.  I have mainly been checking for tests, and functionality.  This needs to change and I think I've got some decent steps to help me make the leap, and use some tools to help illustrate my concerns to my coworkers.
</p>
<dl title='code review toolbox'>

  <dt><a href='http://jonathanaquino.com/CodeReviews.pdf'>Code Review Cheat Sheet</a></dt>
  <dd>Great points on how to handle a code review without upsetting the developer of that code.</dd>

 <dt><a href='http://ruby.sadi.st/Flog.html'>Flog</a></dt>
  <dd>A 'golf' score for your code.  Anytime code hurts my eyes, flog has a score that backs up my thoughts.  Even without the developer knowing how things are being scored numbers seem to work for us.</dd>

  <dt><a href='http://ruby.sadi.st/Flay.html'>Flay</a></dt>
  <dd>Find violations of the dry principle and often fixing these issues can lower your flog score.</dd>

  <dt><a href='http://github.com/martinjandrews/roodi/tree/master'>Roodi</a><dt>
  <dd>Points out OO Design principle issues.  I haven't used this much, but the few test runs I tried came up with some good suggestions.  Really you should just check out the docs for all the things Roodi does.</dd>

  <dt><a href='http://github.com/kevinrutherford/reek/tree/master'>Reek</a><dt>
  <dd>Ruby code smell detector.  Reek looks for a lot of common <a href='http://wiki.github.com/kevinrutherford/reek/code-smells'>code smells</a> and is easily configureable.</dd>
</ul>

<p>
I hope that by bringing a new approach to code reviews I can improve our product, team morale, and the skills of everyone involved.  I will keep you posted on how my toolbox works out.  I'd also welcome any suggestions on source about improving code reviews and/or tools like the above that help focus where I need to spend the most time reviewing.
</p>
