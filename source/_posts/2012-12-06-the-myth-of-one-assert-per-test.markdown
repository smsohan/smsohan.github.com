---
layout: post
title: "The Myth of One Assert Per Test"
date: 2012-12-06 09:55
comments: true
categories: [TDD]
---

> TL;DR; It's not one <code>assert</code> per test, rather one logical path per test.

I find this to be a classical example of how an inappropriate choice of terminology leads to huge confusion. In trying to find the original source of the "one assertion per test" quote, google came back only with a bunch of confused blog entries :(

Without much ranting, lets see a code example to start with:

```ruby example code
def eligible_credit_card_types(customer)

  annual_income = customer.annual_income

  if annual_income > 100_000
    [CreditCard.new(type: 'platinum', limit: 10_000),
     CreditCard.new(type: 'gold', limit: 8_000),
     CreditCard.new(type: 'cashback', limit: 5_000) ]

  elsif annual_income > 50_000
    [CreditCard.new(type: 'gold', limit: 6_000),
    CreditCard.new(type: 'cashback', limit: 3_000)]

  elsif annual_income > 30_000
    [CreditCard.new(type: 'cashback', limit: 1_500)]

  else
    []
  end

end
```

By saying one logical path per test, I mean I'd write a total of 4 tests for this method, each covering a logical path. But I really don't care about how many assert calls you need in each logical path to express the desired behavior. For example, this is totally fine:

```ruby example test
context '#eligible_credit_card_types' do
  it 'returns platinum, gold and cashback for people making over a 100K annually' do
    #setup customer, call the method
    cards = credit_card_authorizer.eligible_credit_card_types(customer)

    cards.size.should == 3
    cards[0].type.should == 'platinum'
    cards[0].limit.should == 10_000
    #same for cards[1], cards[2]
  end
end
```
Of course, this can indeed be converted into a single assertion, if you already had <code>equals</code> method overriden for the credit cards. But, I'd probably skip adding that code if its just for the sake of writing tests.

{% img right /images/bowling.jpg 300 550%}
<i>Image taken from mchenrybowl</i>

The mechnical thought of "one assertion per test" is lame.

1. If for nothing else, these silly assertions would multipy the running time of your test suite by a factor of digits.
2. Doesn't provide you any additional coverage or safety.
3. As long as each of your tests cover a unique logical path, there's only one logical reason why it'd fail, irrespective of how many assertions you put in there.

But, there's a but! If you need many asserts per test, the code is probably asking for some refactoring. It indicates that one logical path in your code is touching too many things. Worse, when it calls too many methods that belong to other objects. Testing such a logical path is hard, usually requires a big setup and a bigger assertion. In such cases, breaking down the test into multiple tests may yield some superficial readability of the test, but certainly works around the actual problem in the code without fixing it. Almost always, this indicates long procedural methods and I'd suggest taking a second look at it to refactor into a more OO code.

Happy Friday!


