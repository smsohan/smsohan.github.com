---
layout: post
title: "AngularJS: Refresh or Die"
date: 2014-04-03 08:27
comments: true
categories: [JavaScript, AngularJS]
---

Just worked on my first AngularJS project at [http://loveyyc.info](http://loveyyc.info) ([source code](https://github.com/smsohan/analytics)) and wanted to share my thoughts about it while the memory is still fresh. My impression in a one-liner:

> AngularJS needs to rename a lot of things and introduce higher level abstraction

A great technology only becomes so because people feel happy about it. The happiness for people like me comes from the fact that not only I understand the technology well, but also find it to be easy to use. I think AngularJS is an amazing framework for building JavaScript apps. But oh boy, the barrier to entry is brutal to say the least. Their terminology makes it frustrating and very hard to compose a mental model of the framework.

But instead of just complaining and ranting, I wanted to suggest a few concrete refactorings. I think these refactorings would make my way easier next time I'm about to introduce someone new to AngularJS:

```js
//rename rootScope to pageState
//BAD
var matchController = function($rootScope)...

//GOOD
var matchController = function($pageState)...

//rename controller$scope to controllerState
//BAD
var matchController = function($scope)

//GOOD
var matchController = function($controllerState)

//introduce customTag, customAttribute as an abstraction over lower level directive

//example template <searchResult highligted></searchResult>

//BAD
app.directive('searchResult', function(){
  return {restrict: 'E'}
});

//GOOD
app.customTag('searchResult')
app.customAttribute('highligted')

//Use intention revealing name instead of symbols, uglifiers have a different job than humans

//directive#require

//BAD
require: '^searchResult'

//GOOD
nestedInController: 'searchResult'

//BAD
scope: {
  count: "&",
  count: "=",
  count: "@",
}

//GOOD
customTagState: {
  count: expression("count")
  count: twoWayBound("count")
  count: stringValue("count")
}

//BAD transclude -> really?
tranclude: true

//GOOD
//always transclude, if the user has put content inside a custom tag. why not?

//NOT SO GOOD
worksAsLayout: true

//linking is a low level concept. abstract using a higher level name

//BAD
directive.link

//GOOD
directive.init

//why need a controller in directive

//BAD
directive.controller

//GOOD
no controller required


//BAD -> $compile what?
$compile

//GOOD
$compileTemplate

//BAD -> $apply what?
$apply

//GOOD
$compileTemplateAfter

//BAD filter -> means take a list and produce a sublist. It has little to do with most AngularJS filters
$filter.filter('currency')
$filter.filter('number')

//GOOD
helper

//BAD factory -> WTH
app.factory('myData')

//GOOD
app.model('myData')
```

I just wanted to share my 2 cents about it. I love this framework and I think it's really good and productive once you understand the core concepts. But I strongly suggest the AngularJS team to take some time to simplify things. A refresh will save it and make it hugely successful or it'll die like a champion fighter, who had all the right weapons but the wrong uniform.

Thoughts?