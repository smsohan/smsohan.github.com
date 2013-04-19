---
layout: post
title: "Unconventional Conventions"
date: 2013-04-19 09:33
comments: true
categories: [EmberJS]
---

JavaScript has been reborn, thanks to [jQuery](http://jquery.com) to begin this epic and wonderful march. These days, we have a huge influx of micro to macro frameworks, all providing some well curated set of features. Every now and then I try to get a glimpse of the new and hot stuff so when time comes, I can make an informed decision on what to use and why.

This post is about my recent attempt at learning [AngularJS](http://angularjs.org/) and [EmberJS](http://emberjs.com/). Client-side MVC and/or single page apps need some core features as follows:

1. URL routing
2. Rendering views
3. Read/write data from/to the backend server

Both AngularJS and EmberJS offer these and some additional features, most notably, [two-way data binding](http://emberjs.com/guides/object-model/bindings/), so a change in the JavaScript object reflects on the rendered view and vice versa. But, to be honest, I think they overstretched the framework N miles to the north of where it should be. Let me explain with an example:

Conventions are good when they offer an easy mental model. For example using the following router mapping:

```javascript
App.Router.map(function() {
  this.route('favorites');
});
```

it would match `favorites` with a `App.FavoritesRoute` based on the names. It's an easy enough pattern to recognize and remember. However, I find it's too far fetched in the following example:

```javascript
App.FavoritesRoute = Ember.Route.extend({
  model: function() {
    // the model is an Array of all of the posts
    return App.Post.find();
  }
});
```

My mental model is challenged in a few ways here:

1. Route has a `model` hook - which, by conventional usage, I can't seem to recognize as a pattern.
1. `App.Post.find()` entails an `Ember.ArrayController`, one provided by the framework. Here, a router is producing a `Controller` in disguise from the `model` hook.

I'm sure with repeated usage, I'd be able to use these conventions without much of a problem. But on a pure API design/Architecture perspective, I think it'd make more sense to remove such unconventional conventions.

To be honest, I've attempted the EmberJS guides for the 3rd time now, in less than a month, and I feel lost in so many conventions. I have had similar experience with learning AngularJS as well.

With AngularJS, I was first a bit skeptical about all the custom tags that you'd introduce when using AngularJS. But after giving it a go, it sort of made sense as a trade-off between less code and manageable clutters. But I got really uncomfortable with the likes of the following:

```html
<ul class="phones">
  <li ng-repeat="phone in phones | filter:query">

  </li>
</ul>
```

While it offers a snappy filtering experience and some declarative code, it feels too far stretched again. The convention around `$scope`, and a few other oddities as you'll see in this [EggHead.io $scope vs scope tutorial](http://www.egghead.io/video/NnB2NBtoeAY) can be quite a stress on the brain.

To conclude, I personally like both frameworks and think they offer some great features over their counterparts, but it'd make sense in a later version to pick conventions where it's truly conventional over an imposed one with lots of foreign concepts.