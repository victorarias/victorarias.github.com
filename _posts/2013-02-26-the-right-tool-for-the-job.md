---
layout: post
title: The Right Tool For The Job
---

#{{ page.title}}

<p class="date">26 Feb 2013 - São Paulo</p>

I’ve been working on a new project for the past three weeks: It’s a kind of an e-commerce that sells advertisement space. The reason that motivated this post was the database chosen: MongoDB.

We used MongoDB successfully in many other projects, mainly because it’s faster to develop new features with it than with other databases like MySQL or PostgreSQL. The reason that it’s faster to implement and ship new features is because we don’t need to handle migrations or table structures, and even with Rails this can be a pain. What is more, at the end the MVP (Minimum Viable Product) was a success! We’ve shipped the app in a short period and the client was impressed!

So, what’s the problem? Relational data! Well, not only that, but let’s talk about it first.

We have five main entities: Chain, Store, Edition, Order and User. Below is a diagram that summarizes the relationship between these entities.

<img class="center" src="/images/2013-02-26/1.png" alt="diagram"/>

The application was firstly modelled this way because it’s the way the data is consumed and “flows” on the user interface, and also because this design meets the needs of the user in a leaner way. It’s also important to note that this modeling fits naturally with MongoDB.

The problem appeared when we started developing the administrative interface, especially with one feature that we always take for granted from SQL databases: querying. We needed to be able to query Orders based on many criteria like Edition (easy, direct link), User (easy, direct link), Store (nested) and Chain (“double” nesting), and MongoDB doesn’t have this querying capabilities baked in.

The solution was easy: denormalization! On MongoDB schema design it is natural that the Order should have a reference to the Store and the Chain, or at least their ids, names or any data that enabled the querying that the application required. It’s important to have a “denormalization mindset” when you are designing a NoSQL schema, otherwise you will end up fighting the database instead of being productive. I think that especially with MongoDB this can be a bigger problem than with other NoSQL databases because it has so many features that is easy to forget its limitations.

There is also another feature “missing” from MongoDB that have potential to be a problem: lack of transactions. When placing an order in our application the user is also reserving a scarce resource, and even with validations checking if the resource is available we can’t be 100% sure that, in a scenario with many concurrent requests, two orders won’t end up reserving the same resources.

Fortunately this isn’t a big problem because the backend workflow would identify the problem and send it to a manual follow up, but in the future we will probably need to address this.

So… do we always think pragmatically when we choose this kind of tool? Or can our past trauma (big balls of mud written in stored procedures against a bad designed schema) plus our urge to deliver blind us when making these decisions? Wouldn’t we have done a better job if we had chosen another database? Would PostgreSQL fit this application better in comparison to MongoDB?

I’m still unable to answer these questions, but I will reflect on that in the next few months that I continue to evolve this application. What do you think? What would you choose?
