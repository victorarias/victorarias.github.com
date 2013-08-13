---
layout: post
title: "Leaky Ruby - Caution With Procs"
keywords: "ruby,memory leak,proc,block,lambda,leak,garbage collector"
location: São Paulo
short_url: http://goo.gl/ieh3Q1
---

Watching the discussion on the Ruby Rogues Parley about the difference between procs and lambdas I realized that, by being tightly coupled with the context they are created, procs could easily create memory leaks.

<!-- more -->

The following example illustrates that:

{% highlight ruby %}

class Printer
  @messages = []

  def self.print
    puts yield
  end

  def self.lazy_print(&block)
    @messages << block
  end
end

class Foo
  def initialize(i)
    Printer.print { "hey #{i}" }
  end
end

class LeakyFoo
  def initialize(i)
    Printer.lazy_print { "hey #{i}" }
  end
end

100.times { |i|  Foo.new i }
100.times { |i|  LeakyFoo.new i }

GC.start
puts "Foo count = #{ObjectSpace.each_object(Foo).count}"		#=> Foo count = 0
puts "LeakyFoo count = #{ObjectSpace.each_object(LeakyFoo).count}"	#=> LeakyFoo count = 100

{% endhighlight %}

As you can see, because the Printer class caches every block for future use, not one LeakyFoo has been collected by the GC.

Unfortunately is extremely easy to forget about this behavior, so anyone (even me) is likely to write the following piece of code to define an instance finalizer:

{% highlight ruby %}

class FatFoo
  def initialize(i)
    ObjectSpace.define_finalizer( self, proc { puts "finalizing  #{i}"} )
  end
end

100.times { |i|  FatFoo.new i }
GC.start
puts "FatFoo count = #{ObjectSpace.each_object(FatFoo).count}" #=> 100 

{% endhighlight %}

One solution to this problem is to create the Proc object in a different context:

{% highlight ruby %}

class SlimFoo
  def initialize(i)
    ObjectSpace.define_finalizer( self, self.class.finalizer(i) )
  end

  def self.finalizer(i)
    proc { puts "finalizing #{i}" }
  end
end

100.times { |i|  SlimFoo.new i }
GC.start
puts "SlimFoo count = #{ObjectSpace.each_object(SlimFoo).count}"

{% endhighlight %}

Sadly, this is the kind of behavior that can bring subtle and catastrophic bugs.