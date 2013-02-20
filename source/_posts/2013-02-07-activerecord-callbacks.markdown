---
layout: post
title: "Are ActiveRecord Callbacks Any Good?"
date: 2013-02-07 09:04
comments: true
categories: [Ruby on Rails]
---

Wanted to share my thoughts on ActiveRecord Callbacks. I'd like to know your thoughts if you disagree. Please use the comments.

* Only use them when the behavior is <b>must have</b> under all situations, including your tests. For example, we know email addresses are case-insensitive. So no matter what, we may always want to store a lower cased version in the db.

```ruby user.rb
#Good
class User < ActiveRecord::Base

  before_save :downcase_email

  def downcase_email
    email.downcase!
  end

end

```

* Never use them otherwise. A couple of classic examples that I consider as bad:

```ruby user.rb
#Bad: Sends email, probably not required under all situations such as when creating via migrations, tests etc.
class User < ActiveRecord::Base

  after_create :welcome

  def welcome
    WelcomeMailer.welcome(self).deliver
  end

end

#Bad: Interacts with external components
class User < ActiveRecord::Base

  after_create :setup_orientation

  def setup_orientation
    OrientationMessageQueue.enque(self)
  end

end

```

* A common problem with the callbacks is, there are some handy methods that skip the callbacks. For example, consider the following:

```ruby User.rb
#Careful

class User < ActiveRecord::Base

  before_save :update_coordinates

  def update_coordinates
    self.coordinates = Geo.find_coordinates(zip_code)
  end

end

#works fine
sohan = User.find_by_name('Sohan')
sohan.zip_code = '33333'
sohan.save!

#doesn't fire the callback
User.update_all({zip_code: '33333'}, {zip_code: '55555'})
```

If you're using the callbacks, I'd emphasize again, only use when you are absolutely sure the desired behavior applies under all contexts.
