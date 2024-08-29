---
layout    : post
title     : "Teaching students to build a crawler"
author    : Dennis Lim
date      : 2024-08-28 22:30:00 -0400
categories: computer science
---

Last night was the eighth and last session. For two months, every Tuesday at five, I have been on a Zoom call with a group of university students teaching them to build a web crawler from nothing. Tonight I don't have a class to prepare and it feels strange.

The setup: Open Avenues runs a program where people working in industry design an eight week project and run it for students at partner universities. I signed up as a fellow this spring. My project was called "Build your crawler". Node.js, a website of their choosing, structured data out the other end, and a Slack message when something changes. Sixty minutes a week, plus homework, plus a lot of messages in between.

I picked crawling because it is the thing I keep writing about here. The weekend crawler in 2018, the apartment listings last winter, the syncers at work right now. It is also the best first project I know for teaching how the web actually works. You cannot scrape a page without learning what a request is, what a response is, what the browser does with HTML, and why the site you want is built the way it is.

The eight weeks, roughly:

1. Install Node and an editor. What a crawler is. Half the hour went to one student's PATH. It always does.
2. JavaScript basics. Variables, functions, promises. I skipped classes entirely. Nobody needed them for this.
3. A static site scraper. Fetch a page, parse it, print a list. The first "oh" of the course, when the terminal filled with headlines from a real site.
4. Structured data. Turning the list into objects with fields. Deciding what a field is. This took longer than the code.
5. Saving to a file. JSON, then CSV, then "why is my Korean text broken" and a detour into encodings.
6. A dynamic site. The page is empty until JavaScript runs. Open the network tab, find the real endpoint, call that instead. The second "oh".
7. Tracking changes. Run it again, diff against last time, post the diff to a Slack channel through a webhook. Block Kit for making it pretty.
8. Build your own. Each student picked a site and presented what they built.

Things I learned about teaching, which is not my job:

The slide deck is for me, not them. I made decks every week. What students actually used was the repository of sample code from each session. Next time, less deck, more code with comments.

Every student hits a different wall and the wall is never the topic of the day. It is Node versions, or a corporate laptop that blocks npm, or a site that returns 403 to anything without a user agent. I started keeping fifteen minutes at the end for walls.

Explaining robots.txt to people who have never thought about it is a good reminder of why it exists. One student asked whether it was okay to crawl a site that asked not to be. We talked about it for a while. The answer I gave was: it is not about whether you can. I think that landed.

The final presentations were better than I expected. A price tracker for a game store. A scraper for university course openings that messages you when a seat frees up. One student built a dashboard on top of hers and it looked better than things I have shipped.

I do not know if I will do this again. It cost a real evening every week during a period where I did not have many evenings to spare. But eight people can now open a network tab, find the endpoint, and get their own data, and a few of them will never stop doing that. That seems like a fair trade.

The sample code is public, workshop by workshop, if you want to run the same course for someone.
