---
layout: post
title: "Tests Are Your Best Friends: Do Not Neglect Them"
keywords: "tdd,test driven development,tests,automated tests,automation,broken windows,legacy code"
location: São Paulo
short_url: http://goo.gl/qxGAk
---

In the last 2 years I worked at a company that fully embraced **Test Driven Development** (TDD) and automated testing in general. During that time no one around me questioned it: I had to create automated tests for every production code I wrote, and it had to be done preferably through TDD. The people from there even used to say that "code written without TDD isn't a professional one".

I must say that I disagree with the whole "TDD Is The Only True Way" story, but I thoroughly  believe in something closely related: ***"production code without automated tests isn't a professional one".***

<!-- more -->

Although TDD is more a matter of code design than testing, it's known that it produces a wide suite of automated tests that not only can be used as a tool to guarantee that the code behaves as intended, but also as a live specification, given that it was made by an experienced TDD practitioner. However, there are many ways of developing software and TDD is just one of them.

During my time as a professional developer I can certainly say that:

- I wrote a good amount of high quality code without TDD in the past.
- I know a lot of great developers that don't practice TDD and can still deliver good code.

The problem isn't not doing TDD but instead not creating automated tests to ensure that the written code will continue to behave as designed. It's obvious that it takes more time to deliver something when creating tests is part of your development process, but not doing it will result in a enormous liability, with probably disastrous consequences in the future.

Every software that is used will be modified, and that can't be done with confidence without any kind of automated testing - every change can break something, and a lot of times that broken feature goes unnoticed until the application is deployed and customers call the company complaining about some nasty bug.

Lack of confidence brings another problem: fear. Developers with fear never refactor code, even if it is screaming with [broken windows](http://www.codinghorror.com/blog/2005/06/the-broken-window-theory.html), and neglect to fix "broken windows" accelerates the rot of the application faster than any other factor.

Legacy code without tests can be tackled - books like [Working Effectively With Legacy Code](http://www.amazon.com/Working-Effectively-Legacy-Code-ebook/dp/B005OYHF0A/) are dedicated to help developers do that - but is always best to prevent the disease instead of remedying it. And it's cheaper too!

Do not forget to create automated tests, even if just for the main features. You're not saving time by not creating them - at best you're just delaying something, but you're probably creating a big problem in the future.
