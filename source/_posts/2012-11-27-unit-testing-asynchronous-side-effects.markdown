---
layout: post
title: "Unit Testing and Sleep"
date: 2012-11-27 15:01
comments: true
categories: [TDD]
---
If your unit test code needs a sleep, its time to refactor it. Ideally, you'd only need to stub/mock the asynchronous call instead of introducing arbitrary sleeps in the test code, because they will eventually fail. Moreover, until it fails, it will slow down your test runner.


