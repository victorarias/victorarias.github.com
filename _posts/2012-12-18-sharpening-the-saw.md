---
layout: post
title: Do You Sharpen Your Saw?
keywords: ",coding,algorithms,F#,code design,oo,object orientation,functional,tests,design patterns,agile, lean"
location: São Paulo
---

What do you do to improve yourself? Do you do something to perform well at work, to delivery better software in less time?

<!-- more -->

In the day-to-day routine is easy to lose yourself. Our job is usually very time-consuming and many different aspects of life (feeding the cats, walking the dog, etc.) leave us with almost no time to improve ourselves. This normally leads to obsolescence, lack of interest and other bad things. 

What people and some companies usually don't realize is that spending time practicing and studying leads to greater productivity, so at the end of the day spending time not working can make you more productive – it’s obvious, but true. It is like the story of the lumberjack that was so busy sawing with a dull saw that he had no time to sharpen it. ([Codding Horror - Sharpening The Saw](http://www.codinghorror.com/blog/2009/03/sharpening-the-saw.html))

Thinking about this, I made a list of topics that a good programmer should invest some time on as well as some good books and others resources:

##Paradigms & languages

Do your code usually looks like spaghetti? Or do you only know object orientation? If so, learn a functional language! Do you only program with strong typing languages? You should try a dynamic one!

By learning a language with another paradigm you will expand your brain capacity, increasing your solutions arsenal and creativity. Example: Learn Scala if you know Java, F# if you program with C# or even a "pure" functional languages as Haskell. Languages shape the way we think, solve problems and write code, so by learning a different language, even if you will not use it in your job, you will become much better with the ones that you use every day. You can google this subject to find interesting discussions and stories about this.

Another way to improve yourself is to learn a language from a different "level" than the ones you know. For instance, it is useful to learn C/C++ if you are a ".NET programmer" that only knows C#/VB.NET and languages like JavaScript, T-SQL, etc., or obviously the opposite if you're a C/C++ developer.

##Algorithms

How much do you know about basic data structures? Do you know or remember how to implement a heap, or the Dijkstra algorithm? Studying algorithms is a very nice way to practice your logic and coding skills and it also can become handy in your day-to-day routine.

A good type of exercise to improve your algorithms skills is to solve computational puzzles and questions from programming contests. This is also a great way of preparing yourself to code interviews!

If you already know the “classic” algorithms you can practice with ones that you may not know from topics like machine learning, natural language processing, etc.

####[Algorithms (4th Edition) - Robert Sedgewick, Kevin Wayne](http://www.amazon.com/Algorithms-4th-Edition-Robert-Sedgewick/dp/032157351X/)

I really like this book. It's a great "introduction" of algorithms and algorithms analysis, much less complex than Cormen (Introduction to Algorithms - 978-0072970548) but undoubtedly still very useful. It has a very direct approach to the topics with simple and almost "generic" Java code, although you still need some math to fully understand some thoughts and proofs. To help consolidating the knowledge it has a good amount of exercises at the end of each chapter, and you can find a summary of the content in its booksite. (http://algs4.cs.princeton.edu/home/).

####[Project Euler](http://projecteuler.net/)

From this website: "Project Euler is a series of challenging mathematical/computer programming problems that will require more than just mathematical insights to solve. Although mathematics will help you arrive at elegant and efficient methods, the use of a computer and programming skills will be required to solve most problems."

####[UVa Online Judge](http://uva.onlinejudge.org/)
Gallery of problems used in programming contests.

##Code design

Do you really know how to make your code efficient? Maybe this topic is the most useful and important one for the everyday software development, and it is also one of the most overlooked. The job of creating decoupled, clean and efficient code can become very hard, as result of the amount of techniques plus the variation of scenarios and the pressure to delivery. Acquiring this knowledge and really understanding it is important and also expensive both in time and effort.

To become better at code designing you must read a lot about SOLID principles, DRY and approaches like Domain Driven Design. A related field of knowledge is design patterns, but you must be careful to not overestimate them. Despite this, I think that the most important action that one can take is also the simplest one: to read and to understand code. Choose different open source projects, read and understand them. I’m sure you will learn a lot.

Another crucial topic is automated testing; today some people even say that code without tests isn’t a professional one (a bit harsh, but testing is very important). Do you practice TDD (Test Driven Development) and/or BDD (Behavior Driven Development)? Doing TDD correctly usually leads to a better design, or at least doing it reveals design problems that you didn’t know you had.

####[Growing Object-Oriented Software, Guided by Tests - Steve Freeman, Nat Pryce](http://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627)

This is an amazing book about how to create an object-oriented software from its inception to the end using TDD. It approaches subjects like: the "setup" actions of a project such as creating a "walking skeleton" to enable end-to-end testing from its beginning; design techniques looking forward to a clean, expressive and sustainable code; test techniques to create a valuable test suite. It ends with some advanced testing topics like testing concurrent code, persistence and managing complex data.

####[Clean Code: A Handbook of Agile Software Craftsmanship - Robert C. Martin](http://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)

Great book about principles, patterns and practices. It covers from basic topics to complex ones, first with theory then with case studies, finishing with a comprehensive list of code "smells".

####[Design Patterns: Elements of Reusable Object-Oriented Software - Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides](http://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612)

It's a classic book about design patterns. Unlike some people I do not think that it is a MUST READ book, although I acknowledge its importance and this is why I'm listing it here. Because it is an old book, it must be read with caution because some of its patterns are trying to solve problems that we haven't anymore. Nowadays, Ayende is doing a nice review about this book and the impact of time on it: http://ayende.com/blog/tags/design-patterns-test-of-time

##Processes

It’s important to master the technical aspects of software development, but we are not alone in the world and we have, almost every time, to work for someone with a team composed by other developers, DBAs, designers, etc. Because of that it's important to know about development processes as well as to be a ninja programmer.
I really like agile methodologies and I think that knowing at least the basics from Scrum, XP (Extreme Programming), Kanban and Lean is very useful, mainly because they focus on delivering fast, really fast, making adaptations based on the feedback received from the people who uses the software.

##What I Do

I try to include sessions of studying and practicing these topics in my daily schedule, often before I go to work, when I try to take advantage of the fresh mind and the accomplished feeling that it gives me for the rest of the day.
Besides these topics and techniques, it’s important to periodically reevaluate your studies to see what works best for you, always looking for better ways to study. Sometimes I even create sheets to track my improvement with different routines, studying techniques and subjects.

For the reason I always search for better ways to improve myself, I often read articles and books related to the field or about better ways to train my skills. With that in mind, there is another book that I find special but I can’t fit it into any category: The Pragmatic Programmer, from Andrew Hunt and David Thomas. (http://pragprog.com/the-pragmatic-programmer)
It contains a great set of tips that are useful to developers from any experience level. I first read it in the beginning of 2010 and it blew my mind, and even today I find it useful when I occasionally read it. Because of this post I’m thinking about reading it again and I’ll probably write a review highlighting its better parts.

Like everything in life, it takes time and effort to be good – talent isn’t enough. So... What do you do to sharp your saw?
