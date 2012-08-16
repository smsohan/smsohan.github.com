---
layout: post
title: "Deploying a Java Application Using Capistrano"
date: 2012-08-07 20:18
comments: true
categories:
---

In our current project, we have a little Java Bridge, that reads JMS messages from an enterprise service bus (ESB) and pipes it to an ActiveMQ topic. This had to be written in Java, or some flavor of it like JRuby/Scala, because the ESB only allows their signed client libraries to interact with their API.

Since, we were already using Capistrano to deploy our Rails/Nginx app, we wanted to leverage the same tool for deploying the Java Bridge. This would also make maintenance easier, since the same standards is used for deploy locations/log files/roations etc.