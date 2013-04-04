---
layout: post
title: "Implementation Challenges with a Multi-Tenant/SaaS Database"
date: 2013-04-04 10:29
comments: true
categories: [Architecture, Database]
---

{% img right http://farm1.staticflickr.com/21/26445881_7c69777b7b.jpg 500 353 'Photo credits to corydalus' 'climbing'%}

[This 2006 MSDN article](http://msdn.microsoft.com/en-us/library/aa479086.aspx) points out some key aspects of designing a multi-tenant database for SaaS applications. As you can read in the article, SaaS databases need to pick one of the following three configurations:

1. separate databases
2. shared database, separate schema and
3. shared database, shared schema.

A number of factors including economic, security, skillset, etc. contribute to the selection of the best suitable configuration. In this post, from my experience, I'm sharing the following practical requirements that introduce additional implementation challenges:

1. Each account needs to have a __maximum allowed space__ on the database (economic).
2. Data from one account should never be accessible to other accounts (security).
3. However, for backend usage, we need the ability to run queries across all accounts.

__Size limiting is quite hard__. It almost forces the use of separate database/schema per account. Even then,  most databases of today don't have a clean mechanism to exert such a hard limit.

__Separate databases reduce the chance of cross-account data leaks__. But backend tasks suffer for this. For example, your monthly billing processor needs to generate bill for all accounts. With one database/account, it cannot do one simple query to a single database anymore.

Also, __most ORM libraries don't support separate databases__ for a single type. For example, to fetch the orders from the database, the ORM library needs to connect to database A for account X, but to database B for account Y and so on. At this point, if possible, you'll need to tweak the ORMs a lot or fall back to your own ORM, which as [I wrote in the past](/blog/2012/11/06/orm-or-not/), is almost never a good idea.

__Connection pooling is another challenge__. It's generally a good practice to use connection pooling, to save the overhead of establishing a connection before every query. With separate databases, and hundreds, if not thousands, of accounts being served from an app server, the connection pool would either have too many or too few connections in it to be useful.

I don't know about a clean architecture that'd address these requirements while not introducing the dev challenges. Please comment if you've any suggestion.

