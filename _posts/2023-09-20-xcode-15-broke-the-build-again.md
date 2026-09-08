---
layout    : post
title     : "Xcode 15 broke the build again"
author    : Dennis Lim
date      : 2023-09-20 22:10:00 -0400
categories: computer science
---

Every September Apple ships a new Xcode and every September something in our Flutter build that worked on Friday does not work on Monday. This year was not special. I am writing it down anyway because I keep having to rediscover the same things and maybe this saves me an hour next year.

We have a few Flutter apps in one repo. The one I spend the most time on is a tool for analyzing how people move through an interactive video, flows and segments and where they drop off. This month I was adding progress display for flows and a way to see the area sizes in the analyzer. Normal feature work. Then Xcode 15.

What broke, in order:

The linker. Xcode 15 ships a new linker and it is stricter about a thing that the old linker let slide. A couple of plugin frameworks failed to link with an error about a symbol that was clearly there. The fix was a flag to fall back to the classic linker until the plugins update. One line in the project settings. Ninety minutes to find that line.

Minimum deployment target. A dependency quietly bumped its minimum iOS version and the new Xcode enforced it where the old one warned. Bump ours to match. Check that nobody on the team still tests on the device that just got cut off. Nobody did.

Privacy manifests. Not required yet, but the new Xcode started warning about them, and warnings in our CI are errors because past me decided that in 2021. Past me was right in general and annoying in this specific case. Added the manifest.

Build numbers. Not Xcode's fault, ours. The TestFlight upload failed because the build number had already been used, because someone else uploaded from their machine during the week I was fixing the linker. Bumped it. Made a note to move build number generation into CI where it belongs, which is a note I have made before.

Total cost: about a day and a half. Every year it is about a day and a half. I could treat that as a tax and stop being annoyed by it. I am working on that.

What I actually changed this time so next year is shorter:

- A doc in the repo called "yearly Xcode upgrade" with this year's fixes and where they live. Next September, start there.
- The CI image pinned to a specific Xcode version, upgraded on purpose on a Tuesday, not by surprise on a Monday.
- Build numbers from CI. Finally. It took twenty minutes. I do not know why I put it off for two years.

The feature work shipped a day late. The analyzer shows flow progress now, and area sizes, and nobody outside the team will know that behind it is a linker flag with a comment that says "remove when the plugins catch up", which they will, eventually, probably.

Next September I will read this post, sigh, and open the doc. That is the best outcome I can design for.
