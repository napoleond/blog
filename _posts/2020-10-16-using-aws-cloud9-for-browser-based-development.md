---
layout: post
title:  "Using AWS Cloud9 for browser-based development"
date:   2020-10-16 09:02:00 -0500
categories: []
tags: []
---

I've been working on a side project recently, related to [this other blog](https://tabbydata.com) that I started a few weeks back. (The project isn't ready to share quite yet, but you might be able to guess what I'm working on from those posts!)

Even though I've tried to become much more pragmatic in my technology choices over the years, side projects still end up being a great way to experiment with new, shiny things—or at least, new _to me_. (As a side note, this aspect of side projects—selecting the right stack and practice learning new technologies—is one of the two main reasons I believe in the controversial idea that engineers who program outside of work are generally better at it than those who do not. The other reason is that it demonstrates they actually enjoy using their superpower.) The latest new, shiny thing for me is AWS Cloud9.

## What problem does it solve?

AWS Cloud9 allows developers to build software entirely within their browser. In doing so, it actually solves a few problems:

 - The local environment doesn't matter anymore; Mac, PC, Chromebook, as long as it can run a modern browser you're all set.
 - The local environment is easily reproducible. If your laptop dies and you need to spin up a fresh development environment, or if you want to collaborate with others and need a fresh development environment for them, it's one click away.
 - Your Cloud9 instance can act as a [bastion host](https://en.wikipedia.org/wiki/Bastion_host) to the rest of your infrastructure, and it's as secure as your AWS console.

That was all pretty appealing to me, so I dug in.

## How do I use it?

As usual, Amazon's documentation is not the friendliest. The [setup guide](https://docs.aws.amazon.com/cloud9/latest/user-guide/setting-up.html) is fine enough, but after that I floundered a bit. The main problem is that, once you set up your Cloud9 environment, you're in an IDE and it feels like you should be doing things "the Cloud9 way" in order to avoid getting bitten later. It turns out that, at least in my case, ignoring that concern is sufficient. Basically, treat the terminal the same way you'd treat any SSH session into an EC2 box that you were planning to use as a dev environment (so use package managers from the terminal, run tests there, etc) and use the file manager/editor for their basic use-cases (or just use the terminal for everything if you want!). My project is built on Lambda, and Cloud9 has some [decent Lambda-specific features](https://docs.aws.amazon.com/cloud9/latest/user-guide/lambda-functions.html) but don't feel that you need to use Cloud9-specific features for build/deploy steps; that is not its core use-case.

Ultimately Cloud9 is just a small convenience layer on top of just spinning up your own EC2 instance to use as a dev box, but the convenience is well worth it IMO. You don't pay extra for running Cloud9 (you're only charged for the underlying EC2/EBS infra) and the fact tht Cloud9 automatically manages starting/stopping the instance for you makes it way more cost-effective than running the instance directly.
