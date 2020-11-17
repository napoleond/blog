---
layout: post
title: Embedded analytics
date: 2020-11-17 10:24:00 -0500
categories: []
tags: []
---

_\[Note: This is a cross-post from over at [tabbydata.com](https://tabbydata.com).\]_

A really common use-case for offline/analytical data systems at startups is embedded analytics—i.e. the data that you expose to your customers for analysis (via in-app dashboards and the like) as opposed to the data you use internally. At scale, this functionality has traditionally been served through dedicated, isolated systems, although technologies like YouTube's [Procella](http://www.vldb.org/pvldb/vol12/p2022-chattopadhyay.pdf) are marking a shift toward "one data repository to rule them all."

For really early-stage startups, though, the options are a bit different. Most commonly, the first version of embedded analytics at a startup is being served by the same application database as everything else. Depending on the datastore, it might even be relying on the exact same records.

Another popular option is to use a managed platform. [Keen.io](https://keen.io) is a well-known example; I've personally found that one to be functionally- and performance-limited, with pricing that doesn't make lots of sense.

Regardless of the specifics, what generally tends to happen with embedded analytics functionality is that flexibility is quickly lost:

 - If the first version relies on the application database, and that database is relational, then the first version is extremely flexible: as long as the data is known, and the query can be expressed in SQL, it can be made readily available. However, growing startups that did not have a good sense for the difference between OLTP/OLAP will quickly learn it the hard way if they pursue this path—the analytics query performance will suffer over time, _and_ it will put pressure on the rest of the application, since the application database acts as a single point of failure.
 - Clever founders who avoid that pitfall, as well as those who learn from it, might build analytics based on a NoSQL store of some sort. (This includes flatfile approaches; most of the managed platforms, like Keen, also fall into this category.) This is not guaranteed to address performance problems, but if they take a read-optimized approach then it usually will. Unfortunately, this comes at the expense of flexibility: the analytics that can be exposed to users are limited by the write format, and changing that generally involves re-running batch jobs of some sort. It does not lend itself well to customer-driven feature discovery, which is what startups need to be doing.

By now, if you've read the other posts on this site, you might be able to guess what's coming: what if startups could use Athena as the backing store for their embedded analytics? It would be clearly separate from the application database, while allowing SQL flexibility to explore the customer's desired analytics features.

I've done this successfully, and the general playbook is the same as that described in my [previous post](/2020/10/14/amazon-athena-simplified/). The main challenge when using Athena as a backing store for embedded analytics is the relatively large query response time. Depending on the use-case, a few seconds of load time may be acceptable; indeed, for "dynamic" embedded analytics which give the user control over query parameters this is often the norm. For regularly-viewed dashboard metrics that need to load quickly, pre-caching is often an option. For instance, you could have a cron job which pre-caches dashboard metrics on a per-user basis at a regular interval.

I've seen this option used successfully by tiny 1-person startups, and seen it scale to hundreds of thousands of users without issue, all while preserving feature flexibility and remaining cost-effective. If you're interested in setting this up for your startup, I'd love to hear about it (and help however I can)! Please DM [@tabbydata](https://twitter.com/tabbydata) on Twitter or email [dave@tabbydata.com](mailto:dave@tabbydata.com).
