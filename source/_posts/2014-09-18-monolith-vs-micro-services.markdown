---
layout: post
title: "Monolith vs Microservices"
date: 2014-09-18 14:56
comments: true
categories: 'Architecture'
---

Microservices are talk of the town these days. I wanted to share my thoughts on microservices based on some experiments that we are running into at our current project.


![minons](images/minions.jpg)

SOURCE: [https://www.flickr.com/photos/31366276@N03/9327275207/in/photostream/](https://www.flickr.com/photos/31366276@N03/9327275207/in/photostream/)

Recently, we deployed a microservice for two-step verification feature on one of our projects. This was a strict business requirement, because having a separate server to store your 2nd-factor authorization provides additional security in case the servers hosting your primary factor are compromised.

We operate on a cloud environment, so spinning up new servers is a simple process. The source code for this whole Ruby on Rails based two-step verification service is no more than a couple hundred lines. So, in theory, it should be very easy to deploy such a service. However, it proved to be a lot of work in the end.

For example, to deploy this service, we actually had to spin __a few servers__ for each of our staging and production environments. They also had to be __load balanced__ for obvious reasons. They needed their __own database__ for the business requirements, which also needed automated __periodic backups__. __Networking and VPN__ related configurations as well as __DNS__ configurations were also required. __Monitoring__ tools had to be configured so we can get alerts in case things were about to fail. __Deployment scripts__ had to be written for this service as well.

  > All in all, I'd say __it took 20x the time to deploy this microservice than writing the code for it__.

Really. No kidding.

Since it has been deployed, we didn't ship changes as often to this service compared to our main project. This is what I find to be the primary benefit of this approach, since it doesn't require as big a regression test during our releases.

However, when things go wrong, our debug efforts are harder since more infrastructural pieces are involved. Considering the additional work required and the value gain, I'm really not sure if microservices provide any real ROI.

The additional complexity of dealing with many servers as opposed to a larger APP may or may not be worth it. I agree with Martin Fowler on the [prerequisites of microservices](http://martinfowler.com/bliki/MicroservicePrerequisites.html). Unless, you have streamlined an automated way to provision new servers with all required parts, it may actually be best for you to keep working on the monolith. It's not the end of the world, and you'll have more time to spend with the family!