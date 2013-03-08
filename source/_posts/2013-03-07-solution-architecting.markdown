---
layout: post
title: "Solution Architecting Using Queues?"
date: 2013-03-07 14:44
comments: true
categories: [Architecture]
---

When building a bunch of applications that need to interact with each other, Queues or [Message Oriented Middleware](http://en.wikipedia.org/wiki/Message-oriented_middleware) services offer some very useful features. To name a few, you get features like a) __Guaranteed message delivery__, b) __routing__, c) __throttling__, etc., for free. Tools like ActiveMQ, RabbitMQ, MSMQ, JMS servers are tuned and time tested to handle such requirements robustly. I've had good experience with these tools for the happy paths. However, when dealing with unhappy paths, irrespective of the tools you use, it can be quite challenging to design the failovers.

Here's a list of the things to consider when designing a solution using Queues:

### Speed of Consumption

Queues are essentially a producer/consumer or pub/sub system. This means a slower consumption rate results into a pileup of messages. In a high transaction system, millions of records can pile up in the matter of hours - forcing the producer to slow down or messaages being dropped. While architecting a solution, the rates should be measured and tested. Load tests are very useful to avoid such production issues. These issues are generally hard to fix once rolled out.

### Error Paths

Even when a consumer is keeping up with the producer, chances are some messages cannnot be processed due to errors. If it's a network error or some other error external to the system, a simple redelivery from the queue may fix it. But in real life, chances are, this is an application defect. So, a simple retry won't make any difference. The application defect needs to be fixed and then, the message needs to be replayed. The solution needs to be designed considering the effect/resolution of the error paths.

### Automated Monitoring

Automated monitoring is crucial to any Queue in the production system. Without monitoring, the queue may be flooded with unconsumed messages.

### Human Intervention

When a message cannot be processed by a consumer, chances are, a human intervention will be required to complete/undo the intended operation. For example, if your payment processing application sends out a message on complete, and the shipping processor fails to process it - a human interaction may be required to fulfill the shipping or undo the order. When designing a solution, such edge cases should be identified.

### Deployments

Typically queues are good in terms of holding the unconsumed messages for disconnected consumers. However, if a consumer has significant downtime between deployments, this can stretch the limits of the queue.


I'm sure this is not all. These are from my real-life experience only. If you have experiences to share, please use the comments below.

