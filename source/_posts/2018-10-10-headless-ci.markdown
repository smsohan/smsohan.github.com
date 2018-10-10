---
layout: post
title: "Headless CI"
date: 2018-10-10 08:02
comments: true
categories:
---

Past few months at work we're trying to revamp our CI infra. This has
been a migration from a [Jenkins](https://jenkins.io) cluster to [GoCD](https://www.gocd.org).
This post is a result of my frustrations with the state of CI in the age of Git/GitHub.

With Pull Requests, or a form of pre-merge review process, it's common
to leverage a few automated checks alongside our human peers to remove
potentially breaking changes from being merged into the repo. These
changes typically require a CI server and often multiple CI
jobs/pipelines to check test run / coverage results, linting, or any other policy
that can be automatically enforced.

This is where you need to integrate your GitHub/what have you with your
GoCD or the likes. And it's a time consuming task for any complicated
project. You essentially, setup pipelines that are somewhat duplicate
to be executed pre and post merge. The more I think about, the more it
feels like the whole CI process should be a headless one, and deeply
embedded within the code repo. This would save unneccessary hassles such
as setting up user accounts / notifications across multiple systems,
navigating between web interfaces to see important logs when things
fail etc.

I like the direction [GitLab](https://about.gitlab.com/features/gitlab-ci-cd/) is taking.
They have made CI part of GitLab UI so you can see the code and its
build status in the same place. I'd like other CI environments to do the
same, or at least be a little app/plugin that you can add on top of GitHub - to
blend within the GitHub experience such that it doesn't feel like a
foreign object.


