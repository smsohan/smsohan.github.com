---
layout: post
title: "MicroOptimization Trap"
date: 2013-04-30 11:24
comments: true
categories: [Others]
---

Yesterday, couple of my friends and I were discussing about a CoffeeScript topic. In short, given the following code in CoffeeScript:

```coffeescript
class Cat
  type: -> 'cat'
  meow: -> 'meow meow mee..ow'
```

The compiler produces this in JavaScript:
```javascript
var Cat;

Cat = (function() {

  function Cat() {}

  Cat.prototype.type = function() {
    return 'cat';
  };

  Cat.prototype.meow = function() {
    return 'meow meow mee..ow';
  };

  return Cat;

})();
```

As you see here, the prototype method definitions use the prefix `Cat.prototype` repeatedly. Which could be compressed using closure as follows:
```javascript
var Cat;

Cat = (function() {

  function Cat() {}

  var p = Cat.prototype;

  p.type = function() {
    return 'cat';
  };

  p.meow = function() {
    return 'meow meow mee..ow';
  };

  return Cat;

})();
```

This closure form is for sure less bloated than the one produced by CoffeeScript. And on IE7, each `Cat.prototype` lookup takes about a microsecond, so, the closure form quite literally yields a micro-second-optimization! When one zooms into such micro-optimizations, other bigger fishes are easily missed.

For example, even though the output is a little bloated from the CoffeeScript compiler, the CS source code is rediculously simpler/shorter even for this tiny example. But that's not the only reason why one would use CS. If for nothing else, CS produces lint JavaScript for free. I'm not saying one has to use CoffeeScript, but this serves as an example scenario. When making a decision, a tunnel-vision into the micro-optimization may as well be a trap.