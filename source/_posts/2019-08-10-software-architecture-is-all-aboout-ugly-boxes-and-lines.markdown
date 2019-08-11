---
layout: post
title: "Software Architecture is all about Ugly Boxes and Lines - My Wishlist"
date: 2019-08-10 20:28
comments: true
categories: [Architecture] 
---

In my last post, I claimed [software architecture is all talk and no show](/blog/2019/08/02/all-talk-no-show-software-architecture/). When we have a visible one, it's a bunch of poorly drawn boxes and lines. I don't have a problem with boxes or lines, but I do like beautiful drawings.

Despite many standards, we still mostly use  whiteboard drawing of boxes and lines for sharing software design as we build new systems or introduce new team members. Where it sucks is the lack of evolution and context of the rest of the system that's not drawn on the board.

A digital repro of software architecture diagrams often happen in PowerPoint or similar tools that allow us to draw boxes and lines. This process is so rough that people just give up.

At work, I have been using [WebSequenceDiagram](http://www.websequencediagrams.com). While it's still not an eye-candy, I like the fact that you can draw a diagram from using plain text. Consider this as an input to create the accompanying diagram:

```
title Toilet Flush System
User -> Flush Lever: Push
Flush Lever -> Outlet Valve: Open
Outlet Valve -> Toilet Bowl: Water
Outlet Valve -> Inlet Valve: Open
```

![Sequence Diagram](/images/Toilet_Flush_System.png)

While this text to sequence diagram is a great achievement for a tool, I don't see such tools for software architecture diagrams. Here's my wishlist of features that I'd want in a software architecture tool:

1. **Text input**. Allows us to easily create the diagrams and use all the version control features.
2. **Map like UX**: Allows us to easily transition between higher and lower level components.
3. **Beautiful**.

Do you know any? Do these requirements make sense?
