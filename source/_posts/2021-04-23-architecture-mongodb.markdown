---
layout: post
title: "Software Architecture - Topic 5 - MongoDB"
date: 2021-04-23 10:19
comments: true
categories: "Software Architecture"
---

Continuing on this architecture series of posts. Similar to the [post on Redis](/blog/2021/03/04/architecture-redis/), this time let's focus on another hugely popular distributed database
called [MongoDB](https://www.mongodb.com). If you aren't familiar with MongoDB, it's a distributed database that allows you to store and query humongous amounts of JSON-like data.

To get an overview of MongoDB and its architecture, you can watch the following YouTube video:

<iframe width="560" height="315" src="https://www.youtube.com/embed/oH-gQ4JdXQc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Of course, you can also download the [official architecture guide](https://www.mongodb.com/collateral/mongodb-architecture-guide) to learn more. 
Let's focus on rationale behind MongoDB's architectural decisions. You can find answers to all of the following questions
if you went through the guide or the video. But my goal is get you to think about the why behind the decisions.

MongoDB has databases and collections. While collections are conceptually parallel to tables in traditional relational databases, MongoDB doesn't impose
any restrictions on the schema of a collection. Can you think of reasons behind this decision? What trade-offs come to your mind? For example,
not having a schema allows you to store anything within a collection, essentially reducing the need for schema migrations on very large databases.
But at the same time, it may complicate consumption of the data. What other trade-offs can you think of?

MongoDB uses a router named Mongos and needs config servers to route queries when data is sharded into multiple partitions. What benefits and challenges do you see with the addition of this router component?

MongoDB requires you to have uneven number of replicas to deal with failover when a new primary node needs to be elected. How do you judge this design decision?

By design, MongoDB allows you to choose a write concern at write time to allow you to balance your need in the [CAP](https://en.wikipedia.org/wiki/CAP_theorem) triangle. Can you explain what you must do
 to ensure you'll not lose data is 2 out of 3 of your replica servers were to go corrupt?

MongoDB allows you to run complex map reduce queries in the form of aggregate pipelines. In a distributed database, which component within the MongoDB ecosystem
is responsible for aggregating data that spans multiple nodes?

If you followed the Redis post, what would you say are the top 3 architectural difference between MongoDB and Redis? Can you reason about the why behind the
differences?

Based on MongoDB's architecture, where do you see a potential bottleneck that can affect scaling a MongoDB cluster? What options do you have to work around?