---
layout: post
title:  "Data practices for scrappy startups"
date:   2020-09-02 12:13:00 -0500
categories: []
tags: []
---

*I'm contemplating some follow-ups to this post, but want to gauge interest first. If you'd like to read more about data practices for scrappy startups, please fill out this (really short!)* [*Google Form*](https://forms.gle/6PtpnLDtxuctU9Py7)*. Thank you!*

---

<br/>
Even though the [scrappy startup I'm most enamored with right now](https://stripe.com/) is (ahem) bigger than this, the 1-, 2- and 3-person startups of the world hold a special place in my heart. There's something intoxicating about small groups of people creating something from nothing.

By the time startups reach the point where the "scale-up" happens, they usually have some aspect of a "data practice" in place. There will be an engineering side (either dedicated data engineers or application/infrastructure engineers who are "borrowed" periodically to build and maintain the data systems) and an analyst side (again, either dedicated data scientists or some set of people who are comfortable using SQL to answer business questions). Tiny startups typically don't have this, primarily for two reasons:

1.  In the very early days of a startup, the amount of user/customer activity is small and therefore the amounts of data generated are perceived as having low value.
2.  The resources required to implement a data practice are therefore perceived as being better spent elsewhere.

Yes, "perceived as."

First, small amounts of user/customer activity can still result in highly valuable data. How are people finding your new feature? Do free users engage with it as much as paid users? What can you learn from looking at cohort behavior before/after a pricing change?

Even if it's true that early data is valuable data, it might still be too expensive to implement a "data practice," right? What would it even entail? The Facebooks and Googles of the world have armies of engineers that speak another language, with words like Hive, Presto, Hadoop, and Flink, but all of that is usually a distraction for founders and early employees who must remain focused on making things people want. You could just spin up a separate database (whatever your application uses) and start dumping reporting data in there, but to do that well you'd probably want to build a frontend for it so you could include data from third parties like your CRM, your marketing email service, website analytics, etc. And you probably also want some sort of visualization tool, the stuff that enterprises call "business intelligence." And if you do your job well and collect a lot of relevant data, you'll quickly reach storage sizes that become a material part of your infrastructure costs.

So, while I still contend that there *is* a way to do all of this cost-effectively (in terms of both money *and* time!), it's easy to understand why most early-stage startups don't bother. This results in one of the few advantages big companies hold over small startups: big companies which are well-run are disciplined about using data to drive decisions. **Small startups might pay lip service to data-driven decisions, but they can't actually do this because they don't even have all their data in one place.**

The way I learned about all this, by the way, was from working in my own tiny startup a few years ago. The solution I came up with at the time was not only for internal reporting; we also used it to serve embedded analytics to our users (at a small fraction of the cost charged by third parties who specialize in this stuff). A lot of the underlying technologies have improved since then, which is great news for anyone interested in re-implementing this today.

The very rough recipe is as-follows:

1.  Use [Amazon Athena](https://aws.amazon.com/athena/) to store your data. This uses Presto and a bunch of other fancy data-land goodies under the hood, but you don't need to worry about that. Unfortunately the documentation for Athena is inscrutable to mere mortals, but there are a simple set of best-practices you can follow without becoming an Athena expert.
2.  Build a simple ingestion pipeline. This is way easier nowadays, because Athena supports SQL INSERT statements, so you can just build a normal REST API with a normal SQL backend. (It used to be necessary to interact with the S3 files that back Athena, and this was awkward.) I recommend having a single endpoint to consume all your writes, rather than separate endpoints for each resource type, because you want the process of adding new resource types to be very lightweight. You can put this API behind a queue if you envision consuming a lot of writes.
3.  Don't worry about migrating all your existing data into the system, at least not at first. (It will feel intimidating and prevent you from actually using it.) Instead, focus on instrumenting your app to copy new writes into Athena (in addition to your application database). You might also want to instrument new events, which are unrelated to your application objects---page loads, for example. You probably don't need to worry about instrumenting client-side events, at least initially.
4.  Instrument all your third-party tools to send data into Athena as well. The easiest way to do this is using something like [Zapier](https://zapier.com/). This way your entire company can be queried in one place.
5.  Now you need a convenient way to query Athena and visualize the results. The options at this point are endless (it's "just SQL" after all) and switching costs are low, so pick anything you like. [Redash](https://redash.io/) is a nice open-source option which you could either host yourself or have them host.

This approach costs approximately 1 engineer-week to set up, with an ongoing monthly cost in the low hundreds of dollars (depending on storage size and usage). The general architecture will scale from gigabytes all the way up to petabytes of data if you need it to. Best of all, once it's in place, this system will change the question from "How can I measure this?" to "What should I measure?" which is a much more powerful question.

<br/>

---

<br/>
_**I'm contemplating some follow-ups to this post, but want to gauge interest first. If you'd like to read more about data practices for scrappy startups, please fill out this (really short!) [Google Form](https://forms.gle/6PtpnLDtxuctU9Py7). Thank you!**_
