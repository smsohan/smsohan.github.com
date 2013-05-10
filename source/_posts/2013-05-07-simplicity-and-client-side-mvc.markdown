---
layout: post
title: "Simplicity and Client-Side MVC"
date: 2013-05-07 08:56
comments: true
categories: [JavaScript]
---

After spending about 6 months on this new project using BackboneJS, and spending some hours learning AngularJS and EmberJS, my realization at this point is:

> Use Client-Side MVC very Selectively.

Sometimes on a single page of your app, you need to offer a lot of interactions, each scoped to a small part of the page only. In such cases Client-Side MVC offers some neat features. I'll try to share my perspective with some concrete examples where I'd say yes/no to Client-Side MVC.

1. Build a Calendar page - Yes.
2. Build a Master/Detail view - No.
3. Build a Credit Card Payment Form - No.
4. Build a Story Wall like Trello - Yes.
5. Build an Airport Departures/Arrivals display - No.
6. Build a Search form - No.

As you see here, I suggest using it only when a lot of Client-Side interactions can happen, with little server side data requests.