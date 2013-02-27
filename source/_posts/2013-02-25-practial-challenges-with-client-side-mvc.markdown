---
layout: post
title: "Practical API Design Challenges for Client Side MVC"
date: 2013-02-25 22:40
comments: true
categories: [Architecture]
---

This following list is probably a subset of a bigger one, but I've composed it based on my experience using BackboneJS + Rails on the current project at work. So, here goes the list:

###Designing Granularity of the API###

Using RESTful API, it makes sense to have separate API methods to repesent each type of resource. However, to construct a meaninful UI for the end user, often time we need to represent multiple resources on the same screen. Let's explain this with an example requirement:

As a user I want to see the **emails** using a saved **custom filter**

In this case, we would want the UI to show the description of the selected filter, and also the emails matching the filter. Conceptually, <code>CustomFilter</code> and <code>Email</code> are two resources or models, having distinct properties and behaviors. It'd make sense to have two separate API's as follows:

**Filter API:**
GET http://fancy_domain.com/filters/emails_from_boss_about_release

```json
{
  id: 10,
  filter_name: emails_from_boss_about_release,
  subject_including: 'release',
  from: 'boss@the_company.com'
}
```

**Email API:**
GET http://fancy_domain.com/filters/10/emails

```json
[
  {
    subject: 'Release note',
    from: "boss@the_company.com",
    to: "sohan@the_company.com",
    body: "<html>.. </html>",
    sent_at: "20130102040511"
  }
]
```

Although this granularity makes good sense as an API, it starts causing issues for typical client side MVC use case.

>Unless the data from these two API methods are combined on the server side, the client must make two requests.

In a real world use, I've seen it's more common for the UI to require multiple resources than not. Since, we want our users to get faster response, we try to create API's that suit the UI's need. So, APIs start getting bloated payloads to match the UI's need. This typically results into something like the following:

**Filter Email API:**
GET http://fancy_domain.com/filters/emails_from_boss_about_release?include_emails=true

```json
{
  id: 10,
  filter_name: 'emails_from_boss_about_release',
  subject_including: 'release',
  from: 'boss@the_company.com',
  emails:
  [
    {
      subject: 'Release note',
      from: 'boss@the_company.com',
      to: 'sohan@the_company.com',
      body: '<html>.. </html>',
      sent_at: 20130102040511
    }
  ]
}
```

This is an example with just two resources. I've seen UI's that need more than two resources, and its quite common as well.

###Short/Medium/Long form of Models/Resources###

The view usually renders a subset of all possible attributes associated with a model.

For example, when rendering the <code>WeatherForecast</code> model, in the list view only the minimum and maximum temperature for the day with a short summary of Sunny/Cloudy/Rainy/Snowy are shown. However, when you want to see more, you'd want to show the hourly variation of the weather with additional data of interest.

This requires the RESTful API to prepare multiple respresentations of a resource to suit the specific UI needs. This can complicate the logic on both the server and client sides, since the code must accommodate different representations for the same model.


