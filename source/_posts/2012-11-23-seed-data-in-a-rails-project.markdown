---
layout: post
title: "Seed Data in a Rails Project"
date: 2012-11-23 13:18
comments: true
categories: [Ruby on Rails]
---

Most applications I've worked on required some kind of seed data. For example, I've used seed data to create a super admin user so the site can actually be used upon a fresh install. Some other common cases include things like, list of credit card types, names of countries/states etc. This post is about some ideas to manage thsese seed data in a Ruby on Rails project.

### Where to put the seed data?

I think you have 2 choices to pick from.

1. db/seeds.rb
2. db/migrate/2342423424_some_migration.rb

I prefer having it in the seeds.rb whenever possible since this is the obvious place for it. But I've often put them in migrations as well. There are trade-offs associated with both.

Migrations only run once. This means, "double/miltiple seeding" is not a concern at all. So, you can write a migration with some seeding command as follows:


```ruby Migration CreateUsers

  def self.up

    create_table :users do |u|
      u.name
      u.email
      u.encrypted_password
      u.password_salt
    end

    create_super_admin_user
  end

  def self.create_super_admin_user

    User.create!(name: 'SuperAdmin', email: 'super@mysite.com', ...)

  end
```

This works great. It creates the super user and by virtue of migrations, it only ever creates one super admin.

The caveat with this approach is, if you change certain things later, then this migration will fail to run. For example, say we added a required field to user as follows:

```ruby User

  class User
    validates :age, presence: true
  end

```

Now, if you run the migration on a freshly checked out code, it will fail due to this validation. One workaround is to fix the code in this migration. But for people that already ran the migrations, and likely your production environment already did so, this has no effect. At this point, it starts getting complicated and some band-aid solutions are usually implemented.

On the other hand, if you are using db/seeds.rb for seed data, you'd have to make sure its idempotent. So, a rerun should not create a 2nd copy of the objects. As a start, your code could be like this:


```ruby db/seeds.rb
  if User.where(email: 'super@mysite.com').blank?
    User.create!(name: 'SuperAdmin', email: 'super@mysite.com', ...)
  end
```

With this in place, you can easily incorporate the change for the new validation as follows:

```ruby db/seeds.rb
  super_user = User.where(email: 'super@mysite.com').first
  if super_user && super_user.age.blank?
    super_user.update_attributes(age: 20)
  else
    User.create!(age: 20, name: 'SuperAdmin', email: 'super@mysite.com', ...)
  end
```

This would work for people who already had a super user and also for people that just got a fresh copy of the code.

> So, I prefer putting data in seeds.rb with idempotency.

In addition to handling such cases, it also makes it easy for anyone to take glance at the seed data in one place.

### How to handle large amount of seed data?

When you have more than a handful of data to seed, I'd recommend tidying up your db/seeds.rb into a more manageable structure. Here's how I'd do it:

```bash File organization
  |- app
  |- db
    - seeds.rb
    |- seeds
      - users.rb
      - credit_card_types.rb
```

With this structure in place, you can turn you seeds.rb into a simple manifesto file as follows:

```ruby seeds.rb

  require 'seeds/users'
  require 'seeds/credit_card_types'

```

This will help you keeping things organized and hopefully give you a happier solution to the seed data problem.




