---
layout: post
title: "Deploying to TV Screens"
date: 2012-07-28 15:28
comments: true
categories: [Javascript, Ruby, Rails]
---

Off late, I am working on a project to render real time business data with interesting visualizations, so people can feel the pulse of the business. For the last couple of months, I have been planning to write a detailed post about it. But after a few false starts, I am finally settling on smaller posts, telling a small part of the story each time.

>So, have you ever worked on a web application that is primarily viewed through 55"+ 1080p TV screens?

We are showing real time business data, aggregated from multiple data sources as they are happening. The screens are gonna be mounted on the wall, like as you'd see in the airports. And it needs to be running 24x7

This introduces an interesting deployment challenge:

>How would you reload the screen every time you re-deploy the app?

A regular web app is interactive. So, when we re-deploy the app, the users typically get the latest version as soon as they reload the page or navigate through the app. However, in an airport like setting, where information is displayed across many screens, and typically no-one is clicking it, the app needs to be aware of updates and reload itself to achieve the same. This is essential, for example, if there's an API change on the server side, the HTML/Javascript/CSS must be in sync to be able render it.

![Airport displays](http://farm1.staticflickr.com/65/212623519_eae543d64c.jpg)

Photo credits to  5mal5

The app itself uses JSON API calls to render the live data. Each screen is somewhat like a single page app, using multiple AJAX calls to render different parts of the screen, showing different data. The API calls are all funneled through a single Javascript module. The module looks like the following (showing a simplified version for brevity):

{% gist 3194417 %}

If you have noticed here, there's an extra check inside the success callback. To begin with, the page remembers the server token on reload. So, whenever there's a new token, it refreshes the page. Since all API calls are funneled through this module, this becomes a no-brainer to support new screens/API calls.

Our API's respond with a server token, which is guaranteed to:

1. Remain same for each server deployment, and
2. Change whenever there's a new deployment.

However, we still need to make sure the server token indeed ensures these two essential properties. With a little trick, this becomes trivial. For our app, we are using Capistrano to deploy our Ruby on Rails project. For those new to Capistrano, it uses a timestamped directory for each deploy, symlinking it to the current. So, it looks somewhat like the following: 

{% gist 3194484 %}

Every Ruby on Rails app also comes with a little method, Rails.root that returns the full path to the directory of the current deployment. So, in this example scenario, we get the following:

Rails.root #==&gt; /app/realtime/releases/20120729083021

Since every deployment will be a new timestamp, this method ensures a unique token for each deployment. That's all we need for the api module to be aware of new deployments and auto refreshes. Here's an example controller/action (again, simplified for brevity): 

{% gist 3194430 %}

I liked the organic nature of this technique. It is harvesting on the available tools. Although the examples in this post show Ruby/Rails as an example, I am sure the same techniques can be applied to other technologies with the same simplicity.

Before I conclude, I would share one limitation of this technique here. Since the page reload happens on a shared api module, the reload needs to be generic, without requiring any special knowledge about the pages to reload. This pretty much means, a page needs to be able to reconstruct itself entirely from it's URL. Requiring any Javascript state beyond the URL, would probably require API specific handling to reload, killing the advantage of this technique. But the good news is, its always a good practice to rely solely on the URL to construct a page.

Thanks a ton if you've followed all the way. Stay tuned for the upcoming posts, where I will be telling the story of handling multiple API calls on a page, highlighting data changes and some other interesting bits about a real time dashboard.