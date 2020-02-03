---
layout: post
title:  "Toward my own memex"
date:   2020-02-03 09:53:27 -0500
categories: []
tags: []
---
# Toward my own memex
Vannevar Bush famously described the [memex](https://en.wikipedia.org/wiki/Memex) as "an enlarged intimate supplement to one's memory." And today, even though we've realized hypertext and a plethora of other advancements which Bush could only dream of, Bush' dream of a memex still eludes us.

What we have instead is absurd amounts of storage space, and a compulsion to hoard any data we can amass into it. I am as guilty as anyone else: piles of used notebooks, never to be read from again. Thousands of entries in Evernote, Workflowy, Apple Notes, Google Docs, and any other digital dumping ground the universe provides. Blog posts, lost to the sands of time, or maybe only to the [Internet Archive](https://archive.org/web/). The point is, we've all made a habit of writing down our thoughts, but--at least in my case--that writing has not resulted in significantly improved accretion of knowledge.

A short while ago, I watched [Andy Matuschak](https://andymatuschak.org/) give a talk about this very subject, and it was illuminating. I will not attempt to summarize his entire talk here (although he's done some podcast interviews which are broadly similar and worth a listen), but I will say that there were four key takeaways for me:

1. Most knowledge workers' note-taking habits are garbage.
2. It is possible to make memorization a choice, thanks to [spaced repetition](https://www.gwern.net/Spaced-repetition).
3. It is possible to use note-taking as a tool for knowledge accretion by using the [Zettelkasten method](https://zettelkasten.de/posts/overview/).
4. The concept of spaced repetition can be applied to more than just flash cards. ("Spaced everything," as Andy calls it.)

After reflecting on these thoughts for a while, I came up with a fifth takeaway: _programmers should build their own tools_. (I don't actually know if this was reflected in Andy's talk, but it came around the same time for me.)

And all of that led me to reflect on the memex once more. Studying the Zettelkasten approach, one quickly realizes that it's not too different from Bush' idea for a memex. Intimate ideas, interconnected. The remaining problem, then, is that the Zettelkasten has no in-built mechanism for strengthening the bonds between what's written down in external memory (notes) vs. what's encoded in internal memory (brain). Enter "spaced everything."

Last month I completed my first memex prototype. It's an intimate tool, and it's built just for me, but I'm excited about how it works:

1. The interface is browser-based, so I can easily add notes from anywhere. They're stored as Markdown files in S3.
2. It has a global search functionality, which makes it easy to (a) find notes by title, (b) find notes by tag, (c) find notes by reference, e.g. "which other notes link to this one?"
3. The editor has a typeahead feature, so I can easily reference existing notes and tags when they exist.
4. I can use tags to define "inboxes." I have inboxes for: long-form writing works-in-progress, loose thoughts that I have yet to encode in a Zettel, things that I want to read later, or read more thoroughly, tasks that I have to do, questions or ideas that I want to research, and more besides.
5. The interface has a global "spacing" mode. Effectively, this turns every single entry of any sort into something like an Anki card, and hides any entries which are not scheduled for review. (This is the most powerful feature of the entire tool, and a concept which I shamelessly stole from Andy Matuschak.)

Together, those features enable the following daily workflow:

1. Look through all inboxes. (In fact, currently my set of notes is small enough that I actually don't filter by tag and rely purely on the spacing function.)
2. "Skip" anything which won’t get addressed that day, after reviewing any entries that need to be reloaded into short-term memory. (The spacing function automatically determines how long to skip it for, in exponentially increasing increments.)
3. If there is something truly important or desirable which won’t get done, "bump" it (this is an interface feature which resets the skipping increment to zero) and then "skip" it for the current day. (This will cause it to recur the next day.)
4. "Inbox zero" anything that's left.

I'm only a few weeks into this process, but it's _really_ exciting so far. It feels hugely liberating to have a system that automatically brings previous notes to the forefront for me, on an interval that is in my control but which doesn't require much thought. I've already added about 100 notes, and many of them are only roughly formed ideas, but I can have confidence that (a) they won't get lost, (b) I will review them in the future, and (c) the worthwhile ideas will have many opportunities to be expanded upon, while the less interesting ones will organically fade into the background. It's also a much less intimidating approach than "starting a Zettelkasten" since the first draft of a note can just be dropping something informal into my writing inbox, to be revisited later.