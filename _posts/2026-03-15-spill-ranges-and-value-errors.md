---
layout    : posts
title     : "Spill ranges and #VALUE! errors"
author    : Dennis Lim
date      : 2026-03-15 23:10:00 +0900
categories: computer science
---

Two months into the formula engine. The regression suite has grown, the easy functions are done, and I have spent the last three weeks on one topic: how far a formula spills, and when it produces #VALUE! instead. Here is what I learned, because I could not find it written down anywhere.

The setup. A dynamic array formula returns a range. The spreadsheet writes that range into the cells starting at the formula's cell, extending down and right as far as the result is big. That region is the spill extent. Other formulas can reference it with a special marker and get the whole thing.

The rules, as I now understand them, and as they cost me:

The extent is determined by the result, not by the formula text. You cannot know it by parsing. You have to evaluate. This broke my first design, which tried to compute the dependency graph before evaluation. For spilling formulas the graph depends on the values.

Nested ranges inside a function argument change the extent in ways that depend on the function. A range on the right side of an INDEX behaves differently from the same range on the left. I had that backwards for a week and the regression suite caught it on a case from a real depreciation schedule.

A scalar inside what looks like an array context is not always a scalar. Sometimes the function lifts it. Sometimes it does not, and you get a single value spilled into one cell, and everything that referenced the expected range gets #VALUE!. I have a note in the code that says "internal scalar judgment, fallback applied" and a test case with a cell reference from a real sheet that I will not forget.

IF is special. A literal branch of an IF does not participate in the spill extent. A computed branch does. This is either an intentional design decision from decades ago or an accident that became a contract. Does not matter which. Sheets depend on it.

Dynamic width. Some formulas spill to a width that depends on another cell's value. Change that cell, the extent changes, and every dependent formula's extent changes with it. The evaluation order has to handle an extent that shrinks and leaves stale cells behind. Those stale cells must be cleared, or a formula downstream reads last run's values and produces an answer that is wrong by exactly the amount of the stale cell. That bug took two days and it was found by an accountant, not by a test.

What #VALUE! actually means, after all this: "you gave me a shape I did not expect". Not "your data is bad". The shape. Once I started thinking of it as a type error rather than a data error, half the mysterious ones made sense.

Tooling that helped:

The debugger app. Step through evaluation, see the extent of every spill highlighted on the grid, see where it disagrees with the spreadsheet. I added an overlay that shows the extent our engine computed next to the extent the real sheet has. Where the rectangles differ, the bug is.

A process guide for the team about how to build input sets for regression cases, separated by which process the case exercises. Written after the second time someone filed a bug that was actually a wrong expectation in the test data. Misdiagnosis number one, avoided by a doc.

The engine passes every case it has ever seen. It will fail the next real sheet on something new. That is the job and I have made peace with it. Tax season starts in earnest next month and the sheets are about to get interesting.
