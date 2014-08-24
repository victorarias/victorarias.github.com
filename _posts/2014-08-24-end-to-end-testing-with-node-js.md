---
layout: post
title: "End-to-end testing with Node.js - A rubyist view"
keywords: "node.js,acceptance,integration,javascript,capybara,zombiejs,websocket,express,mocha,chai"
location: Stockholm
short_url: http://goo.gl/iFcmRN
---

My first challenge when I decided to adopt Node.js to develop web applications was to find out how to write end-to-end/acceptance tests. My workflow to develop something new usually starts with a high-level feature test, so finding something like [capybara for Ruby/Rack](http://jnicklas.github.io/capybara/) and [RSpec](http://rspec.info/) was the logical first step.

<!-- more -->

I started picking up [Mocha](http://visionmedia.github.io/mocha/) as a test framework. It supports the BDD-style that I like in RSpec and has different kind of reporters, styles of writing tests (called interfaces), configurations and integrations that I'll probably never touch. I also added [Chai](http://chaijs.com/), an assertion library, to be able to write nice assertions like:

{% highlight javascript %}

var expect = chai.expect;

expect(foo).to.be.a('string');
expect(foo).to.equal('bar');
expect(foo).to.have.length(3);
expect(tea).to.have.property('flavors')
  .with.length(3);

{% endhighlight %}

Finding a test library was very easy since there is a lot of those in the JavaScript world (Ex: [qunit](http://qunitjs.com/), [jasmine](http://jasmine.github.io/)). My goal was to write something like this:

{% highlight ruby %}

describe "Creating stuff" do
  it "works" do
    visit root_path

    click_link "Stuff"

    fill_in "Name", with: "Wow such a nice thing!"
	select "Such a nice test", from: "Options"

    click_button "Create"

    expect(page).to have_content("Name: Wow such a nice thing!")
    expect(page).to have_content("Such a nice test")
  end
end

{% endhighlight %}

The best library that I found for this was [Zombie.js](http://zombie.labnotes.org/). Although not perfect, it is definitely the most capybara-like testing framework currently available in the Node.js ecosystem. The code above would have been written like this using Zombie.js, Mocha and Chai:

{% highlight javascript %}

describe('Creating stuff', function() {
  browserContext();

  it('works', function() {
    var browser = this.browser;
    browser.visit('/').
      then(function() {
        return browser.clickLink('Stuff');
      }).
      then(function() {
        return browser.fill('Name', 'Wow such a nice thing!').
          select('Options', 'Such a nice test').
          pressButton('Create')
      }).
      then(function() {
        expect(browser.text()).to.contain('Name: Wow such a nice thing!');
        expect(browser.text()).to.contain('Such a nice test');
      });
  });
});

{% endhighlight %}

Not bad, right? The only real cons is having to write plumbing to spin up the webserver and configure the browser (capybara handles all of this by himself). This plumbing is hidden inside the *browserContext* method call, right below the *describe*. Here is my current implementation to test an *Express* app using [*Socket.io*](http://socket.io/) for *WebSockets*:

{% highlight javascript %}

process.env.NODE_ENV = 'test';

const http = require('http');
var app = require('../../app/server.js');
var Browser = require('zombie');

module.exports = function() {
  beforeEach(function() {
    // glue the express app with a http web server
    var httpServer = http.Server(app);
    this.server = httpServer.listen(3000);

    // start to listen to websockets - app.messageBus is an abstraction above socket.io
    app.messageBus.start(httpServer);

    this.browser = new Browser({ site: 'http://127.0.0.1:3000' });
  });

  afterEach(function(done) {
    if(this.browser.window && this.browser.window.socket) {
      // websocket client-side cleanup - note how you can access the browser window object
      this.browser.window.socket.disconnect(true);
    }
    this.server.close(done);
  });
};

{% endhighlight %}

Since the test suite needs the *Express* app without the webserver attached, something like the code below is required to export the app object:

{% highlight javascript %}

const
  express = require('express')
  app = express();

// ...
// app specific code
// ...

module.exports = app;

// initialize the http server with websockets only if not required by another file
if(!module.parent) { 
  var httpServer = require('http').Server(app);
  var port = process.env.PORT || 3000;
  httpServer.listen(port, function() {
    console.log("Listening at " + port);
    messageBus.start(httpServer); // socket.io abstraction
  });
}

{% endhighlight %}

Although not ideal, all of this complexity can be hidden within just one method call in your test suite.

*Zombie.js* looks very mature and support a lot of stuff - *WebSockets, Server-Sent Events, cookies and WebStorage, XMLHttpRequest*... basically everything you might need.

I also found two alternatives to *Zombie.js*: *phantomjs* and the famous *selenium-webdriver*. I couldn't find any stable looking library that used phantomjs, so I wouldn't choose it right now (although I'm a fan of the *poltergeist* gem of *Ruby*). *Selenium-webdriver* looks usable, but I would like to avoid it if possible due to "not so nice" memories.

I'm writing tests like this in my [*dashy* project](http://github.com/victorarias/dashy) - a small framework to create "realtime" dashboards using *Node.js*, *WebSockets* and *Angularjs*. It's just an experiment for now (since I'm exploring different things and ways of writing this kind of app using *Node.js*), but it's improving every day and I expect it to be usable in a month. You can find there examples of different kinds of tests in JavaScript (unit, integration, feature/acceptance).