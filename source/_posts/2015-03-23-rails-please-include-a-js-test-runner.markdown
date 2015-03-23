---
layout: post
title: "Rails: Please include a JS test runner"
date: 2015-03-23 15:34
comments: true
categories: [Others]
---

For an opinionated framework like Ruby on Rails, I expect it to come pre-bundled with a CoffeeScript based test framework and test runner. Here's why:

* Teams don't need to waste time deciding which one to use among the mess of Jasmine, Mocha, Expect, Sinon, Chai, Bower, Grunt, Teaspoon, Npm... you name it.

* It'd work better than what's out there since the Rails team will focus on clean integration with Rails and will have a much larger user base to collect contribution from.


I'd prefer [Jasmine](http://jasmine.github.io/) to be the test framework and a runner similar to [teaspoon](https://github.com/modeset/teaspoon) that can run the specs both on the command line and on the browser, with asset pipelines enabled.

Rails made a huge promise to test automation by shipping with a nicely structured way to test the ruby code and doing the same for JS would make life better for so many people like me, I believe.