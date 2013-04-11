---
layout: post
title: Change Classes not Styles
---

I've been working with some javascript lately and wanted to see if it made a difference if I was changing the class or inline style of an element.  I had heard that changing classes was faster, but can it really make a big difference?

## YES!!!

I coded up a quick [example](https://gist.github.com/663556). Here are the results in ms(all on osx):
<ul>
  <li>Safari 5.0.2
        <ul>
          <li>changing style:123</li>
          <li>changing class:15 - 87.8% faster</li>
        </ul>
  </li>
  <li>Chrome 7.0.517.44
        <ul>
          <li>changing style:115</li>
          <li>changing class:16 - 86.1% faster</li>
        </ul>
  </li>
  <li>Firefox 3.0.15
        <ul>
          <li>changing style:333</li>
          <li>changing class:99 - 70.3% faster</li>
        </ul>
  </li>
  <li>Firefox 3.6.12
        <ul>
          <li>changing style:261</li>
          <li>changing class:60 - 77% faster</li>
        </ul>
  </li>
  <li>Opera 10.10
        <ul>
          <li>changing style:554</li>
          <li>changing class:111 - 80% faster</li>
        </ul>
  </li>
</ul>

I really didn't expect those kinds of results. Kind of amazing!
