---
layout: post
title: "Book Review: Modern C++ Programming with TDD"
keywords: "cpp,c++,c++11,modern c++,tdd,test driven development,tests,book"
location: São Paulo
short_url: http://goo.gl/P6J8f
---

I may be wrong but TDD and testing in general aren't as common in the C++ world as they are in Java or Ruby, and because of that and the fact that I already had TDD internalized, I've always found the process of learning C++ very hard.
Besides that, for a beginner, having to deal with the language, different compilers, the compilation process and a crazy amount of boring gigantic books on the subject makes the journey of becoming a good C++ programmer very challenging.
However, [Jeff Langr](http://langrsoft.com/) published a beta version of his book ["Modern C++ Programming with TDD"](http://pragprog.com/book/lotdd/modern-c-programming-with-test-driven-development) and with it he may be changing this perilous landscape!

<!-- more -->

###The Book

<div class="thumb">
  <a href="/images/2013-06-28/book.jpg" target="_blank">
    <img class="center simple-border" src="http://www.victorarias.com.br/images/2013-06-28/book.jpg" alt="screenshot"/>
  </a>
</div>

["Modern C++ Programming with TDD"](http://pragprog.com/book/lotdd/modern-c-programming-with-test-driven-development) starts listing dependencies used during the book like Google Mock, libcurl, rlog and so on. Jeff is trying to make every sample portable and easy, so not only he describes how to compile and where to put every dependency but he also provides CMake scripts with the distributed source code.
After some plumbing the book really starts in the second chapter: Jeff guides the reader through the process of building the Soundex algorithm using (and explaining) TDD, C++ and Google Test. It's a great chapter even for experienced TDD practitioners - I caught myself doing the Soundex Kata with C++ more than one time after reading it. I imagine that this chapter can also be very useful for an experienced C++ programmer that lack TDD skills - it's a GREAT introduction.

###More About TDD than C++

TDD foundations, tests construction and organization, test doubles and mocking, incremental design, advanced topics, indicators and general tips and strategies about growing and sustaining TDD are the subjects of the following chapters, but although the focus is more on TDD than on C++, I've found the book very refreshing and clear about language particularities, and the modern techniques and constructions from C++11 made me really enjoy the reading - Jeff have no constraint about using the auto keyword, lambdas, range-based for loops, uniform initialization and many other C++11 features. I've also found his "programming by intention" and "small methods" advocacy encouraging (considering what I've seen on C/C++).

###Test Doubles and Mocking

This was the part I was more curious about: how harder it is to apply mocking with C++. The book has an entire chapter about it, divided in three parts: dependency breaking into isolated classes applying handcrafted test doubles, how to simplify test doubles using Google Mock and different ways of designing code to use test doubles (aka injection techniques).
I cannot say that it is easier to create mocks, doubles and set-up expectations with C++ as it is, for instance, with Ruby, but we can't compare a dynamic language with C++. Google Mock does a decent job, so with just a little effort anyone can produce decoupled, orthogonal and tested code. I personally think that the biggest challenge here is understand the error messages from the compiler when you mess up with Google Mock macros.

###Advanced Topics & Honorable Mentions

There is two more chapters that I have to mention: Legacy Challenges and Threading (testing threading code) - both of them have been carefully crafted. In the Legacy Challenges chapter, Jeff talks not only about challenges but also explains and give examples (with "real" legacy code) about refactoring techniques, ending with the Mikado Method for large-scale changes.
The TDD and Threading chapter also has good explanations and examples about how to test-drive asynchronous solutions. Although I'm no expert with C++, I found both theory and practice (examples) very easy to understand using only my knowledge about threading from other languages and platforms.

###Conclusion

Even still missing two parts that I was eager to read ("TDD and Performance" and "Integration and Acceptance Tests"), I found the book very good and worth the price and time. After reading it I'm finally confident to write modern C++ code, mainly because of the fast feedback that TDD gives me. This book had another side effect on me: I'm now eager to finish the [C++ Primer](http://www.amazon.com/Primer-5th-Edition-Stanley-Lippman/dp/0321714113) and the Effective C++ series. :D