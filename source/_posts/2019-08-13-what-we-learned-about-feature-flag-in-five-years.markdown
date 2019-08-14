---
layout: post
title: "What We Learned About Feature Flags in Five Years"
date: 2019-08-13 21:56
comments: true
categories: [Software]
---
Looking at our git logs from [Cisco AMP for Endpoints](https://www.cisco.com/c/en/us/products/security/amp-for-endpoints/index.html) Console, I see that we introduced feature flags back in January, 2014. The reason I got interested in it is because even after all these years of use, today I had to build a new concept on our feature flag code. If you're already using feature flags or thinking about introducing feature flags to your project, this brief experience report may be helpful.

![switchboard](/images/switchboard.jpg)

<small>
Photo credits to [Michael Newton](https://flic.kr/p/9yJU3j)
</small>


Back in 2014, we were growing as a team, but wanted to keep working on a single shared codebase. We perceived  that the productivity gain of having multiple teams working on a shared code outweigh the issues that result from cross-team dependencies. As we started concurrently working on multiple features, mostly independent with different release dates, we ran into issues where unfinished work on one feature was blocking the release of a completed features. After some research, we decided to introduce Feature Flags in our code. First, we read [Martin Fowler's](https://martinfowler.com/bliki/FeatureToggle.html) article on this topic as a guideline. Over time, we have extended the use of feature flags with new concepts and I wanted to document it here for everyone.

1. **Database stored**: We store the feature flag in the main database so that the features can be toggled without needing a code deployment.
2. **Temporary vs. permanent**: We mark some feature flags as temporary when the primary goal is to incrementally release code to production. Temporary feature flags are cleaned once the feature is complete.
3. **Self-serve**: We tag some feature flags as self-serve where end users need to opt-in to use the feature.
4. **Limited availability**: For self-serve feature flags, we tag some features as limited availability. It allows us to release self-serve features to selected customers.
4. **Globally enabled**: We have a mechanism to globally enable or disable a feature flag.
5. **Enabled for all, but**: We have a mechanism for enabling a feature flag for all but some specific targets.
5. **Multi-target**: Sometimes we attach a single feature flag to multiple domain objects such as tenant, user, subscription tier, etc.
6. **Hierarchical**: We use a fallback mechanism for feature check. For example, the check for a feature for specific user falls back to the tenant it belongs to, which falls back to the feature itself being globally enabled.
7. **Code generator**: We use a single-command code generator to introduce a new feature flag to our code. It takes care of the database migration, seed entry, and code references.
8. **Circuit-breaker**: For integration with external services, we've used feature flags as a circuit-breaker to gracefully handle third-party downtime.

There are some reusable libraries and full-on products such as [LaunchDarkly](https://launchdarkly.com) that provide rich APIs and user interfaces for feature flags. At this point, even with all the aforementioned concepts, our custom implementation of feature flag is quite straight-forward and easy to evolve. It has been a key ingredient for our frequent deployments where we have 6 teams concurrently working on different features in the same shared code.
