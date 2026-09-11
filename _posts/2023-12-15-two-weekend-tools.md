---
layout    : post
title     : "Two weekend tools"
author    : Dennis Lim
date      : 2023-12-15 22:20:00 -0500
categories: computer science
---

December in New York, the product work is between milestones, and I have had a few evenings to myself. Here is what they produced. Small finished things, which I like more every year.

A real estate crawler. Our lease is up next year, and the listing sites here are terrible at "show me what changed since yesterday". So I wrote a crawler that pulls listings for a few neighborhoods on a cron, diffs against the last run, and sends me a message with what is new and what dropped in price. Python, a database, a cron entry, a message. Two days.

The same lessons as the 2018 weekend crawler applied. Save raw pages first. Resume from a state file. Be polite or be blocked. I was blocked once. The README says CAREFULLY in capitals again. Some things do not change.

What was new: the diff is the product. The crawl is boring. Seeing "this two bedroom dropped by two hundred dollars overnight" is the thing I actually open every morning. I should have known that from the hotel matching work. The value is never in fetching the data. It is in noticing what changed.

A travel money script. Old Django project I resurrected because I had a spreadsheet problem. When you live in one country and get paid in relation to another, and travel, you end up with expenses in three currencies and no clear picture. The script pulls rates, normalizes everything, and tells me what a month actually cost. It is ugly. It runs on Django 1.11 which is embarrassing. It works and I use it.

A Flutter app that is not ready to talk about. It came out of the analyzer work and the motion vector stuff. Video, gestures, the same space I have been in for two years but pointed at something different. I have a prototype that does one thing. I am not going to describe it until it does two.

Two things I noticed about side projects, again:

I finish them. At work a project is never finished, it is deprioritized. The crawler is done. It does what I wanted and I stopped. That feeling is rare and I should manufacture it more often.

I have no one to review the code and it shows. Two bugs in the crawler that a reviewer would have caught in a minute took me an evening each. I am going to ask a friend to review side projects, just for the discipline.

The crawler is running. Every morning it tells me what changed. Most mornings the answer is nothing, and I find that oddly satisfying too.
