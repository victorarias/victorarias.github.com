---
layout: post
title: "Extending Ruby Types - Ruby Love Part 1"
keywords: ""
location: São Paulo
short_url: 
---

In my last post I wrote about a "bad part" of Ruby: [how easily can it be to "leaky" memory with Ruby](http://victorarias.com.br/2013/08/13/leaky-ruby.html). Although that's true, Ruby is a **GREAT** language and because of that, I decided to write a series of posts about topics that I like the most in it. Today I'm starting with how to extend classes and types.

<!-- more -->

But first, just to be clear: _if there was a book called ["Ruby: The Good Parts"](http://www.amazon.com/gp/product/0596517742/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0596517742&linkCode=as2&tag=vicarisblo-20), it would be almost as the same size as the ["Ruby: The Definitive Guide"](http://www.amazon.com/gp/product/0596805527/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0596805527&linkCode=as2&tag=vicarisblo-20) - Javascript people may get this joke :-)_

##Extending Types

One of the most famous features from Ruby is monkey patching. Everyone knows it, a lot of people hates it. Besides all the drama, monkey patching can be very useful in some scenarios like, for instance, applying a temporary patch to fix an annoying bug inside that gem that you love so much (and still isn't distributed with the fix). 

The code below illustrate how easy is to monkey patch something:

{% highlight ruby %}

puts 2.respond_to? :power_of_two  # => false
puts 2.class                      # => Fixnum

class Fixnum
  def power_of_two
    self * self
  end
end

puts 2.respond_to? :power_of_two  # => true
puts 2.power_of_two               # => 4

{% endhighlight %}

Another great use of monkey patching is to customize behavior from code that you do not own, like for instance from a gem. Although useful, this kind of hacking can be dangerous: no one besides you expects the custom behavior, and this can lead to some hard debugging and a lot of frustration. My recommendation: **do it only when there is no other option, and start looking for an alternative** - I would, for example, talk to the gem author to try to find a better way to solve the problem. This can be a great opportunity to contribute with a Pull Request!

One last thing before moving to the next (and best in my humble opinion) part: **Do not extend Ruby core classes!** [Monkey patching types like String is a very bad thing to do](http://brainspec.com/blog/2013/08/09/make-love-not-ruby-core-extensions/). A safer way to deal with problems that need core type extensions could be through decorators!

##Decoration

I just can't explain with words how easy is to decorate objects with Ruby, so here is one way of doing it:

{% highlight ruby %}

require 'delegate'

class Product
  attr_reader :name, :kind

  def initialize(name, kind)
    @name = name
    @kind = kind
  end
end

class PresentableProduct < SimpleDelegator
  def name
    "<h1>#{super}</h1>"
  end
end

product = Product.new("New Trendy Tablet", "Gadget")
product = PresentableProduct.new(product)

puts product.name   # => <h1>New Trendy Table</h1>
puts product.kind   # => Gadget
puts product.class  # => PresentableProduct

{% endhighlight %}

The *SimpleDelegator* class delegates every call to the object passed into the constructor, making the code above act like a layer on top of the decorated object.

[The decorator pattern](http://en.wikipedia.org/wiki/Decorator_pattern) is a extremely valuable tool that every developer should have in his/hers pocket - decorators are a great way of extending behavior without increasing the complexity of the original code. The fact that it costs almost nothing to decorate something with Ruby really blows my mind - the technique above can be a great lightweight way of extending models with presenting behavior to DRY up your views by making them logic-less.

There is many ways to decorate objects in Ruby, and a better way is through modules and the extend method:

{% highlight ruby %}

class Product
  attr_reader :name, :kind

  def initialize(name, kind)
    @name = name
    @kind = kind
  end
end

module PresentableProduct
  def name
    "<h1>#{super}</h1>"
  end
end

product = Product.new("New Trendy Tablet", "Gadget")
product.extend(PresentableProduct)

puts product.name   # => <h1>New Trendy Table</h1>
puts product.kind   # => Gadget
puts product.class  # => Product

{% endhighlight %}

This technique is even more powerful. With it you're actually changing the object instead of just creating a shell over it - this means that calls originated from inside the object can "see" the decorated code (this doesn't happens with SimpleDelegator). Because it does not change the objects *class*, this approach also plays nicely with *Rails* and other frameworks that depends on the *class* information (*Inflections* are a good example of feature that uses the *class* information).

Code extension and decorators are a very extensive subject, but I hope with this post I could bring something new to people who's starting with the language or who don't know some of its no-so-trivial features. See ya!

*OBS: Ruby 2.0 is packed with a feature called "Refinements" that improves the way of how monkey patching can be made, but it's still a experimental feature and I consciously omitted it here because of that.*



