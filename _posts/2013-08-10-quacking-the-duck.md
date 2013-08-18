---
layout: post
title: "Quacking The Dog - Duck typing for happiness"
keywords: "ruby,duck typing,object oriented design,code design"
location: São Paulo
short_url: http://goo.gl/N2ga9e
---

Duck typing is the "feature" that I currently like the most in Ruby, mainly because it makes it easier to create well-designed code - after all, you don't need to worry about the type system: your focus is on the message you're sending and the kind of role the object you're interacting with can assume.

<!-- more -->

*Edit: I realized that I should have made myself clear that dynamic languages aren't silver bullets and can be dangerous. Because of that I appended a new section in this post to talk about it.*

<img alt="duck typing" style="float:right; margin: 20px 0 5px 15px;" src="/images/duck_typing.jpg"/>

I know Ruby is a "duck type language" since I first read about it, but my background with statically compiled languages prevented me to really understand what duck typing really mean - at least in Ruby. The theory is very simple: if you're designing a method that is expecting a parameter Duck to then send it a message called "quack", any kind of object that respond to "quack" could be passed to it - the type of the parameter isn't important. The obvious conclusion is that if you create the class Dog that implements the method "quack" (a very odd dog), it's perfectly acceptable to pass an instance of Dog to the previous method. It's cool and etc., but it doesn't really makes a big difference, right? At least it was what I thought for a considerably period of time :-(


The power of duck typing struck me when I realized that it enables a very strong relation between objects, empowering APIs/models and reducing noise in code. To illustrate this, let me show you some examples from the Ruby Standard Library.

## File.open

*File.open("path/to/file")* is a very common idiom to create streams to read a file: you pass the **path** and the library returns an object that can read the file. Did you notice how I highlighted the word "path"? It isn't by chance - the *'open'* function really expects something that fit the role of a path, not just a string containing a location. The difference is subtle, but it allows us to write code like this:

{% highlight ruby %}

class VimConfig
  # ... behavior ... #	
  
  def to_path
    "~/.vimrc"
  end
end

config = VimConfig.new
config_file = File.open config

{% endhighlight %}

Nice, isn't it? The Ruby's File API converts its parameters before using it, and one of these conversions is through the *'to_path'* idiom. If you're curious how, below is the C code that does it (*'rb_f_open'* calls *'FilePathValue'* which consequently calls *'rb_get_path_check_to_string'*):

{% highlight c %}

static VALUE
rb_f_open(int argc, VALUE *argv)
{
    ID to_open = 0;
    int redirect = FALSE;
 
    if (argc >= 1) {
  CONST_ID(to_open, "to_open");
	if (rb_respond_to(argv[0], to_open)) {
	    redirect = TRUE;
	}
	else {
	    VALUE tmp = argv[0];
	    FilePathValue(tmp);
	    if (NIL_P(tmp)) {
		redirect = TRUE;
	    }
	    else {
                VALUE cmd = check_pipe_command(tmp);
                if (!NIL_P(cmd)) {
		    argv[0] = cmd;
		    return rb_io_s_popen(argc, argv, rb_cIO);
		}
	    }
	}
    }
    if (redirect) {
	VALUE io = rb_funcall2(argv[0], to_open, argc-1, argv+1);
 
	if (rb_block_given_p()) {
	    return rb_ensure(rb_yield, io, io_close, io);
	}
	return io;
    }
    return rb_io_s_open(argc, argv, rb_cFile);
}
 
VALUE
rb_get_path_check_to_string(VALUE obj, int level)
{
    VALUE tmp;
    ID to_path;
 
    if (insecure_obj_p(obj, level)) {
	rb_insecure_operation();
    }
 
    if (RB_TYPE_P(obj, T_STRING)) {
	return obj;
    }
    CONST_ID(to_path, "to_path");
    //to_path call!
    tmp = rb_check_funcall(obj, to_path, 0, 0);
    if (tmp == Qundef) {
	tmp = obj;
    }
    StringValue(tmp);
    return tmp;
}

