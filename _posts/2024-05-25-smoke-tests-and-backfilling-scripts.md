---
layout    : post
title     : "Smoke tests and backfilling scripts"
author    : Dennis Lim
date      : 2024-05-25 21:50:00 -0400
categories: computer science
---

Two months in. Most of my commits this month fall into two buckets that nobody puts on a roadmap: tests that catch the obvious, and scripts that fix the past. Here is why I think those two buckets are the real job right now.

Smoke tests.

The syncers pull from outside services. The outside services change without telling us. The failure mode is never a crash. It is a parser that returns an empty list because the HTML moved, and a pipeline that happily writes zero rows, and a user who notices a week later that their history stopped updating.

So I added a smoke test per syncer. Not a unit test. A test that actually hits the source with a known account and asserts that it gets back roughly what it got back last time. More than zero items. The expected fields present. Runs on a schedule, not just on deploy, because the source can change on a Sunday.

I renamed them from "integration test" to "smoke test" in the same change. That sounds petty. It is not. When they were called integration tests people expected them to be thorough and complained when they were not. When they are called smoke tests everyone understands the contract: if this fails, something is on fire, go look. Naming is documentation.

Two have fired since I added them. Both were real format changes. Both were fixed the same day instead of the same month.

Backfilling.

When you fix a parser, you have a choice. Fix it going forward and accept that the last three weeks are wrong, or go back and reprocess. Users notice holes in their history. So we backfill.

I wrote a backfill script that takes a date range and a list of users and reruns the sync for that window. First version was naive: one user at a time, one request at a time. It would have taken days for the whole user base. Second version batches requests and runs a few users concurrently. I tried a concurrency of three and got rate limited by the source. Two works. Two is not a number I would have guessed. You find these numbers by getting blocked, which is the 2018 crawler lesson wearing a suit.

A small thing that turned out to matter: the script deletes and rewrites history entries by user id, not by matching content. The first version matched content and left duplicates when the source changed a title by one character. Delete by id, rewrite, done. Idempotent. You can run it twice and the second run does nothing. That property is worth more than any optimization.

The pattern I keep seeing, here and at every job:

The exciting work is the new feature. The work that keeps the product trustworthy is a smoke test that fails at 6am and a script that quietly repairs three weeks of data before anyone notices. Nobody demos a backfill. But the reason the demo of the new feature has real data behind it is that the backfill ran.

I have done this at four companies now. Flights, hotels, video, and now this. The data changes every time. The shape of the work does not.
