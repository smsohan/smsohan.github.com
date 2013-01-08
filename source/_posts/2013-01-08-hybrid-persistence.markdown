---
layout: post
title: "Hybrid Persistence"
date: 2013-01-08 15:08
comments: true
categories: [Database, Architecture]
---

Often times the business requirements demand for database features that aren't easily achievable using a single type of database. And now we have quite a few options to choose from, for example Key Value stores, Document databases, Relational databases etc, each providing some mutually exclusive features from the rest. So, it can be tempting to introduce multiple databases to rip the benefits of each.

But, it comes with a few gotchas that are worth knowing. Here's a short list from my recent experience on a project:

1. No simple way to join data from multiple databases without multiple round trips.
2. Deployment is tricky as it adds more infrastructure.
3. If data is duplicated, migrations need to address multiple sources.

Let's explain this with an example. Say, we need to store data for an e-commerce site. Document stores seem to be good candidate for storing the product info, since different products have different data associated with it.

```json products.json

  {
    id: '4wqrqw4890ipoip',
    name: 'iPhone 5',
    color: 'Black',
    price: '699.99'
  }


  {
    id: '4wqrqw4890ipoiq',
    name: 'Fine Sheet',
    thread_count: 400,
    size: 'Queen',
    price: '699.99'
  }

```

We also need to store transactional data and Relational databases have been successfully used for transactions. So, a simple schema may look like this:

```ruby
  Sales(id, store_id, product_id, quantity)
```

With a hybrid persistence approach like this, just beware that implementing simple features as follows will need more roundtrips:

1. Show the invoice for sale with product <b>name</b>, quantity and price.
2. Show all 'Fine Sheets' that are sold in the 'Brentwood' store.
3. List all products sold today, sorted by <b>name</b>.

Hybrid persistence works fine for caching. But whenever your data is split into multiple sources in way that you'd have to combine the parts from each, it's not gonna be fun time. Just so you know.