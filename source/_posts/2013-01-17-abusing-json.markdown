---
layout: post
title: "MongoDB is Abusing JSON!"
date: 2013-01-17 16:11
comments: true
categories: [Architecture, MongoDB]
---

I find the MongoDB API is abusing JSON in a really bad way. JSON is probably a good format for storing the documents in MongoDB, but using JSON for it's weird API is simply a terrible idea. Here's an example from the [SQL to Aggregation Framework Mapping Chart](http://docs.mongodb.org/manual/reference/sql-aggregation-comparison/)

```js MongoDB Example
  db.orders.aggregate( [
     { $group: { _id: { cust_id: "$cust_id",
                        ord_date: "$ord_date" },
                 total: { $sum: "$price" } } },
     { $match: { total: { $gt: 250 } } }
  ] )
```

I find the UI of this query to be distasteful at the best. Here's an SQL example of the same:

```sql SQL Equivalent
  SELECT cust_id,
         ord_date,
         SUM(price) AS total
  FROM orders
  GROUP BY cust_id, ord_date
  HAVING total > 250
```

I find this SQL example to be a few magnitudes more readable than the JSON one. The JSON query in this example, is full of hacks. Every time I see a "$" sign, I'm totally confused about what it means. For example, consider this fragment, total: { $sum: "$price" } and compare it to SUM(price) AS total.

I think MongoDB has its strengths. But for haven's sake, if they can't find any better, they should leave this JSON ugliness in favor of SQL as a query interface to MongoDB. What do you think?
