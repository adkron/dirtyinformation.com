---
  layout: post
  title: A is for Abbrev
---

I hope that this is only the first in a set of posts that are to make
an intriguing right of passage through the ABCs of the core and
standard libraries.

'A' begins the alphabet and it would be so easy to start out with Ruby's
[Array](http://ruby-doc.org/core-2.0.0/Array.html). Everyone knows there
are many amazing ways to traverse, and manipulate Ruby's array, but we
want to grow in our knowledge. Instead of Array we set our sites on a
very small Module,
[Abbrev](http://ruby-doc.org/stdlib-2.0.0/libdoc/abbrev/rdoc/Abbrev.html).

Weighing in at only 131 lines there isn't much to talk about. Once we put
it on a comment diet Abbrev has a mere 44 lines of code. These 44 lines
create only two methods.

The first method is the module function Abbrev.abbrev. This method can
take two arguments. The first argument is an array of strings. The second
is an optional argument, but we will get back to that in a moment. For
now we will concentrate on the required argument. When passing in an
array of strings Abbrev.abbrev returns a hash of, get this,
abbreviations. The abbreviations are created by taking the beginning of
each word until it clashes with another abbreviation.


```ruby
  require "abbrev"
  require "pp" #for your reading pleasure
  pp Abbrev.abbrev(%w[aardvark arron amos dog])
  {"aardvark"=>"aardvark",
   "aardvar"=>"aardvark",
   "aardva"=>"aardvark",
   "aardv"=>"aardvark",
   "aard"=>"aardvark",
   "aar"=>"aardvark",
   "aa"=>"aardvark",
   "arron"=>"arron",
   "arro"=>"arron",
   "arr"=>"arron",
   "ar"=>"arron",
   "amos"=>"amos",
   "amo"=>"amos",
   "am"=>"amos",
   "dog"=>"dog",
   "do"=>"dog",
   "d"=>"dog"}
```

Now that we have hit the pinnacle of what ruby can do, and still scale,
it is time to move on to the optional argument. The optional argument
is a pattern that is used to match against the abbreviations. It can
be a string or a regular expression. If a string is passed it is
treated as a `begins with` when matching against the abbreviations.

```ruby
  pp Abbrev.abbrev(%w[aardvark arron amos dog], /ar/)
  {"aardvark"=>"aardvark",
    "aardvar"=>"aardvark",
    "aardva"=>"aardvark",
    "aardv"=>"aardvark",
    "aard"=>"aardvark",
    "aar"=>"aardvark",
    "arron"=>"arron",
    "arro"=>"arron",
    "arr"=>"arron",
    "ar"=>"arron"}
```

I haven't found a great use for the pattern argument. If you have any
ideas let me know, and I will add them to the bottom of the article.
Even better, make a pull request to the
[blog](http://github.com/adkron/diryinformation.com). I like to use the
Abbrev when creating command line utilities that take subcommands. You
can then check the subcommands against the hash output of Abbrev.abbrev.
Then your power users have shortcuts. You might notice this with the
rails command. `rails generate` vs `rails g`.

Next time your creating a command line utility, or accepting commands in
your own MUD, reach for Abbrev like a boss.

I almost forgot, the second method. When you require abbrev an extension
is added to array. Array.abbrev that takes an optional pattern. This in
turn calls Abbrev.abbrev(self, pattern).
