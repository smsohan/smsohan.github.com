---
layout: post
title: "LoveJS Presentation at CAMUG"
date: 2012-09-13 21:32
comments: true
categories: [Javascript]
---

I paired with [@TylerMercier](http://codecuriosity.com) as we did a little [hands-on demo on writing testable and OO JavaScript](http://www.meetup.com/Calgary-Agile-Methods-User-Group/events/79722562/).

![Presenting with Tyler](/images/LoveJS.jpg)

This was mostly based on our pairing experience on the current project, but also had things that we learned from our previous web projects. I wanted to share some of the highlights of this session with my readers on this blog:

From experience, I have seen a few charactersitics that make JavaScript coding a real fun. Here's a few that stand out:

>Good JavaScript is Testable and Object Oriented

Really simple stuff, but it took me quite a few years to actually start writing OO JavaScript, and more so, to write automated tests that run on CI server. As I often say, the shittiest part of most web projects is its CSS, with its hacks, and the inherent nature of it. And JavaScript is often just as smelly. But if you just start writing OO JavaScript with tests, your code will drive you to a cleaner path simply because bad code makes testing really tough. And with Jasmine, testing JavaScript is super fun. So, we suggested the following toolset:

1. CoffeeScript
2. Jasmine
3. PhantomJS
4. CI integration

The materials related to this session are available online at [github](http://codecuriosity.com/love_javascript).

It was a great turnout, full house, engaged audience, and, asking really good questions. I had a lot of fun doing the hands-on. You know live demos can be daunting, especially when you are writing on a low res screen so people can actually read the projected text. But thanks a lot to Tyler, we had done a couple dry runs of the demo. That helped us a lot in terms of finding the points to transition between us, as well as some muscle memory as we were writing the code live in front of the audience.

But, I also wanted to share a checklist that we should have done as a preparation for this talk. It's too bad, we didn't do the following, hopefully this will be better next time. Here's what I have in my mind:

* **Audio/video record** the session. So, I can see myself afterwards and find some obvious rooms for improvement.
* Practice using **real projector** so we can find the best theme for the room size. This time I felt some of the text colors weren't as readable for the back benchers.
* Never plan for a 50 minute talk when you have a 60 minute slot. Leave **at least 20 minutes slack**. Have a backup plan for an extension in case you are done early. It sucks to cut short or rush to the ending.

But in retrospect, I was super impressed with how the session ran. Hopefully, with practice it will only keep getting better.

