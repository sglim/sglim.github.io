---
layout    : post
title     : "Things I built while not working"
author    : Dennis Lim
date      : 2023-12-15 22:20:00 -0500
categories: computer science
---

A month into the pause. I said I was working anyway. Here is what that produced, because small finished things are what I have to show right now and I want to show them.

A real estate crawler. We might need to move, and the listing sites here are terrible at "show me what changed since yesterday". So I wrote a crawler that pulls listings for a few neighborhoods on a cron, diffs against the last run, and sends me a message with what is new and what dropped in price. Python, a database, a cron entry, a message. Two days.

The same lessons as the 2018 weekend crawler applied. Save raw pages first. Resume from a state file. Be polite or be blocked. I was blocked once. The README says CAREFULLY in capitals again. Some things do not change.

What was new: the diff is the product. The crawl is boring. Seeing "this two bedroom dropped by two hundred dollars overnight" is the thing I actually open every morning. I should have known that from the hotel matching work. The value is never in fetching the data. It is in noticing what changed.

A travel money script. Old Django project I resurrected because I had a spreadsheet problem. When you live in one country and get paid in relation to another, and travel, you end up with expenses in three currencies and no clear picture. The script pulls rates, normalizes everything, and tells me what a month actually cost. It is ugly. It runs on Django 1.11 which is embarrassing. It works and I use it.

A Flutter app that is not ready to talk about. It came out of the analyzer work and the motion vector stuff. Video, gestures, the same space I have been in for two years but pointed at something different. I have a prototype that does one thing. I am not going to describe it until it does two.

Things I noticed about working without a company around me:

I finish things. At work a project is never finished, it is deprioritized. Here the crawler is done. It does what I wanted and I stopped. That feeling has been rare for years.

I have no one to review my code and it shows. Two bugs in the crawler that a reviewer would have caught in a minute took me an evening each. I am considering asking a friend to review side projects just for the discipline.

The time tracker says I average around five hours a day. Less than the job. More than I expected for a month with no deadline. The rest of the day is walks, coffee with people, and an amount of YouTube I am not proud of. The tracker is honest about that too. Three months from now I will have a number and I will decide whether to be embarrassed then.

Visa update: there is a path. It is not fast. I am on it.

Job update: conversations are happening. Nothing to say yet. The crawler is running while I wait, and every morning it tells me what changed. I find that reassuring in a way that has nothing to do with apartments.
