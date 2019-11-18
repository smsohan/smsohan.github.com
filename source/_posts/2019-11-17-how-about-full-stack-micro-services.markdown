---
layout: post
title: "How About Full-Stack Micro-Services?"
date: 2019-11-17 13:47
comments: true
categories: ['Architecture']
---

![patched quilt](/images/patched_quilt.jpg)

<small>
Source: [Audrey on Flickr](https://flic.kr/p/pUW8f9)
</small>

I think nobody knows how to stitch together an app with full-stack micro-services. I have the following open-questions if you disagree. Of course, if we could send people to moon, we could solve these problems. But the question is, is it worth and should your team  solve these problems? Especially, for small teams?

1. How to render the UI from tens of independent micro-services into the same web page?
2. How to ensure the JS and CSS libraries are compatible within all of the independent services?
3. How to aggregate logs from the services to be able to trace a user / request / transaction?
5. How to measure and reduce overall latency and spinner-fatigue?
4. Which off-the-shelf framework can be used for achieving the above?

Headless micro-services are easy to build, but in many ways are similar to integration over database. It helps scaling teams, but even if you have many teams, I'd say extract service where it makes sense instead of adopting that model as the default choice.

I suggest being careful about following conference talks and blogs on how cool micro-services are. It may work for big and gig tech. They don't always turn a profit! Your small teams are more likely to drown in worthless complexity from a micro-service architecture.