---
layout: post
title: "How much to validate?"
date: 2012-12-14 15:22
comments: true
categories: [Architecture]
---

Input validation is often required to safe guard against inappropriate use. For example, consider the following API:

```ruby TransfersController.rb
class TransfersController

  def create()
    from_account_id, to_account_id, amount = params[:from_account_id], params[:to_account_id], params[:amount]

    from_account = Account.find(from_account_id)
    to_account = Account.find(to_account_id)

    @transfer = Transfer.create!(from_account, to_account, amount)
    redirect_to @transfer
  end

end
```

In this case, the API is expecting a <code>from_account_id</code> which we know, can be easily exploited unless a validation is performed on the server. Say, a simple validation would do this:

```ruby TransfersController.rb
class TransfersController

  def create()
    #...
    if from_account.owner == current_user
      @transfer = Transfer.create!(from_account, to_account, amount)
      #...
    else
      render :unautorized
    end

  end

end
```

This is an absolutely necessary validation to be performed on the server.

However, the validation requirements can be a little relaxed at times. For example, if you know the only client of your API is also your own app, then you may not need to handle all possible validations both in the server and cliend side and come up with fancy errors. When its non-destructive, an invalid request can just be shown the general purpose error page, since this is only for bad users trying to hack the API calls.

For example, lets say we have the following code:

```ruby AppointmentsController.rb

class AppointmentsController
  def show
    user_id, date = params[:user_id], params[:date]

    user = User.find(user_id)

    @appointments = user.appointments.on(date)
  end
end

```
Say, by business logic, everyone can see the apointments for any user. Now, the above code will surely fail if any of <code>user_id/date</code> is not provided, or has a bad data. So, its possible to add the validation logic. But if I know I'm writing the only client that's supposed to hit this API using a client side validated request, I'd just skip the input validation. This is not destructive anyway.

I think it's a trade-off between paranoid programming and pragmatic design. As long as I'm providing expected behavior in the app without opening up a security hole, some validations may be skipped on purpose.
