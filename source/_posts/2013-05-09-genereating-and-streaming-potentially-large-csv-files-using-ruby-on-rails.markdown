---
layout: post
title: "Generating and Streaming Potentially Large CSV files using Ruby on Rails"
date: 2013-05-09 14:38
comments: true
categories: [Ruby, Rails]
---

Most applications I've worked on at some point required that 'Export' feature so people would be able to play with the data using the familiar Excel interface. I'm sharing some code here from a recent work that did the following:

Generate a CSV file for download with up to 100,000 rows in it. Since the contents of the file depends on some dynamic parameters, and the underlying data is changing all the time, the file must be generated live. Generating a large file takes time and the load balancer will drop the connection if it takes more than 1 minute. In fact, as a consumer I myself would be frustrated had it took even 1 minute to see something happening. This problem natually requires a streaming solution.

For a familiar example, let's say we are downloading a CSV file containing transactions on an online store for the accounting folks. Lets say the URL is as follows:

http://transactions.com/transactions.csv?start=2013-01-01&end=2013-04-30&type=CreditCard&min_amount=400

So, this would download a file containing the transactions from January to April of 2013, where a CreditCard was used for a purchase over $400. Here goes the code example with inline comments describing interesting parts.

```ruby app/models/transaction.rb

class Transaction
  belongs_to :store
  attr_accessible :time, :amount

  def self.csv_header
    #Using ruby's built-in CSV::Row class
    #true - means its a header
    CSV::Row.new([:time, :store, :amount], ['Time', 'Store', 'Amount'], true)
  end

  def to_csv_row
    CSV::Row.new(title: title, store: store.name, amount: amount)
  end

  def self.find_in_batches(filters, batch_size, &block)
    #find_each will batch the results instead of getting all in one go
    where(filters).find_each(batch_size: batch_size) do |transaction|
      yield transaction
    end
  end

end

```

Given this Transaction model, the controller can call the methods and set appropriate http headers to stream the rows as they are generated instead of waiting for the whole file to be generated. Here's the example controller code:

```ruby app/controllers/transactions_controller.rb

class TransactionsController

  def index

    respond_to do |format|

      format.csv render_csv

    end

  end

  private

  def render_csv
    set_file_headers
    set_streaming_headers

    response.status = 200

    #setting the body to an enumerator, rails will iterate this enumerator
    self.response_body = csv_lines(filters)
  end


  def set_file_headers
    file_name = "transactions.csv"
    headers["Content-Type"] = "text/csv"
    headers["Content-disposition"] = "attachment; filename=\"#{file_name}\""
  end


  def set_streaming_headers
    #nginx doc: Setting this to "no" will allow unbuffered responses suitable for Comet and HTTP streaming applications
    headers['X-Accel-Buffering'] = 'no'

    headers["Cache-Control"] ||= "no-cache"
    headers.delete("Content-Length")
  end

  def csv_lines

    Enumerator.new do |y|
      y << Transaction.csv_header.to_s

      #ideally you'd validate the params, skipping here for brevity
      Transaction.find_in_batches(params){ |transaction| y << transaction.to_csv_row.to_s }
    end

  end

end
```

As you see in this example, it's pretty straight forward once you put the pieces together. These streaming headers work under most servers including Passenger, Unicorn, etc. but webrick doesn't support streaming responses. It took me some time to figure out the headers and the enumerator thing, but since then it's working beautifully for us. Hope it will help someone with a similar need.

