---
layout: post
title: "Starting with Elixir"
keywords: "elixir,dojo,erlang"
location: São Paulo
short_url: http://goo.gl/3rI7RT
---

I’m writing this post to publicize the material I've wrote to help organizers to setup a coding dojo in Elixir to a group of non-Elixir programmers. The material below ([and in this repo](https://github.com/victorarias/starting_with_elixir)) exposes the basics of the language – just enough to starting coding. I hope this can help people to begin programming with this great language.

<!-- more -->

DISCLOSURE: The material is mainly working code. Non-working code is found within comments. The material is intentionally simple and non-exhaustive, and requires previous knowledge of functional programming.

##Basics

{% highlight elixir %}

# because everything here is working code, is strongly recommended that
# this "guide" should be read alongside the Elixir interactive mode -> iex

# "assignment"

a = 1 # => 1
a + 4 # => 5

1 = a # => 1

2 = a # => ** (MatchError) no match of right hand side value: 1

# wtf? '=' isn't an assignment; it's a kind of "assert"

list = [1, 10, 20] # => [1, 10, 20]
[ a, b, c] = list # => a = 1; b = 10; c = 20;

# assert works here too

[z, 10, x] = list # => z = 1; x = 20;
[h, 15, j] = list # => ** (MatchError) no match of right hand side value: [1, 10, 20]

# "Joe Armstrong, the creator of Erlang, compares the equals sign in Erlang to that used in algebra.
# When you write the equation x = a + 1, you are not assigning the value of a + 1 to x.
# Instead, you’re simply asserting that the expressions x and a+1 have the same value.
# If you know the value of x, you can work out the value of a, and vice versa."
# - Dave Thomas - Programming Elixir

# Lists

list = [1, 10, 20] # => [1, 10, 20]

# Lists are immutable linked lists, so random access is expensive,
# however, they can be constructed or deconstructed with pattern matching

[head | tail] = list # => head = 1; tail = [10, 20]

# Lists Operations

[1, 2, 3] ++ [4, 5, 6]  # => [1, 2, 3, 4, 5, 6]
[1, 2, 3, 4] -- [1, 3]  # => [2, 4]
1 in [1, 2, 3]          # => true

# Tuples

tuple = { 10, 20 }

# pattern matching on tuples

{ a, b } = tuple

# Atoms

# atoms are constants - they are like symbols on Ruby.
# an atom is constant within the application and across machines

{ a, b } = { :foo, :bar }

# Keyword lists

[ fuu: "bar", goos: "fraba", life: 42 ] # => [ {:fuu, "bar"}, { :goos, "fraba" }, { :life, 42 } ]

{% endhighlight %}

##Anonymous Functions

{% highlight elixir %}

# anonymous Functions

add = fn a, b -> a + b end

add.(1, 2) # => 1 + 2

# parameters pattern matching, aka "overloading"

printer = fn
  0, 0, _ -> "FizzBuzz"
  0, _, _ -> "Fizz"
  _, 0, _ -> "Buzz"
  _, _, c -> c
end

printer = fn n ->
  printer.(rem(n, 3), rem(n, 5), n)
  |> to_string
  |> IO.puts
end

fizzbuzz = fn ->
  1..15 |> Enum.each printer
end

# the & notation

# the following function
Enum.map [1, 2, 3], fn n -> n * n end
# is the same as
Enum.map [1, 2, 3], &1 * &1

# the following function
add = fn a, b -> a + b end
# is also the same as
add = &1 + &2

# but the & notation requires some attention
# the following scenario is tricky
add_and_sum = { &1 + &2, &1 * &2 }  # => {&Kernel.+/2, &Kernel.*/2}
# the above function isn't what you expect
# what you really want is
add_and_sum = &{ &1 + &2, &1 * &2 } # => #Function<12.80484245 in :erl_eval.expr/5>

{% endhighlight %}

##Modules and Functions

{% highlight elixir %}

# modules

# defmodule ModuleName do
#   ... content inside module...
# end

# this is a module written with the "real" way of declaring a function

defmodule Factorial do
  def of(0), do: 1
  def of(n), do: n * of(n - 1)
end

# the module below is the same as the above, but using an alternative
# syntax that is transformed during compile time
# to the syntax on the previous example

defmodule Factorial2 do
  def of(0) do
    1
  end
  def of(n) do
    n * of(n - 1)
  end
end

# pattern matching on functions happens inside modules
# and is a great way to simplify solutions

defmodule GCD do
  def of(x, 0), do: x
  def of(x, y), do: of(y, rem(x, y))
end

# guard clauses are also supported

defmodule Factorial do
  def of(0), do: 1
  def of(n) when n > 0, do: n * of(n - 1)
end
# Factorial.of(-1)
# ** (FunctionClauseError) no function clause matching in Factorial.of/1

{% endhighlight %}

##Erlang Interop

{% highlight elixir %}

# erlang interop

:io.format("~.2f~n", [5.126])
# => 5.13
# => :ok

# module 'io' from Erlang's stdlib is accessible through an atom with the same name
# so, as you can imagine, there is an io module with the function format in it :)

{% endhighlight %}

##Built-in data structures

{% highlight elixir %}

