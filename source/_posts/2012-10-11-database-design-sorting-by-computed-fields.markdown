---
layout: post
title: "Database Design: Sorting by Concepts on Nullable Fields"
date: 2012-10-11 21:05
comments: true
categories: Database, Architecture
---

In a recent project, we had this requirement to sort a list of items by a concept that is absent in the database schema, but can be derived from other fields. To make it easy to understand, let's build an example, simple enough to isolate the topic of interest.

Let's say we have a list of locations, stored in a database table as follows:


    Locations (location_id (primary key), province_id (NULL), city_id (NOT NULL))

And say, we want to present a list of locations sorted by "address". As you see in this example, "address" is a concept, not directly present in the database, but can be derived based on province_id and city_id. So, when we say sort by "address", the SQL query may look like the follwing:

``` sql query to sort by address
  SELECT * FROM Locations order by province_id, city_id
```

Now, if you take a second look at the schema, you'll see the <code>province_id</code> is <code>Nullable</code>. And if you are familiar with SQL, you already know that a <code>NULL</code> does not equal another <code>NULL</code>. So, for rows with <code>province_id = NULL</code>, the secondary sort on city_id has no effect at all. As a result, the expected sort by "address" cannot be derived by using this simple <code>order by</code> clause.

There are a few work arounds to it. One obvious workaround is to change the DB schema and make the <code>province_id</code> NOT NULL in favor of a placeholder, for example, "Not Available". This would eliminate the problem altogether. This should be pretty easy to achieve if you have control over the database schema.

But in case this is beyond your control, you'd have to hack your query and sleep with the noisy butterfly in your stomach. Such a poorman's solution would be to put a <code>"case ... when ... else... end..."</code> in the query.

The reason I wanted to share this post is, if your project needs to sort by such a computed concept, here's what you should probably:

1. Say "No" to it.
2. Change your database to incorporate the concept as a column in some table.
3. At the very least, make sure you're not sorting on a Nullable field.
4. Repeat 1-3 in order.

Thanks for reading. Keep crafting good software.








