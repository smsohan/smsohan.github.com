---
layout: post
title: "Development Environments and Dependency Hell"
date: 2012-10-25 21:51
comments: true
categories: ['Software']
---

It used to be pretty straight forward to get up and running with a Rails app. You'd expect something like the following:

``` bash
	git clone git@blah/blah.git
	cd blah
	bundle
	rake db:migrate
	rails s
```

If the project is using rvm and bundler, the ruby versions and gem depdendencies are all taken care of. So far life is good this way.

But it starts getting complicated. For example, your project probably uses MySQL and no matter what, I can't remeber all the c libraries that are pre-requisites for the mysql2 gem to actually install successfully. If it uses Nokogiri, get another hit of all the who-knows-what libxml2* libraries that need to be there. Another gem quite commonly used is rmagick with similar c depdencies. Every time I hit these road blocks, I feel so helpless for:

* I have no idea what is required
* Someone on StackOverflow has a magical solution
* The solution will work on one unix distro but won't on others

This starts getting even more complicated as you start adding external project depdendencies. For example, your project will probably need some Queueing, Caching, Emailing, SOA integration etc. And even if you have very good tests, it's likely that you'd want to manually test to see if your project holds up when integrated to other projects. It's best to have all these third-party products in your dev box for obvious reasons. But its also super hard to keep everything in sync, because

* You'd have to automate the installation of all the third-party products in your dev box
* Find a mechanism to update the third-party projects as they change

To tackle these problems, teams often use Chef/Puppet/Powershell/Vagrant etc. infrastructure automation tools. In practice, I'm yet to find a project where these tools would just work in a single pass. For example, it would miss one package or the other, sometimes it would fail at halfway, sometimes it would get you pretty close but almost never I have seen it to work on a single pass. I find this to be a recurring problem, faced by almost all dev teams.

##One solution I think may work is as follows:##

	1. Teams from each project writes their own bootstrap scripts, so others can just use it
	2. The bootstrap script runs on a CI server and fails a commit if it breaks the bootstrap
	3. Nobody runs a manual command ever for bootstrapping the dev machines

I'm yet to try this approach in practice. But too many times I've seen there's only one/couple people writing the bootstrapping scripts for all projects. Also, nobody really finds the issues until a new dev box needs bootstrapping. A CI server would be a big push to keep it green all the time. And finally, everytime a one-off command is run for bootstrapping, a little part of automation opportunity is missed.

If you've had success with these problems, please share. I'd really like to learn from you and hopefully implement some of your proven practices/principles.