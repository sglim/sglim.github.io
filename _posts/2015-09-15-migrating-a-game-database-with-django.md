---
layout    : post
title     : "Migrating a game database with Django, of all things"
author    : Dennis Lim
date      : 2015-09-15 23:40:00 +0900
categories: computer science
---

We have a problem that every live game has. The schema changes. Every build adds a column here, renames a table there, and the DB people have to write the migration SQL by hand, run it on dev, run it on QA, run it on staging, and pray on production.

The scripts were kept in a folder. Named by date. Some of them had "final" in the name. Some had "final2". You know how it goes.

I had used Django for years before this job, side projects and some freelance work. And the one thing Django does really well, better than almost any framework, is migrations. You change the model, you run makemigrations, it generates the migration file, it tracks which ones have been applied in a table, and it refuses to run them out of order.

So I asked a dumb question in a meeting. Why don't we use Django migrations for the game DB?

The first reaction was "the game server is C++, why would we bring Python into this". Fair. But the migration tool does not need to be the game server. It just needs to know the schema. So I wrote Django models that mirror our tables. No views, no templates, nothing. Just models and the migration framework. The game server never knows Python exists.

What we got out of it:

- Every schema change is a file in git, generated, not typed.
- The database itself remembers which migrations ran. No more "did we apply 0913_final2 on staging?"
- Rollback exists. Not perfect, but it exists.
- New dev environments go from empty DB to current schema with one command.

What we did not get: a free lunch. Some of our tables have game specific tricks that Django's ORM does not understand, so those migrations are hand written RunSQL blocks inside the Django migration. Still tracked, still ordered, but not generated. And I had to explain to a few people that no, we are not rewriting the server in Python.

The thing I keep thinking about is that the good idea was not mine. Django's migration system is years old. Thousands of web developers use it every day. The only thing I did was notice that our problem was the same problem, just wearing armor and carrying a sword.

I think a lot of engineering is like this. The hard part is not inventing the solution. It is recognizing that your special snowflake problem is actually the same shape as a boring problem somebody solved already.

Launch is in a few months. The migration is the one part of the release process I'm not worried about anymore. That is a good feeling.
