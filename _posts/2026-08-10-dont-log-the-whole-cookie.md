---
layout    : post
title     : "Don't log the whole cookie"
author    : Dennis Lim
date      : 2026-08-10 22:20:00 +0900
categories: computer science
---

Short one. I found a bug in my own code this week that I want to write down, because it is the kind of bug that does not crash anything and is worse than most bugs that do.

The login service for the government integration logs what it does. It has to. When the sequence fails, the log is the only way to know which step broke. In December, when I first built it, I logged the session state at each step. Including the cookie header. The whole thing.

Nothing was wrong with the log line. It was helpful. It was also writing every user's active session token into a log file that a number of people and systems could read. A session token is a password that expires. Logging it is logging passwords with a timer on them.

I caught it in December, before an audit, and fixed it, and felt good about that. Then this week, reviewing a change to the login flow, I found a second place. A different branch, an error handler, added in a later commit, logging the same header. The agent had written it, following the pattern of the surrounding code, which was my pattern from December before I fixed it. I had approved it. It went in under my name.

The fix is small. Mask the value. Keep the first few characters so you can correlate across log lines, replace the rest. One helper, used everywhere a header is logged. The interesting part is not the fix.

The interesting part is that a pattern I had already corrected in one place propagated to another because the code still looked like the old way to anyone, human or agent, reading it for style. I fixed the instance. I did not fix the shape. The shape said "we log headers here" and the next change followed the shape.

What I changed this time:

The helper is the only way to log a header. The raw header type does not implement the formatting trait. If you try to log it directly, it does not compile. Hard constraint, not a review note. I have written before that soft suggestions get ignored and hard constraints shape what is even possible. This is that, applied to myself.

A test that greps the log output of the full login sequence for anything that looks like a token. If it finds one, the suite fails. It would have caught both instances.

A line in the module design note that says, in plain words, "never log session values, see August 2026", because the note is what I read before approving changes to this module and the note did not say it.

The lesson is not "be careful". I was careful. I caught it the first time. The lesson is that when most of the changes in a system are written by something that learns from the surrounding code, the surrounding code is the specification. Fixing an instance is not enough. You have to fix what the code teaches.

No data left the building. The audit is next month. I will sleep fine. I am still annoyed at December me, and a little at June me, and I think that is the right amount.
