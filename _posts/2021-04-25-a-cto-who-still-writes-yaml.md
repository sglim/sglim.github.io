---
layout    : posts
title     : "A CTO who still writes YAML"
author    : Dennis Lim
date      : 2021-04-25 23:15:00 +0900
categories: computer science
---

One year as CTO this month. Twenty something engineers, two products, a cluster I did not build but now own. Here is a thing I did this week that a CTO probably should not have done, and why I did it anyway.

We needed a notification system. The deals app pushes hot deals to users. The old approach was a cron job and a table, which worked until the number of users and the number of deals both grew and the job started taking longer than the interval between runs. Classic.

The new design is event driven. A deal gets created or updated, an event goes on a queue, workers pick it up, decide who should get it, and send. On our Kubernetes cluster the natural way to wire that was Argo Events with an SQS source. Queue in AWS, sensor in the cluster, trigger a workflow per batch.

I wrote the first example myself. Not the production version. The example. A sensor YAML, an event source YAML, a trivial workflow that prints the message. Committed it to the repo as a sample so the team could copy from something that actually ran.

Why did I do that instead of asking someone?

Because the fastest way to know if a design is good is to build the smallest version of it. I could have written a design doc saying "we will use Argo Events with SQS". The doc would have been fine. But I would not have known that the SQS event source needs a specific IAM shape, or that the sensor silently does nothing if you get the dependency name wrong, or that the workflow template needs a service account you have to create by hand. Those are the things that turn a two day task into a two week task, and the person who finds them should be the one who proposed the design.

There is an older reason too. Years ago, at an enterprise software company, I worked on a visualizer for the query optimizer of an in memory database. My job was to draw the plan. To draw it well I had to understand it, and to understand it I had to sit with the optimizer engineers and read their code. I learned more from drawing their system than they learned from my drawings. Since then I have believed that if you want to understand a system, build the smallest real thing that touches it. A diagram is not enough. An example that runs is.

The team took the example and built the real thing in a week. It handles the load. The cron job is gone.

Things I am careful about:

- I write examples, not production code. If I write the production code, I become the person who has to maintain it, and I do not have that time.
- I do it in the open. The example is in the repo with my name on it. If it is wrong, someone will tell me, and that is the point.
- I do it rarely. Once a month, maybe. Enough to keep my hands honest, not enough to be in anyone's way.

The title on my badge says CTO. The commit log says I wrote three YAML files this month. I think both are true and I think that is fine.
