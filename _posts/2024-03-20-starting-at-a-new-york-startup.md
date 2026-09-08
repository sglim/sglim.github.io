---
layout    : post
title     : "Starting at Koodos Labs"
author    : Dennis Lim
date      : 2024-03-20 22:15:00 -0400
categories: life
---

Started a new job last week. Koodos Labs, a small startup here in New York, and my first job where the company, the team, and I are all in the same city and the same time zone since 2022. Consumer product, a mobile app plus a web app, built around what people read and watch and play. I am on the data side of it.

Some first week notes.

Local time. My first standup at ten in the morning, in person, in years. I keep looking at the clock at nine pm expecting a meeting. There is no meeting. The two hump schedule is gone and my evenings are mine and I do not fully trust it yet.

The stack is JavaScript top to bottom. Web front end, mobile through a web wrapper, backend, data pipelines, all TypeScript. After years of Rust and Dart and Elixir and Kotlin, one language for everything is either relaxing or limiting and I have not decided which. Relaxing this week.

The problem is syncing. The product needs to know what a user has read or watched or played, and that lives in a dozen other services that were not designed to tell us. Some have APIs. Some have exports. Some have neither and you get creative. My first ticket is one of the syncers. It is broken in a way that is easy to describe and hard to fix: the source changed its format and the parser did not notice.

That is a familiar shape. It is the Shanghai bug from 2016. The pipeline ran, the output was smaller than it should be, nobody had written the check that says "this is suspiciously small". I wrote the check first and the fix second. Some habits survive every job.

Things that are different from my last few companies:

- Size. Small enough that I met everyone by Wednesday. Small enough that "who owns this" has an answer every time.
- Speed. A change I made Tuesday was in production Wednesday. Not staged, not in a release train. In production.
- Tests. Fewer than I would like. I am going to fix that quietly, one smoke test at a time, without making a speech about it. Speeches about testing have never worked anywhere I have been. Green checkmarks that catch a real bug work.

Things I am carrying in from the last two years:

- Write the doc before the meeting. Nobody here does this yet. I am going to do it and see who copies.
- Draw the pipeline. The syncers are a graph of sources and transforms and nobody has drawn it. By next month someone will have, and it will be me.
- Do not be the only one who knows. I was that person for the proto repo and it was fragile. Here I am starting as the new person, which is a good moment to insist on writing things down, because I need them written down.

Different company, different product, same city as my apartment for once. I am cautiously happy. The tracker has a project label again. Its name is not "Working".
