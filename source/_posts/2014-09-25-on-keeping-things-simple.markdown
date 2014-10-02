---
layout: post
title: "On Keeping Things Simple"
date: 2014-09-25 08:28
comments: true
categories: [Architecture]
---

Software architecture, or any design for that matter, needs to strike a fine balance between simplicity and power. Sometimes, the design needs to deliberately remove elements that'd make life better in some way but also cause a lot of frictions in other ways, ways that may not really impact the designer.

A few examples to explain this. Client side MVC frameworks on top of a server side MVC framework. Client side URL routing on top of server side URL routing. Persisting data in many different databases.

All these examples have one common theme: they offer some value in exchange of additional complexity, often time the complexity grows with time as future developments happen, eventually costing more than the value gained.