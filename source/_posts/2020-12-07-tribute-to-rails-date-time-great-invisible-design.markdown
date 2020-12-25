---
layout: post
title: "A Tribute to Time Travel APIs in Ruby on Rails"
date: 2020-12-07 10:28
comments: true
categories: ["Architecture"]
---

> Good design is obvious. Great design is transparent. — Joe Sparano

Ruby on Rails delighted me all through my career. The community is one with a taste for art, thanks to DHH's ability to write well. He set a  high bar and the community also lives up to it. One API that absolutely blows my mind is how delightful it is to work with date and time in Ruby on Rails. Here are a few example use cases:

```ruby
# Schedule a report to run at the beginning of next week
report.run_at = 1.week.from_now.beginning_of_week

#.. or first Friday of next month
report.run_at = report.run_at.next_occurring(:friday) unless report.run_at.friday?

#.. or beginning of next month
report.run_at = 1.month.from_now.beginning_of_month

#.. or mid-day tomorrow
report.run_at = 1.day.from_now.at_midday

# Is today a weekday?
Time.now.on_weekday?

#.. or a weekend?
Time.now.on_weekend?

# Book a calendar event for a whole day
time_off.duration = Time.now.all_day

#.. or find sales in the current quarter
Sale.where(created_at: Time.now.all_quarter)...
```

A lot of time related code I've seen in real projects is finding a point in time relative to `now`. Ruby on Rails makes it an absolute joy traveling time by adding time related methods straight into the [Integer](https://api.rubyonrails.org) class, where the code looks just like how we think about time. Sure, Rails uses complex classes such as `TimeWithZone`, `Duration`, etc. under the hood, but I've almost never seen those used directly. This is such a stark contrast with many other platforms where you have to use pedantic concepts such as `TimeSpan`, `Calendar`, `GregorianCalendar`, etc. The true elegance of time travel API in Ruby on Rails lies in how invisible it is.