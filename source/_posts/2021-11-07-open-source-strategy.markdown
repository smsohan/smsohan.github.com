---
layout: post
title: "Open-source: If You Build it Right, They'll Come"
date: 2021-11-07 12:51
comments: true
categories: "Strategy" 
---

I find open-source to be a rather loaded term; it means different things to different people. For this blog post, I'll imagine there are two kinds of open-source products:

1. Company-owned open-source
2. Crowd-owned open-source

I'm wiring this blog post specifically about company-owned open-source products. In a typical company-owned open-source product, the company pays us to develop and maintain the product. I've been working on the [Azure Communication Services UI Library](https://aka.ms/acsstorybook) that falls into this category. We have full-time developers at Microsoft that work on this product. In the past few weeks, I was researching to come up with an open-source strategy for us. Here I'm sharing what I learned in the process. These findings can help others who are on the same boat.

Here's a quick table of the community engagement modes for open-source products. The higher the layer, the more complex it gets.

|Engagement Mode|Key Enabler|
|----|---|
|Change existing code / architecture| Community meetups and conferences
|Add new features| __Extensibility API__, Public facing product backlog
|Fix bugs| CI, contribution guide, coding style guides
|Fork and customize| Permissive license 
|Discuss issues and questions|  Developer forums
|Report issues| Issue reporting tools and triage process
|Browse code| Readable code and docs
|Download and use| Product is released as a package

I highlighted the topic of __extensibility APIs__ because that was my key observation about making successful company-owned open-source products. For example, VSCode is a hugely popular product and benefits greatly from the community contribution in the form of extensions. The beauty of this process is, it provides a clear separation of concerns. The core team can focus on their company-mandated work while the community can build and maintain the plugins independently. This also allows the community to contribute without having to understand the complex internals of the core. An extensibility API is useful beyond just for the open-source community.

If you want the community to come and contribute to your open-source project, carefully design the core product with extension points in mind. Then, ensure the ergonomics are great for the extension developers. Give them well-documented and thoughtfully-designed APIs. Ensure that they have automated testing and publishing tools for the plugins. Make the plugins easily discoverable so that users can find and use compatible plugins while using the open-source product.

I have another recommendation. A company-owned open-source product should publish their community engagement strategy for clarity. For example, it's better to be upfront if the core team isn't ready to accept community code contributions. We should choose a suitable strategy and evolve it over time as the product itself matures. 

