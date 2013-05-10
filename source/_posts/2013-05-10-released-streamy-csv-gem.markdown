---
layout: post
title: "Released streamy_csv gem"
date: 2013-05-10 08:38
comments: true
categories: [Ruby, Rails]
---

Following the [previous post](/blog/2013/05/09/genereating-and-streaming-potentially-large-csv-files-using-ruby-on-rails/), I decided to spin off a little ruby gem for you folks. Get streamy_csv, and write only your application code while it'll do the boilerplate work for you.

In a nutshell, with this gem in your application, all you need to do is this:

```ruby
Class ExportsController

  def index

    stream_csv('data.csv', MyModel.header_row) do |rows|
      MyModel.find_each do |my_model|
        rows << my_model.to_csv_row
      end
    end

  end
end
```

Find more at [https://github.com/smsohan/streamy_csv](https://github.com/smsohan/streamy_csv)