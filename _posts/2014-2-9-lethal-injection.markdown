---
layout: post
title: Lethal Injection
---

When was the last time that you ran into something that just didn't
quite fit into the nice little RESTful world you think you've built
yourself? Or when you found yourself jamming more and more conditionals
into your controllers? Do you ever get the feeling there is just too
much to be done in that little controller?

```ruby
  #bad restful controller
  def update
    if params.has_key?(:ordered_ids)
      #update position attribute of each record
    else
      #some other form of updating
    end
  end
```

So you reach for the non-RESTful version.

```ruby
  #routes.rb
  resources :things do
    collection do
      post :reorder
    end
  end

  #bad controller
  def reorder
    #update ordering
  end
```

This seems like it isn't a bad idea at first, but then you start doing
this all over the site. You find that your are copying code everywhere.
All because you missed the noun. Why don't we call is a sort order?

Now create a sort order controller for each thing needing sorting. Now
we sit with a lot of rote work to set up each controller. We still
haven't reached the nirvana that we know is out there. All we need is a
little dependency injection and the controllers can melt away.

I wish that I could claim that there is some amazing secret here, but
Rails gives us a beautifully simple way to inject parameters into our
controllers.

```ruby
  #routes
  scope "things", as: "things", klass: Thing do
    resource :order, only: [:update]
  end
  resources :things

  scope "foos", as: "foos", klass: Foo do
    resource :order, only: [:update]
  end
  resources :foos

  #order controller
  def update
    SortOrder.new(sortable_class).reorder(ordered_ids)
    #...
  end

  private
  def sortable_class
    params.fetch(:klass)
  end

  def ordered_ids
    params.fetch(:ordered_ids) { [] }
  end
```

Ok, so what do we have going on here? I hope that no one gets hung up on
the example and can see what is going on. Within the routes file we are
injecting a parameter. In our case we happen to call it `klass`. You can
call it whatever you like. This parameter need not be a String like the
parameters coming from the browser. These parameters override anything
coming from the browser and since we are using actual Ruby classes we
have no issues with evaluating Strings.

Now we can reduce repetition and stay within the RESTful bounds. Our
controllers really can concentrate on a single responsibility. Now I can
get some sleep.
