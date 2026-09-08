---
layout    : post
title     : "A headless Mac, launchd, and a Stream Deck"
author    : Dennis Lim
date      : 2026-05-20 22:40:00 +0900
categories: computer science
---

There is a Mac mini on a shelf at home with no monitor attached. It runs my life. This post is about how that happened and what it does, because a few people asked.

It started with one script. I wanted a thing to run every morning and I did not want to think about it. On a Mac that means launchd, which is the system's job scheduler. You write a small property list file, put it in the right folder, and the system runs your job on the schedule you describe, restarts it if it dies, and logs where you tell it to. It is cron with opinions. The opinions are mostly good.

One script became five. Five became a repo where the plist files live next to the scripts they launch, so the whole setup is version controlled and I can rebuild the machine from a clone. A launchd job is not real until its plist is in git. That is the rule and it has saved me once already.

What runs on it now:

A personal stats project. It collects numbers I care about from a handful of sources, stores them, and renders a page. A dashboard for my own life, updated on a schedule, with no cloud in the middle. The collection jobs run at night, the render runs in the morning, and by the time I have coffee the page is current.

Home things. Lights, a few sensors, a couple of automations. The Mac is the always on machine, so it hosts the pieces that need to be always on.

Development helpers. Backups, a git mirror, a job that checks whether my other machines are reachable and messages me if one is not.

And the Stream Deck.

I have one of those little fifteen button panels. The official software wants a desktop session and a logged in user. My Mac has neither. So I wrote a small daemon that talks to the device directly, no vendor app, and maps the buttons to things on the machine. Lights on and off. A scene. Play music to the speaker in the kitchen. Trigger a launchd job on demand instead of waiting for its schedule. Show a status light per server, green or red, so I can glance at the shelf and know if something is down.

It is the most satisfying thing I have built this year and it is maybe four hundred lines.

Why this matters more than a hobby post:

I have spent a decade building systems for other people that run on machines I never touch. Cloud, containers, someone else's kernel. Having one physical box that I fully own, that runs my own jobs on my own schedule, and that I can walk over to and press a button on, has changed how I think about the work. Every job on it has a name, a log, a schedule, and a reason. When something breaks I know within a minute because the button is red. I want the systems at work to feel like this and most of them do not.

The 3D printer is back in service, by the way, and it printed the mount that holds the Stream Deck to the shelf. Some things come around.

Next: the same setup, documented well enough that someone else could copy it. Which means writing it down properly. Which means this post was the first draft.
