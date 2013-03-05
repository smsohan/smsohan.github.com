---
layout: post
title: "Random Data in Tests"
date: 2013-02-27 15:24
comments: true
categories: [TDD]
---

... are evil if you expect the randomizer to give you unique values on each call.

Here's an example:


```ruby
before(:each) do
  @user_1 = User.create!(email: "email-#{Random.rand(1000)}@example.com")

  # the following will fail intermittently if you have a unique validation on User#email

  @user_2 = User.create!(email: "email-#{Random.rand(1000)}@example.com")
end
```

It's a timebomb, use it, it will haunt you later. Be sane, stop using random numbers in tests like this.