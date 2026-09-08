---
layout    : post
title     : "It's the concept, not the word"
author    : Dennis Lim
date      : 2026-09-07 03:30:00 +0900
categories: life
---

A few weeks ago someone said "let's triage this first" in standup. Everyone nodded. After the meeting one of the newer guys asked me, kind of quietly, what triage actually means. Not the dictionary meaning. What we actually do when we say it.

I gave him a short answer but the question stayed with me.

Try this. Pretend you don't know the word triage. Never heard it. Someone drops 40 bugs on you and says, figure out what to fix first. What do you do?

You start sorting. This one crashes the app for everybody, goes to the top. This one is a typo on a settings page nobody opens, goes to the bottom. This one you are not sure, so you put it somewhere in the middle and come back later. You end up with a few buckets. Maybe three, maybe five. You argue with a coworker about where one of them belongs.

That is triage. You just did it without the name.

Now the opposite. Say you learned the word from a book. Triage, sorting cases by urgency, comes from military medicine, three categories, and so on. You can recite it. But when those 40 bugs land in front of you, do you know that the Android crash goes to the top because it hits 30% of users, not because the word "crash" sounds scary? Do you know when to skip the whole thing because there are only 3 bugs and sorting them takes longer than fixing them?

That part is not in the word. It is not in the procedure either.

This is what I mean by concept. The word is a label. The procedure, step one, step two, step three, is one particular way somebody wrote it down for their situation. The concept is the thing under both. It is the reason the steps are in that order. If you have the concept you can rebuild the steps when you forget them. You can also break the steps when they don't fit, and you know why you are breaking them.

I see this a lot with developers, and I was the same. We collect vocabulary. Dependency injection, idempotent, eventual consistency. We memorize the code review checklist. We learn that you "should" write the test first. Then the situation looks a little different from the book and we freeze.

And the senior person next to you seems to just know. It looks like they are skipping the steps. They are not. They have the concept, and the steps come out of it by themselves.

So these days when I learn something new I ask myself a dumb question. If I forget this word tomorrow, can I still explain what problem it solves and how I would go about it? If no, I only learned the word. Not the thing.

Words are cheap. You pick them up from a Slack thread. The concept is the part you have to earn.
