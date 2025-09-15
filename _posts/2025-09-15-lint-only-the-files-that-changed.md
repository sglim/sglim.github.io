---
layout    : posts
title     : "Lint only the files that changed"
author    : Dennis Lim
date      : 2025-09-15 22:20:00 +0900
categories: computer science
---

A small change this week that I want to write down because it is the kind of change that saves everyone ten minutes a day and nobody ever puts on a slide.

One of our products has a CI step that runs the linter and formatter across the whole repository. Good practice. Except the repository is large, the tool is fast but not instant, and every pull request was paying the full cost even when it touched one file. Worse, the whole repo check kept failing on files nobody in the PR had touched, because someone had merged something slightly off last week and now everyone downstream inherited a red check.

So people started ignoring the red check. Which is the actual failure. A check that is red for reasons unrelated to your change is not a check. It is noise that trains people to click merge anyway.

The fix is small. Lint only the files changed in the PR. Git can tell you which files those are. Pass that list to the tool. The check goes from a minute to a few seconds and, more importantly, it is only red when you broke something.

It took a couple of tries to get right:

Renamed files show up as delete plus add. Handle both sides.

Deleted files must be filtered out, or the linter complains about a path that does not exist and the check goes red for the dumbest possible reason.

The base branch matters. Diff against the merge base, not against the tip of main, or a PR that has been open for a week gets blamed for everything merged since.

And then the whole repo check still runs, but on main after merge, not on PRs. If something slips through, main goes red and one person fixes it, instead of forty PRs going red and everyone ignoring it.

This is not clever. It is the same idea as Bazel testing only what changed, which I first met at Skelter in 2016, scaled down to one linter in one repo. The principle is identical: the cost of a check should be proportional to the change, or people will route around the check.

What I keep noticing at this company is how much of the AI assisted workflow depends on exactly this kind of hygiene. The agents open a lot of small PRs. If every small PR costs a full repo lint, the agents wait, and the people waiting on the agents wait. Fast, precise checks are not a nice to have when the commit rate is that high. They are the thing that makes the rate possible.

Ten minutes a day, forty people, that is a lot of hours a year for a change that took an afternoon. I will never be able to prove that number and I believe it anyway.

The bigger integration work is starting next quarter. That will not be a small post.
