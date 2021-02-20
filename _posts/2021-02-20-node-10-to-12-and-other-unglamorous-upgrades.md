---
layout    : posts
title     : "Node 10 to 12 and other unglamorous upgrades"
author    : Dennis Lim
date      : 2021-02-20 22:30:00 +0900
categories: computer science
---

This week I bumped Node from 10 to 12 in our base Docker image. That is the whole feature. I want to write about it anyway because it took four days and the four days are where the real job is.

Node 10 went end of life last spring. We knew. It was on the list. The list is long and every item on it is something nobody is asking for, so it sat there while we built hotels and a web version of the app and a push system for deals. Then a dependency we needed dropped support for 10 and the item stopped being optional.

Day one: change the FROM line. Build. Twelve services. Nine built. Three did not. One had a native module compiled against the old ABI. One had a pinned dependency that pinned another dependency that did not exist for Node 12. One failed for a reason I still do not fully understand and fixed itself after a clean build, which is the kind of fix that lets you sleep but not well.

Day two: tests. Tests passed on nine. On one they hung. Not failed, hung. Something about how timers behave when the event loop is empty changed between versions and one of our test helpers depended on the old behavior without anyone knowing. Two hours to find, five minutes to fix.

Day three: staging. Everything worked. Then someone noticed the memory graph looked different. Not worse, different. The new version has a different garbage collector default. We spent an afternoon deciding it was fine. It was fine. But you cannot skip the afternoon.

Day four: production, one service at a time, with a rollback ready. No incidents. Nobody noticed. That is the goal and it still feels anticlimactic.

Things I keep relearning:

- The pinned version that "works" is a loan. The interest is paid all at once, later, by whoever is unlucky enough to be on the team when it comes due.
- Upgrade the base image on a schedule, not when forced. Forced upgrades happen at the worst time by definition, because the force is always a deadline.
- Write down the weird one. The build that fixed itself after a clean is going to happen again and future me will want to know it happened before.

As CTO I could delegate this. I did some of it. But I did the first day myself on purpose. You cannot make good calls about technical debt from a spreadsheet. You have to feel how much the old version resists. Four days of resistance told me more about the state of our stack than a quarter of status reports.

Next on the list is the front end build. I am not looking forward to it. I am going to do it anyway, in March, before something forces me to do it in a weekend.
