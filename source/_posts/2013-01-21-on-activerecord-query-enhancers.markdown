---
layout: post
title: "On ActiveRecord Query Enhancers"
date: 2013-01-21 16:43
comments: true
categories: [Ruby, Rails]
---

The question is, should we use the third-party ActiveRecord Query Enhancers like SearchLogic/Squeel/MetaSearch?

Quoting from [Squeel's Github README](https://github.com/ernie/squeel) page,

```ruby
Squeel lets you rewrite...

Article.where ['created_at >= ?', 2.weeks.ago]
...as...

Article.where{created_at >= 2.weeks.ago}
This is a good thing. If you don't agree, Squeel might not be for you.
```

At work, we are migrating a Rails 3.0 project into Rails 3.2. We used MetaSearch in the project quite extensively and now discussing if using Squeel (successor of MetaSearch for newer Rails versions) would be a good decision. I recognized there are good points on both sides of the debate and wanted to capture the list here for a reference.

###You'd want to use such enhancers because:

1. They extend the basic AR API to provide a DSL. For example, Squeel provides you an API so you can write the following:
<code>User.where{ country != "USA" && drives_truck == true }</code>
Instead of this Activ eRecord Query:
<code>User.where('country <> ? && drives_truck = ? ', 'USA', true)</code>
2. They write complex join statements, including outer joins and joining multiple tables, for you using a shorthand. e.g. <code>User.where{company_name_eq 'Coders'}</code>
3. They support negative logic (not equal, not in) and OR SQL queries that would require raw String queries using ActiveRecord API.
4. They provide fancy operations such as <code>User.where{name_or_address_contains 'scott'}</code> that would require some raw String when using AR directly.

###You'd avoid using these enhancers because:

1. You think that using String is just fine over using a Hash with hardcoded symbols anyway.
2. As new versions of AR are released, there's little guarantee the third-party API will still be compatible.
3. You are concerned about adding another pile of abstractions and magic on top of ActiveRecord.

Please share if you prefer one over another and if you do, please let us know why.