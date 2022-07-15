---
layout    : posts
title     : "A voting system for demo day in three days"
author    : Dennis Lim
date      : 2022-07-15 23:40:00 +0900
categories: computer science
---

The company runs a demo day. Teams show what they built, everyone votes, someone wins. This year the person organizing it asked, on a Monday, if we could have a real voting system by Thursday instead of a shared spreadsheet. I said yes because I like small deadlines and because I move to New York in two weeks and wanted one clean, finished thing before I go.

Three days. Here is what got built and what got cut.

Requirements, as I understood them after ten minutes:

- Everyone in the company can vote. Once. From their phone.
- Votes are hidden until the organizer opens the results.
- Multiple categories. Best demo, most useful, audience favorite.
- It has to not fall over when sixty people tap at the same second.

Day one was the boring part. Auth by company login, a table of entries, a table of votes with a unique constraint on voter plus category. That constraint is the whole integrity story. Not a check in the code, a constraint in the database. If two taps race, the database rejects one. I do not have to think about it.

Day two was the screens. Vote page, results page, admin page to open and close voting. I built the results page as a live view so the organizer could put it on the projector and watch the bars move. That was not a requirement. It was the thing that made people cheer, which I have learned is worth an unplanned afternoon.

Day three was the part I always forget to budget. Deploy, test on real phones, discover that one browser on one phone does not send the cookie the way I expected, fix, redeploy, write a two paragraph doc for the organizer so they do not need me on the day.

What got cut: ranked voting, comments, any kind of design beyond default components. The organizer asked about ranked voting on day two. I said next year. Saying next year on day two is how you ship on day three.

It ran on Thursday. Sixty something votes, no duplicates, no crashes, the live bars did their job. The organizer took over the admin page and I did not touch it during the event, which is my definition of done.

Why write this up? Because it is the same shape as every hackathon post I have written. Small scope, one owner, cut on the second day not the last, put the constraint in the database, budget for the day nobody budgets for. I have known these things since 2017. I still have to remind myself every time.

Also because it is my last small project from the Seoul office. Two weeks from now I will be writing from a very different desk.
