---
layout: post
title: "Configure Me Not"
date: 2013-11-03 21:29
comments: true
categories: [Software, Design]
---

Configuration in software provides a method to build systems that can
adapt to different configurations. For example, if a website's language and
date/currency formats are configurable, then it can be configured to
support multiple languages and regional formats. Configuration makes it
possible to deliver such features without needing a log of change in the
application source code.

However, this notion of flexibility that configuration provides can be a trap at times. I've a definition of configurable as follows:
> A configurable must have at least two configurations.

This is another way of saying YAGNI. But I find this to be more specific
than YAGNI, as it quantifies and makes it apparent.

Here are a few examples to illustrate my definition.

1. 
    __Custom interfaces with a single implementation.__

    Interfaces are often times thought as a configurable component, as a new
    implementation can be used in place of an old one without changing the
    code that uses it.

    Except, if your interface only ever have one implementation, this
    provides a false notion of flexibility. In practice, I've seen for most
    custom interfaces, a new implementation almost always needs a change in
    the original interface which doesn't really make it configurable
    anymore. 

2. __Default arguments in methods that are never passed a non-default
   value.__ 

    Default arguments are great, as they often times simplify the common
    case. However, if a method with a default argument is never called
    with a non-default value, it's simply not worth using a default argument. Use a local variable instead.

3. __Configuration key value pairs where there's only one value.__

    Since magic numbers and hardcoded strings are bad, it's tempting to use
    the configuration file to hold such values. However, if there's only one
    such value, it's probably a constant and not a configurable object.

4. __Exhaustively validating method parameters against all possible but unused values.__

    If you're writing a method that's only gonna be called from another
    method in your project, you probably know what you're passing to the
    method. Validating for different negative inputs to such methods provide
    a sense of robustness without really adding any value to it.

Hoping, the definition makes sense. Would love to hear your
opinion and examples of configure me not.

