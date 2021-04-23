---
layout: post
title: "Software Architecture - Topic 4 - Redis"
date: 2021-03-04 09:39
comments: true
categories: "Software Architecture"
---

Welcome back to the Software Architecture series. I know at least a few people from my team are following, and that's a great encouragement.

For today's post, let's focus on learning from a very popular and commonly used open-source project called [Redis](http://redis.io). To the developers, Redis is a
dead simple key-value store with a super simple API as follows:

```bash

$ set today 'Thursday'
OK

$ get today
Thursday

$ set temp 20
OK

$ incr temp
21

$ incrby temp 3
24

$ get temp
24
```

Of course Redis has more advanced features, but not too many. I think Redis is a delightful system. It's fun to use and has a reputation for being incredibly fast and scalable. I'm going to recommend you to spend some time on [Redis architecture](https://docs.redislabs.com/latest/rs/concepts/) and see if you understand the concepts to confidently answer the following questions:

1. Why is Redis so fast?
2. What can you do to prevent data loss when using Redis?
3. How does Redis distribute its data to multiple nodes?
4. What happens when you add a new node to a Redis cluster?
5. What happens when you remove a node from a Redis cluster?
6. How does Redis allow you to have an even distribution of the data in your cluster?
6. How can you build resilience using Redis when a whole datacenter fails?
7. How does a Redis client discover which Redis server to go to?
8. How do you know if you have enough capacity in your Redis cluster?
9. How does Redis provide end-to-end encryption?
10. If a Redis cluster dies, how can you restore it?
11. What metrics would you use to monitor if your Redis cluster is healthy?


My plan is to introduce you to a bunch of open-source systems like Redis and ask similar questions. The idea is, after going through a few of these systems, you'll start to see patterns and trade-offs for each. Being familiar with real-world systems and seeing the patterns in use, I hope you'll be able pick and choose the patterns that best fit your system requirements, environment, and people.

Happy learning!