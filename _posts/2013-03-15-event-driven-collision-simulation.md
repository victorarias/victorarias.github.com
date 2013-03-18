---
layout: post
title: Event Driven Collision Simulation
keywords: ",algorithms,javascript,coursera,canvas,html5,priority queue"
location: São Paulo
---

I’m attending the Algorithms 1 course on Coursera, from professors Robert Sedgewick and Kevin Wayne, and so far it’s been great. It’s nice to remember and improve my knowledge in algorithms while coding solutions to unusual problems like the 8-puzzle problem (solved using the A* algorithm), and all of that for free! :-) This post is inspired on one of the applications of priorities queues: event driven simulations.

After studying sorting algorithms (selection, insertion, shell, merge, quick and heap sort) we were briefly exposed to collision simulation and its naïve O(n^2) and improved O(lg N) solutions.

Since the improved solution is SO MUCH better in theory, I decided to try it myself, implementing both solutions with JavaScript on a HTML5 Canvas, a technology that I always wanted to use but had never found an opportunity.

Both implementations use the same structure of *initialization/resource/drawing/warmup* but they differ mostly on its update mechanism: the function responsible for calculating collisions between circles and walls and then moving them based on its direction and speed.

The simplistic approach uses a O(n^2) loop to check for collisions between every single particle since every one of them is a candidate to be at the same place at the same time, and only after that the postioning is calculated.

The event driven approach has a very different 	nature: its strategy is to predict collisions, keeping them in order and executing them one by one. This approach is powered by a priority queue that has both insert and remove operations with O(lg N) time.

Even having these solutions written in other languages and platforms they weren’t easy to implement, mainly because of the language. Don’t get me wrong: JavaScript is a great language, but it has so many “gotchas” that it took me a week of “freetime coding” to accomplish a bug-free solution.

People tend to praise JavaScript and even put down who criticize its problems, but I personally think that this is a childish behavior. I acknowledge JavaScript importance and I know that is possible to achieve great things with it, but no one can say that it isn’t unnecessarily hard to write complex systems with it.

Just by reading my code now I remembered five topics that it can be tricky:

-	Equality (==) and strict equality (===) operators: [http://stackoverflow.com/questions/359494/javascript-vs-does-it-matter-which-equal-operator-i-use](http://stackoverflow.com/questions/359494/javascript-vs-does-it-matter-which-equal-operator-i-use)
-	Function Scoping: [http://stackoverflow.com/questions/2295281/javascript-function-scope](http://stackoverflow.com/questions/2295281/javascript-function-scope)
-	Absence of integer type: you can’t rely on integer divisions :(
-	Optional parameters: there’s no support for that and if you misguidedly do an if(parameter) bad things may happen because values like 0 evaluate to false.
-	Inheritance: this is a big and complex topic. There isn’t any classical (from classes) inheritance in JavaScript, so you need some plumbing code to simulate the behavior that is normally expected. I used in my code a simple prototypal inheritance ([http://javascript.crockford.com/prototypal.html](http://javascript.crockford.com/prototypal.html)), a lot different from what people expect in inheritance. I’m thinking about writing another blog post on this subject in the future to help “pinning” inheritance patterns in my head. 

So, let’s go to the code. It can be found in this repository: [https://github.com/victorarias/collision_system](https://github.com/victorarias/collision_system)

Below is a picture of the running code and a link to the demo:

<div class="thumb">
  <a href="/images/2013-03-15/1.png" target="_blank">
    <img class="center simple-border" src="/images/2013-03-15/1.png" alt="screenshot"/>
  </a>

  <p>
    <a class="btn" target="_blank" href="/demo/event-driven-simulation/cool_index.html">Demo</a>
  </p>
</div>

The main files are listed below:
-	priority_queue.js: data structure that enables the cool solution.
-	circle.js: type to represent a circle on the canvas.
-	collision_system.js: base type to run startup code and the main update+drawing code.
-	simple_collision_system.js: O(n^2) implementation
-	cool_collision_system.js: O(lg n) implementation
-	profiler.js: type to “hijack” functions and measure performance (just a simple arithmetic average)



Let’s take a look at the commented implementation of both implementations:

<script src="https://gist.github.com/victorarias/5163882.js">
</script>

<script src="https://gist.github.com/victorarias/5164279.js">
</script>

These implementations have very different performance considerations. Let’s look first at some raw numbers:

- Number of particles: 1000
- Simple 0(n^2) implementation: ~25 FPS
- Cool O(lg n) implementation: ~58 FPS

These aren’t impressive numbers, right? On my machine 1000 particles is the point that the simplistic solution is at its limit: the browser can’t hold the animation above this. This is expected on the simplistic solution, however I expected a different result from the cool solution – at least this was what I thought when I first saw the numbers.

After some study I realized why this happened:

1. The cool solution is constrained not by the number of particles but instead by the number of collisions (so a change of radius will change the measured number).

2. The cool solution aim to a smooth simulation and not only to an animation, paying with “animation speed” instead of “animation quality”, meaning that the particles will decrease its velocity instead of start jumping on the screen.

The result is that the cool solution can actually handle much more particles in the simulation, but it will slow down the particles speed on big loads. Tweaks can be made to increase the speed on these scenarios (decreasing/increasing drawing rate, limiting even more the size of the priority queue, etc.), but these tweaks may not play well at common scenarios.

Coding these algorithms was a fun and very rewarding job. I’m a fan of deliberate practice as a technique of skill improving and this was a perfect example of it: a well-defined task, challenging but doable with easy feedback and a lot of opportunity to correct errors.

I must thank Coursera for the opportunity of taking these classes for free. Sincerely, I would pay for this kind of knowledge and exposure. I’m anxious to complete this course and start its second part! 
