---
layout    : posts
title     : "First week at a startup with a monorepo"
author    : Dennis Lim
date      : 2016-03-15 23:20:00 +0900
categories: computer science
---

First week done. Engineer number five. Here is what the place looks like from the inside.

One repository. Everything. The chatbot research code, the web apps, the build tooling, the ops scripts, somebody's experiment with speech recognition. All in one git repo behind a Gerrit server. The founders came from a company famous for doing exactly this, and they brought the habit with them.

Build system is Bazel. I had heard of it. I had not used it. The first two days were mostly me trying to understand why my tiny Python script needed a BUILD file with a py_binary rule and a list of deps that I had to write by hand. By day four I got it. When every target declares exactly what it depends on, the build can cache everything and test only what changed. On a monorepo that is the difference between a five minute CI and a five hour CI.

Code review is through Gerrit. People here asked if that was a shock coming from a game company. It was not. I did an internship at the founders' old company years ago and the review culture there was intense in the same way. Every change is a "CL", it gets a reviewer, the reviewer actually reads it, and you don't submit until you get a +2. There is a pre-review lint hook that rejects your push if the formatting is off. I had missed this. At the game company review existed on paper. Here it exists in practice.

So the review was not the culture shock. Something else was.

At the game company I owned the patcher and part of the server. Clear boundary. Here, in the first week, I touched the server, wrote a Dockerfile, fixed an Ansible playbook, and sat in a meeting about what the first product should even be. Nobody said "that's not your job". There is no "your job" yet. There are five engineers and a whiteboard.

That is exciting and a little scary. In a big company you can be excellent at one thing. Here you need to be decent at ten things and know which one matters this week.

Small things I noticed:

- Everyone writes design docs before code. Even for small things. Especially for small things, because small things become big things.
- There is a users/ directory in the repo where each person has a scratch space. It's a nice trick. Experiments are in version control but clearly labeled as not real.
- The office has more monitors than people.

The product direction is still forming. Chatbots and conversational stuff is the research bet. There is also talk of a consumer travel app that would be a more concrete thing to ship. I think I'll end up on the travel thing. Servers and mobile, which is what I know.

One week in and I have already written more Python and more YAML than in the whole last year. Let's see what month two brings.
