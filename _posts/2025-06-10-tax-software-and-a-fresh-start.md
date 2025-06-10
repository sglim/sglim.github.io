---
layout    : posts
title     : "Tax software and a fresh start"
author    : Dennis Lim
date      : 2025-06-10 22:50:00 +0900
categories: life
---

New job. A Korean company that builds tax and accounting software for small businesses. I know nothing about tax. That was the appeal.

Let me explain that because it sounds like a bad reason.

I have spent a decade on products I could explain to my parents in one sentence. Flights. Hotels. Interactive video. A reading tracker. The tax domain is the opposite. It is deep, it is regulated, it changes every year by law, and the people who understand it are accountants, not engineers. Which means the engineering problem is not "build the feature". It is "understand a system that was designed by lawyers over fifty years, and make software that agrees with it exactly". Exactly. Off by one won is a bug that a customer will find.

I like that kind of problem. It is the hotel matching problem with higher stakes. Identity, consistency, sources that disagree, and a ground truth that lives in a government system you do not control.

The arrangement is unusual and I like it. I am not an employee yet. I am coming in as a contractor for a few months, with the understanding on both sides that if it works, I take the CTO seat. A trial. They get to see how I actually operate before handing me the engineering org, and I get to see the codebase, the team, and the founders before I commit years to them. After the last job I wanted exactly this and I did not expect anyone to agree to it.

So while we figure each other out, I am picking up the work nobody owns. Infrastructure first, because the company grew fast and the infrastructure grew with it in the way infrastructure does when nobody is the infrastructure person. Kubernetes, deployment, secrets, the internal git server, who can access what. It is also the fastest way to learn how a company really works. Read the deploy pipeline and you know what the team values. Then the integration work with the national tax service, which is its own adventure that will get its own post.

First weeks, what I found:

A lot of things that work and nobody knows why. This is normal at a company this age. Someone set it up, it worked, they moved on. My first month is mostly writing down why.

A team that is good and busy. Nobody has time for the infrastructure work, which is why it is the first thing I am touching. I am not going to be precious about it. Unglamorous work is the thing I am apparently for, and it is a good way to earn a room before you try to lead it.

AI in the workflow, for real. In New York we used the assistants a bit. Here the team has built the way they work around them. Agents run in the repo, review comments come from a bot before a human, and the commit history has a texture I have never seen. Hundreds of small, well described commits a week from one person. I have opinions forming about this and they are not settled yet. Ask me in six months.

What I am bringing:

The doc before the meeting. The diagram of the system. The smoke test that fires at six am. The same three things I have brought everywhere, and they land the same way every time: politely ignored for a month, then quietly copied.

What I am learning:

Korean tax law, badly, one term at a time. My notes file is mostly definitions. I now know what a simplified taxpayer is and I did not need to know that for thirty nine years.

Fresh start, domain I do not know, a trial run at a job I have done once before and want to do better. On paper it is not exciting. In practice I have not been this curious about a job in a while. Let's see if it lasts.
