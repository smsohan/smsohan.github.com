---
layout: post
title: "AngularJS is Very Productive, and Cool too!"
date: 2013-06-14 13:10
comments: true
categories: [JavaScript, AngularJS]
---

It has a very steep learning curve, but yields a superb productivity boost once you've learned it. Check out [my demo of the wizard](http://smsohan.com/angular_wizard/) that we'll discuss next.

AngularJS works by extending HTML to produce declarative UI code and eliminating the need for a lot of boilerplate code. For example, the mental model of a wizard can be expressed using the following HTML:

```
<wizard title="Flight Search">

  <step title="Search">

  </step>


  <step title="Select a flight">

  </step>


  <step title="Select a return flight">

  </step>


  <step title="Checkout">

  </step>

  <step title="Confirm purchase">

  </step>

  <step title="Receipt">

  </step>

</wizard>

```

With AngularJS, one can write exactly this markup with the help of two custom directives, `widget` and `step`. This declarative UI code makes it very easy to read. In addition to this, the two way data binding capabilities of AngularJS makes it very productive as we don't need to write a bunch of references to the DOM nodes and render the nodes as the data changes. For a working example, check the source code of the demo and if you are like me, you'll love to see how simple it is.