{% endhighlight %}

## Array indexer

The array indexer *(a_array\[index\])* is another example of rich API: it calls *'to_int'* on the indexer, so any object that answers to *to_int* can be used. This enables code like this:

{% highlight ruby %}

class PodiumPosition
  # .. behavior .. #
  def to_int
    @race_position
  end
end
 
position = PodiumPosition.new(1)
prizes = [ "orange", "apple", "corn" ]
puts "Congrats, you won #{prizes[position]}"

{% endhighlight %}

##IO.select

It was through the *IO.select* API that I first saw Ruby's power. This API calls the *select(2)* system API that receives file descriptors and blocks the current thread until one or more of the file descriptors become ready for an IO operation. The Ruby API is defined as above:

{% highlight ruby %}

	select(read_array
		[, write_array
		[, error_array
		[, timeout]]]) → array or nil

{% endhighlight %}

So, basically you can pass an array of streams and "select" will block the calling thread until the streams are ready to be read or written (errors or timeouts aren't important right now). The problem is: when you are dealing with streams you often store them inside meaningful objects with behavior (like a Connection class on a networking code), hiding the IO object from the rest of the application (encapsulation!). How can a Reactor core (from the [Reactor Pattern](http://en.wikipedia.org/wiki/Reactor_pattern)), for instance, pass a stream to the *'select'* API? Should it break the encapsulation and map the objects? Obviously no! *'to_io'* conversion method to the rescue!

{% highlight ruby %}

class Connection
  # .. rest of the class .. #
 
  def accept_connection(io)
    @io = io
 
    # new connection code
  end
 
  def to_io
    @io
  end
end 
 
class Reactor
  # array_of_connections_to_read is an array of instances of the above Connection class
  # array_of_connections_to_write is an array of instances of the above Connection class
 
  def tick
    to_read, to_write = IO.select(array_of_connections_to_read, array_of_connections_to_write)
  end
end

{% endhighlight %}

As you can see, Ruby's standard library is filled with <img alt="love" style="width: 32px;" src="http://www.victorarias.com.br/images/heart.png"/>.

##Refactoring

The most obvious but worth mention consequence of duck typing is how it makes refactoring easier: the ["Replace Conditional with Polymorphism"](http://www.refactoring.com/catalog/replaceConditionalWithPolymorphism.html) and ["Replace Type Code with Strategy/State"](http://www.refactoring.com/catalog/replaceTypeCodeWithStateStrategy.html) refactorings, just to name a few, are much simpler and fast to do when you don't have to care about types, but only about behavior.

##The Dark Side

*appended section*

Not having a compiler to watch your back can be dangerous. A professional Ruby programmer **(1)** shall never forget that he or she have the responsibility to test the codebase and assert its behavior, and **(2)** have to write clean code and refactor smells. Ruby code must always be well-written, otherwise the risk of a debugging nightmare is just too high.

Also, dynamic languages are best suited for small-to-mid-sized software. My experience showed me that is better to split applications into smaller ones when they become too complex, and this practice is even more important when dealing with dynamic languages - a reddit user made the point of having to rename a function used across a 100k LOC codebase, and I just can't say that it is easy to accomplish that. Dealing with a **published** interface is just hard - sometimes is better to just "mark it" as obsolete.


##Conclusion

Dynamic languages are nice to solve problems employing good design, and Ruby's Standard Library adopts duck typing in a very nice way. It presents a very good example of how a Ruby programmer should program: accepting objects that fits in a role instead of just an instance of a given type.

I hope this post clarifies people who still didn't realize the full power of languages like Ruby. I would recommend these books for further study:

- [Practical Object-Oriented Design in Ruby](http://www.poodr.info/), from Sandi Metz
- [Confident Ruby](http://devblog.avdi.org/2012/06/05/confident-ruby-beta/), from Avdi Grimm
- [Object on Rails](http://objectsonrails.com/), also from Avdi Grimm (free to read on the web - it contains an amazing use of decorators that every Ruby developer should know)
