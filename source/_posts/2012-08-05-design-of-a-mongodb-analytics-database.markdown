---
layout: post
title: "Design of a MongoDB Analytics Database"
date: 2012-08-05 17:38
comments: true
categories: Architecture
---

We built a MongoDB based realtime analytics solution and learned a few things in the process. I would like to share some of our learnings in this post.

When tasked to find a good database for holding realtime analytics data, I was searching through the wisdom of internet to come up with a good choice. At a high level, the database needed to deliver the following:

1. Atomic counters so that events could be counted fast. For example, count up the Total Sales metric for each sale event.
2. Unique sets. For example, unique set of buyers who bought anything today.
3. Simple lookups. For example, when a sale happens, it needs to lookup the original price to calculate the profit on the fly.
4. Indexing on multiple fields. For example, a product lookup may be by its ID as well as by a combination of other fields.
5. Fast at doing 1-4, so it can keep up with the event stream in realtime.

It became pretty clear that, the internet was favoring either Redis or MongoDB for such requirements. I did a little spike using both, and settled down on MongoDB. This was becauase, MongoDB had better support for performing lookups and indexing. Also, the document oriented nature of MongoDB suited well with the Object Oriented models, leaning towards a more direct match between the two worlds. Redis is awesome for doing increments and set operations, but it would require some application level handling of OO -> DB interfacing as well as intelligent lookups. So, MongoDB was the winner for the job.

If you've watched the video at [the mongodb blog](http://www.10gen.com/events/analytics-with-mongodb) you know it's quite straight forward to design a document for holding realtime analytics data.

> The key design goal is: precopute the metrics in the required aggregation levels.

This design goal is applicable to pretty much all reporting/dashboarding projects. Since it's common to aggregate different types of data on a dashboard, without precomputing the results, it would test anyone's patience before the report is ready. In our case, the dashboard mostly displays data for the day in realtime. So, a simplified text only view would display the following:

<pre>
###Sunday, August 5, 2012

* Total Sold: 2,500
* Total Profit: $ 1,345,427
* Proft Margin: 5.09%
* Buyers: 311
* Sellers: 126

####Online

* Total Sold: 400
* Total Profit: $ 98,762
* Proft Margin: 7.1%
* Buyers: 114
* Sellers: 28

####In Stores

* Total Sold: 2,100
* Total Profit: $ 124,666,5
* Proft Margin: 4.91%
* Buyers: 257
* Sellers: 110
</pre>

As discussed above, for a faster (and sane!) response time, the data better be precomputed in the database. So, to hold this precomputed data, we have designed a document like the following example:

<pre>
metric: {
  event_date: '2010/08/05',
  channel: 'online',
  total_sold: 2100,
  total_buying_price: 112,567,432,
  total_selling_price: 123,875,127,
  buyers: ['buyer 1', 'buyer 2'],
  sellers: ['seller 1', 'seller 2']
}
</pre>

And there is a document like this for each channel, for each day. This simple document is suitable for producing the desired report. Of course, in the real project, we need more data points in the report as well as aggregation levels beyond just a day and a channel. And you guessed it right, we have a document like this example one for each required aggregation level.

This design has held pretty well for our project. This is simple, there's a direct match between what is shown and what is stored. The data is already grouped, so queries simply fetch the documents without needing to perform any major computation on the fly. This model makes it trivial to add a new data point, as well as a new reporting aggregation level.

But the challenge is to make sure we can in fact keep a document like this up-to-date as events happen in realtime without falling behind. Let's talk about it in the upcoming post.

If you haven't had a chance to read the previous post in this series, here's a link to [Deploying to TV Screens](http://blog.smsohan.com/blog/2012/07/28/deploying-to-TV-screens/)