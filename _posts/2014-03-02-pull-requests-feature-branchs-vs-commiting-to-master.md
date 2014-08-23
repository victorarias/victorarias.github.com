---
layout: post
title: "Pull Requests vs. Committing to master"
keywords: "pull requests,git,continuous integration,ci,continuous deploy"
location: Stockholm
short_url: http://goo.gl/2PpLSE
---

When working on a project, committing and pushing to master is a no-brainer. Using feature branches is a logical step forward in a development process, but merging a branch back to master isn't always a piece of cake, right? In that case, pull requests to the rescue!

Until less than two months ago, working with feature branches and pull requests were, for me, "the only way". Then I started working at [Barsoom](http://www.barsoom.se)/[Auctionet](http://www.auctionet.com), and there we usually work direct on master.

<!-- more -->

The reason we work like that is our continuous deployment process. After every commit is pushed, the following happens:

* A test run is triggered at our CI server. The CI server distributes the test execution to [testbots](https://github.com/barsoom/testbot), so tests are ran in multiple machines at the sime time.
* If the tests passes, the new version of the app is deployed to a staging environment.
* A new version of the system can be deployed to production at any time after that with just one message to our chat bot.

This whole process can be monitored in our development dashboard and status messages are sent to our development chat room. Retrying "builds" and deploying to production is completely trivial for us. In practice, we end up deploying new versions of our apps to production many times a day. **Our smaller apps are automatically deployed to production after a successful test run and we're considering making this the default behavior of our main app.**

![Dashboard](/images/pipeline_monitoring.jpg)

Even though I really enjoy working with pull requests, using PRs are not the obvious decision for me anymore. Since I'm still making my opinion on the subject, I summarized below the main differences between the two processes as I see now:

####Pull Request's pros
* Easy diff and review: GitHub's interface for PRs are really great for code reviews.
* History can be rewritten before merge to master.
* The contributor has a great interface to add a good description of the feature/changes. [Here is one example of that.](https://github.com/bitly/dablooms/pull/19)

####Pull Requests cons
* It can take too much time to integrate the code, so merge may be difficult.
* Longer period of waiting to deliver and deploy changes.
* Rewriting history is not a good idea when more than one developer is pushing to the same branch.

####Committing to master pros
* Continuous Integration/Delivery/Deployment for real, with crazy fast feedback loops.
* Merges conflicts are rare.

####Committing to master cons
* Hard to do code review.
* History "can't" be changed, so you can expect a lot of reverts.
* Feature toggles are required to hide incomplete features.

Doing effective code reviews is the biggest challenge that we have right now. We're making some experiments and doing commit-by-commit reviews with specialized tools may be the best option. We don't know yet.

I'll probably have a strong opinion in the next few months, but my current decision process on the matter is:

* Are you working in an app (not a library) and have an automated deploy pipeline? Commit to master.
* Otherwise, use feature branches + pull requests.

