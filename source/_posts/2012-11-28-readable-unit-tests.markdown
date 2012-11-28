---
layout: post
title: "Readable Unit Tests"
date: 2012-11-28 10:10
comments: true
categories: [TDD]
---

From my experience with writing/reading unit tests for years, here's a little guideline to keep 'em readable:

1. A unit test needs to fit entirely (including setups) on a screen without scrolling.
2. It is OK to have a little bit of duplication in unit tests for readability.
3. Avoid nesting of contexts beyond 2 levels. Instead, use methods to setup and flatten.
4. Do not use if/else/loops in a unit test.
5. If your test needs too much setup/mocking/stubbing, time to refactor the code.

A simple readable unit test is only achievable when the code itself is simple. Adhereing to these guideline will probably make the code simpler as a direct impact!