dict = HashDict.new first: "Victor", last: "Arias",
  country: "Sweden", music: "Lungs from Florence + The Machine"

# Dict protocol
Dict.get dict, :music # => "Lungs from Florence + The Machine"
Dict.keys dict # => [:country, :first, :last, :music]
Dict.values dict # => ["Sweden", "Victor", "Arias", "Lungs from Florence + The Machine"]

# more in http://elixir-lang.org/docs/stable/Dict.html

set = HashSet.new [:first, :last, :country, :music]
set2 = HashSet.new [:city, :state]

# Set protocol

Set.member? set, :country # => true
Set.union set, set2 # => #HashSet<[:city, :country, :first, :last, :music, :state]>
Set.difference set, set2 # => #HashSet<[:country, :first, :last, :music]>

# more in http://elixir-lang.org/docs/stable/Set.html

# List

List.concat [1, 2], [10, 11] # => [1, 2, 10, 11]
List.flatten [[1], 2, [[[3]]]] # => [1, 2, 3]

# fold(l|r), zip, unzip, last, keyfind, keydelete, keyreplace
# more in http://elixir-lang.org/docs/stable/List.html

# Enum & Stream

Enum.each ["a", "b", "c"], IO.puts(&1)
Enum.all? [2, 4, 6], &(rem(&1, 2) == 0)
Enum.sort [1, 10, 2, 42, 17] # => [1, 2, 10, 17, 42]

{% endhighlight %}

##List Comprehensions

{% highlight elixir %}

list = lc x inlist [1, 2, 3], do: x - 1 # => [0, 1, 2]

list = lc x inlist [1, 2, 3], y inlist [:a, :b, :c], do: { x, y }
# => [{1, :a}, {1, :b}, {1, :c}, {2, :a}, {2, :b}, {2, :c}, {3, :a}, {3, :b}, {3, :c}]

list = lc x inlist [1, 2, 3], y inlist [:a, :b, :c], x > 2, do: { x, y }
# => [{3, :a}, {3, :b}, {3, :c}]

{% endhighlight %}

##Records

{% highlight elixir %}

defrecord Person, first_name: nil, last_name: nil, age: nil, country: "Brazil"

person = Person.new first_name: "Victor", last_name: "Arias", age: 26
# => Person[first_name: "Victor", last_name: "Arias", age: 26, country: "Brazil"]

Person[first_name: "Victor", last_name: "Arias", country: "Sweden"] # <- compile time!
# => Person[first_name: "Victor", last_name: "Arias", age: nil, country: "Sweden"]

# a record is just a tuple starting with a module
# so the following tuple declaration works as a record creation
# exactly as Person.new ...
another_person = {Person, "Victor", "Arias", 26, "Brazil"}
# => Person[first_name: "Victor", last_name: "Arias", age: 26, country: "Brazil"]

{% endhighlight %}

##Mix

{% highlight elixir %}

# mix is a command line utility to manage Elixir projects.
# it can be seen as a 'mix' between rake (db:|routes...) and rails (s|g|...)

# victorarias@Victors-MacBook-Pro ~/development/starting_with_elixir $ mix help
# mix                 # Run the default task (current: mix run)
# mix archive         # Archive this project into a .ez file
# mix clean           # Clean generated application files
# mix cmd             # Executes the given command
# mix compile         # Compile source files
# mix deps            # List dependencies and their status
# mix deps.clean      # Remove the given dependencies' files
# mix deps.compile    # Compile dependencies
# mix deps.get        # Get all out of date dependencies
# mix deps.unlock     # Unlock the given dependencies
# mix deps.update     # Update the given dependencies
# mix do              # Executes the tasks separated by comma
# mix escriptize      # Generates an escript for the project
# mix help            # Print help information for tasks
# mix local           # List local tasks
# mix local.install   # Install a task or an archive locally
# mix local.rebar     # Install rebar locally
# mix local.uninstall # Uninstall local tasks or archives
# mix new             # Creates a new Elixir project
# mix run             # Run the given file or expression
# mix test            # Run a project's tests

{% endhighlight %}

##Testing

{% highlight elixir %}

# Elixir bundles a simple yet effective testing library
# is worth noting that the 'assert' used below
# is actually a macro, and because of that
# it can "see" the code being asserted
# and print meaningful error messages when the assert fails

ExUnit.start

defmodule Tests do
  use ExUnit.Case

  test "fuu" do
    assert true
  end

  test "introspection" do
    assert 1 == 2
  end

  test "introspection 2" do
    assert 1 > 3
  end

  test "introspection 3" do
    assert 1 >= 3
  end
end

#  1) test introspection (Tests)
#     ** (ExUnit.ExpectationError)
#                  expected: 1
#       to be equal to (==): 2
#     at 9_testing.exs:11
#
#  2) test introspection 2 (Tests)
#     ** (ExUnit.ExpectationError)
#              expected: 1
#       to be more than: 3
#     at 9_testing.exs:15
#
#  3) test introspection 3 (Tests)
#     ** (ExUnit.ExpectationError)
#                          expected: 1
#       to be more than or equal to: 3
#     at 9_testing.exs:19

{% endhighlight %}

Any feedback is appreciated :)
