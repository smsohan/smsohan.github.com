---
layout: post
title: "Software Architecture - Topic 6 - Slack and Microsoft Teams"
date: 2021-06-18 15:23
comments: true
categories: "Software Architecture"
---

Most applications have a request-response based single-channel data-flow. In such systems, human or software triggered requests are served by software provided responses. For example, when you make your DuckDuckGo search, you initiate a request and their server produces a response back to you. Realtime multiplayer systems are quite different because the pattern of information flow is more complex, often being a two-way or many-to-many data flow, with strict latency constraints. For example, when you chat with a bunch of friends, or join them for a video call, the data-flow is quite different than when you watch a YouTube video.

I found two great talks from the folks at Slack about "How Slack Works" and "Scaling Slack". The nice thing about these two talks is they are presented one year apart, and it gives a great view into the challenges with designing a realtime multiplayer system in the first place, and then evolving the design to meet scaling needs. To an aspiring architect, these two talks can provide a real-life example of thinking in terms of evolutionary architecture as a vital tool to strike a balance between upfront and just in time design.

<iframe width="560" height="315" src="https://www.youtube.com/embed/WE9c9AZe-DY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/_M-oHxknfnI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

I also recommend you to check the architecture of Microsoft Teams. The contrast between Slack and Teams design will show you the stark difference between the two approaches. The key difference between the two is Slack was built from scratch, and Teams was built on top of a whole bunch of existing services such as Skype, OneDrive, Sharepoint, etc. As a result of different organizational dynamics, the two products are quite unique in their architecture even though there's a major overlap of features offered by both.

<iframe width="560" height="315" src="https://www.youtube.com/embed/I33pQ9PUHNc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Once you get a chance to watch these talks, I recommend taking some time to think about the main conceptual design elements of realtime multiplayer systems. For example, patterns of many-to-many communication channels, low latency data-flow, security and access control, etc.