---
layout    : posts
title     : "Syncing Goodreads, again"
author    : Dennis Lim
date      : 2024-07-30 22:30:00 -0400
categories: computer science
---

I rewrote the Goodreads sync this month. Version two. Version one was written before I joined, worked well enough, and had reached the point where every fix made two other things worse. I want to write about the rewrite because rewrites are usually a mistake and this one was not, and it is worth being clear about why.

The context: users connect their reading history and we pull in what they have read, are reading, and want to read. There is no proper API for this anymore. There are exports, there are pages, there is a small amount of creativity. Version one handled the common case and fell over on the edges. Users with thousands of books. Users who had deleted shelves. Books with the same title by different authors. Rate limits that changed depending on the time of day, apparently.

Why a rewrite instead of fixes:

The core loop of version one assumed you could fetch everything and then process it. That is fine for a hundred books. For five thousand it meant a request that timed out, and the retry started from zero, and the user never finished syncing. No amount of patching fixes an architecture that assumes the wrong size. The right shape is a cursor: fetch a page, process it, store the cursor, fetch the next page. Interruptible, resumable, boring.

Once I was rewriting the loop, the rest followed. The filter for which items to keep moved from "after fetching everything" to "per page", which meant the memory footprint stopped depending on the user. The dedup logic moved from title matching to a proper key. The concurrency setting became a config value instead of a constant, and the config value is two, because two is still the number.

What I kept from version one: everything that touched the actual page structure. That code was ugly and it was correct, because it had been fixed forty times against real pages. Rewriting it would have thrown away forty lessons. I moved it into its own module, wrote tests around its current behavior, and left it alone.

That is my rule for rewrites now. Rewrite the shape. Keep the scars.

Some numbers, rough: users who previously never finished syncing now finish. The slowest sync we have seen is under an hour where before it was infinite. Support tickets about missing books dropped enough that the support person asked what changed.

A thing I did that I am slightly proud of: before shipping it to everyone, I turned it on for the team. Two weeks. Everyone here has an account with a real, messy history, and the team found four bugs that my test accounts did not. Expanding the test range to real people with real data before real users is the cheapest QA there is.

What is left: images. Cover images come from a different source and that source has its own opinions about rate limits. That is August's problem.

Two months into version two and I have not had to touch the core loop once. That is the metric. Not that it works. That I stopped thinking about it.
