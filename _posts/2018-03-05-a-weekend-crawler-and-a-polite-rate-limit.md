---
layout    : posts
title     : "A weekend crawler and a polite rate limit"
author    : Dennis Lim
date      : 2018-03-05 21:30:00 +0900
categories: computer science
---

I wrote a crawler this weekend. Nothing to do with work. A friend needed data from a couple of public sites that do not have an API, and I have not written a scraper for fun in a long time, so I said sure.

Two days, maybe eight hours total. Python, requests, BeautifulSoup, the usual. Here is what I remembered and what I forgot.

The site will block you. Not maybe. Will. I hit the first site a bit too fast on Saturday morning and got a captcha page for an hour. Not a ban, just a time out. The message in my README now says "use CAREFULLY" in capital letters because I know I will forget by next year.

Politeness is engineering, not manners. A delay between requests, a real user agent, respecting robots.txt, stopping when you get a 429. None of this is being nice. It is making the crawler work. An impolite crawler collects nothing after the first ten minutes.

Parse once, save raw. I made the mistake early of parsing on the fly and only saving the fields I wanted. Then I wanted one more field. Then I had to crawl again. Now every page goes to disk as HTML first, parsing is a separate step that runs on the saved files. Disk is cheap. Re-crawling is not.

Resume from where you stopped. My laptop went to sleep on Saturday night. When I woke it up the crawler had died and I had no idea which pages it had done. Sunday morning I added a tiny state file. Done ids, one per line. Restart reads it and skips. It is the least fancy thing in the code and it saved the most time.

The part that was actually interesting was the second site. It rendered everything with JavaScript, so requests got an empty shell. I did not want to bring in a headless browser for a weekend thing. I opened the network tab, found the JSON endpoint the page was calling, and hit that directly. Cleaner data than the HTML anyway. Half the time the "hard to scrape" site is the easiest one, because somebody already built an API and just did not tell you.

Why write about a toy? Because I noticed something. At work I write servers. Everything is authenticated, everything is versioned, every failure has a retry policy. Writing a scraper is the opposite. You are the unreliable client. You are the one who gets rate limited. It is good to be on that side once in a while. It makes me kinder to the clients hitting my own API.

Friend got the data. Crawler is in a repo with a scary README. I'll probably never run it again, but if I do, at least it will resume.
