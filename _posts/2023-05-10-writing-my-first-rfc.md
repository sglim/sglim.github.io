---
layout    : post
title     : "Writing my first RFC"
author    : Dennis Lim
date      : 2023-05-10 23:20:00 -0400
categories: computer science
---

We have an RFC process now. Somebody set it up last fall: a repo, a template, a rule that big changes get a written proposal and named reviewers before code. This month I wrote my first two, and I want to write about the writing, not the proposals.

The proposals themselves are small. One says the export step should have options, so a B2C user can get just the resolution and format they need instead of everything. The other says we should describe frame timing in terms of actual frames rather than a calculated list of timestamps, because the calculated list drifts from the real frames and variable frame rate video makes it worse. Both are the kind of thing I would have just done two years ago.

That is exactly why the process exists.

Things I learned writing them:

The template forces the question I skip. "Goals and non goals". I wrote the goals in two minutes. The non goals took an hour, because writing "this RFC does not change the player" made me check whether that was true, and it was only mostly true, and mostly true is where bugs live.

Named reviewers change how you write. The template has a checklist of people who must sign off. When I know the maker engine lead is going to read the frame timing proposal, I write the section about the maker engine differently. More precisely. With the edge case they will ask about already answered. It is the same effect as code review, one step earlier.

The diagram is not decoration. For the frame timing one I drew the current flow and the proposed flow side by side. Two reviewers said the diagram was the moment it made sense. I have been producing flow diagrams for the team regularly this year and every time I am tempted to skip it, and every time it is the thing people point at.

Comments are the product. The first proposal got a dozen comments. Half were "what about X" where X was something I had not considered. The final version is better than my draft in ways I could not have gotten to alone. If the RFC had been a Slack message, those comments would not exist, because Slack is not a place where people write three paragraphs about frame timing.

The cost is real. Two RFCs took most of a week, between writing and responding. Two years ago that week would have been code. Some of that code would have been wrong, and the wrong parts would have cost more than a week to find later, but I will never be able to prove that, which is the annoying thing about prevention.

What surprised me is that I like it. I have been writing design docs since 2016 and the RFC version is better because it has a lifecycle. Draft, review, accepted or rejected, merged. A design doc in a wiki is a suggestion. An accepted RFC in a repo is a decision, with a number, that you can point at in a code review six months later.

Third one is in draft. It is bigger. I am drawing the diagram first this time.
