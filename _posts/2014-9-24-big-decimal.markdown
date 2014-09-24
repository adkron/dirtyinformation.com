---
  layout: post
  title: B is for BigDecimal
---

Continuing the jaunt into the Ruby alphabet we run into
[BigDecimal](http://www.ruby-doc.org/stdlib-2.0.0/libdoc/bigdecimal/rdoc/BigDecimal.html#method-c-mode)
and BasicObject. BasicObject is so, well, basic. It is something to take
a look at, but I believe that the usage of BigDecimal is one thing that
every developer needs in their tool belt.

Have you ever had a problem with a floating point number? Did you even
notice the issue? Why don't we try this one on for size.

```ruby
  1.2-1.0 #=> 0.19999999999999996
```

What in the heck happened? This is an error in the representation of
floating point by binary numbers. To understand the accuracy issues and
their underlying causes everyone should read
[this](https://en.wikipedia.org/wiki/Floating_point#Accuracy_problems).
Don't go away quite yet. There is a way to solve this problem.

The superhero of our day is BigDecimal. Let's take out example from
above.

```ruby
  BigDecimal.new("1.2") - BigDecimal("1.0") #=> #<BigDecimal:7f809b143930,'0.2E0',9(27)>

  (BigDecimal.new("1.2") - BigDecimal("1.0")).to_s #=> "0.2E0"
```

You will need to be familiar with [scientific
notation](https://en.wikipedia.org/wiki/Scientific_notation) in order to
understand the output.

Notice that the output no longer suffers from the same issues at the
above. There is one small issue that I have found in creating a new
BigDecimal with a floating point argument.

```ruby
  BigDecimal.new(1.0) #=> ArgumentError: can't omit precision for a Float.
  BigDecimal.new(1.0, 1) #=> #<BigDecimal:7f809b0671b0,'0.1E1',9(27)>
```

When providing a floating point number as the input of a new BigDecimal
we must also provide the number of [significant
digits](https://en.wikipedia.org/wiki/Significant_figures). These digits
determine our level of confidence in the numbers after the decimal
point. BigDecimal uses the significant digits to determine where to
round values when completing computations. Here are a few outputs from
irb.

```ruby
irb(main):031:0> BigDecimal.new(1.2222, 1)
=> #<BigDecimal:7f809b0147d0,'0.1E1',9(27)>

irb(main):032:0> BigDecimal.new(1.2222, 2)
=> #<BigDecimal:7f809a8bb060,'0.12E1',18(27)>
```

The first input shows 0.1E1 and loses all the information from after the
decimal point in 1.2222. This is because our confidence is only one digit.
The second version shows a 2 because we include the first digit
after the decimal place in our count of significant digits.

The final digit is handled through a rounding mode of the BigDecimal.
The default mode is half_up. This means that the BigDecimal is rounded
to the nearest neighbor or up if equidistant. This is the same rounding
you probably used in school. There are many different versions of
rounding that are available to us. For example, we will use Banker's
rounding. This is a type of rounding found in many bookkeeping
applications. In banker's rounding we round to the nearest number, but
if we are equidistant we round to the nearest even number. Don't ask me
why this is used. It seems crazy to me.

```ruby
irb(main):037:0> BigDecimal.new(1.27,2)
=> #<BigDecimal:7f809a883ea8,'0.13E1',18(27)> #default rounding

irb(main):038:0> BigDecimal.mode(BigDecimal::ROUND_MODE, :banker)
=> 7

irb(main):040:0> BigDecimal.new(1.25,2)
=> #<BigDecimal:7f809a84b508,'0.12E1',18(27)>

irb(main):041:0> BigDecimal.new(1.35,2)
=> #<BigDecimal:7f809a842fe8,'0.14E1',18(27)>
irb(main):042:0>
```

I hope that this gets you a good start to using BigDecimal. Next time
you are working with floating points you may find that your needs are
better suited by BigDecimal.
