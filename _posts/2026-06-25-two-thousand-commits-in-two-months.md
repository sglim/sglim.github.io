---
layout    : post
title     : "Two thousand commits in two months"
author    : Dennis Lim
date      : 2026-06-25 23:30:00 +0900
categories: computer science
---

I looked at the commit count on the project I started in April. Over two thousand commits. In under three months. Most of them mine, most of them written by an agent, all of them reviewed by me. I have been putting off writing about this because my opinions were not settled. They are settled enough now.

How it works, mechanically: I describe what I want. The agent reads the code, proposes a change, runs the tests, and opens a small commit with a message that explains what and why. I read the diff. I accept it, or I say what is wrong and it tries again. Repeat, many times a day. The commits are small because I make them small. A big one I cannot review, and a change I cannot review does not go in.

What changed about the work:

I read more than I write. By a lot. My job has become review, direction, and taste. Whether the abstraction is right. Whether this is the file the change belongs in. Whether the test actually tests the thing. The typing is not the bottleneck anymore and it turns out it was never the interesting part.

The regression suite is the whole game. On the formula engine, and now on the integration project, the reason I can accept a change in thirty seconds is that a thousand cases will tell me if it broke something. Without that, review would have to be exhaustive and the pace would collapse. I said tests let you move fast. Now I would say tests are the speed limit. The better they are, the faster you are allowed to go.

Commit messages became documentation. Two thousand commits, each saying what changed and why, in a consistent format, is a history I can actually search. I have gone back to a March commit to understand a June bug more than once. The agent writes better commit messages than I do, because it does not get tired at four pm.

What worries me:

Understanding. This is the thing I keep coming back to. When you write the code, you know it. When you review the code, you know it less. When you review two thousand changes, there are parts of the system you have approved but could not rebuild from memory. I have started writing a short design note per module, by hand, in my own words, as a forcing function. If I cannot write the note, I do not understand the module, and I should not be approving changes to it.

Ownership. Every one of those commits went in under my name. "The agent wrote it" is not an answer I would accept from a junior and it is not one I accept from myself. If it is wrong, it is mine. That framing has made me a stricter reviewer than I was of humans, which is uncomfortable to admit.

Drift. Small changes are easy to approve individually and add up to an architecture nobody chose. Once a week I read the whole module, not the diffs, and ask if this is still the shape I would have drawn. Twice I said no and we spent a day undoing a week of small correct steps in the wrong direction.

What I believe, for now:

The rate is real and it is not going back. The skill that matters is the one I have been building since the design docs at Skelter and the RFCs at Momenti: holding the whole system in your head and knowing when a change does not fit. That was always the senior engineer's job. It is now the only job.

Every engineer is a lead now, with a few tireless juniors who never sleep and never push back. That is a strange thing to be. I am still learning how to be it well.
