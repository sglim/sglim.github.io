---
layout    : posts
title     : "Elixir, Phoenix, and GraphQL over websockets"
author    : Dennis Lim
date      : 2023-02-15 22:30:00 -0500
categories: computer science
---

Our backend is Elixir. I have known this for a year and a half and mostly stayed on the client and engine side, because that is where the fires were. This winter the fire moved. The web player needed live updates from the server, the server team was thin, and I ended up writing Elixir for real.

Some notes from someone who came to it late.

Elixir reads like Ruby and runs like Erlang. That sentence is a cliche because it is accurate. The syntax is friendly. The runtime is the thing. Every connection is a process, processes are cheap, they crash independently, and a supervisor restarts them. After years of Node where one unhandled rejection can take down a worker, it is a relief to have a system where "let it crash" is a design principle and not a bug report.

Phoenix channels are websockets done properly. A channel is a process per client per topic. Join, push, broadcast. The player subscribes to a topic for the media it is showing and the server pushes changes. I wrote the first version in a day and spent the rest of the week learning why the first version was wrong, which is the normal ratio.

The interesting part was GraphQL over the same websocket. Our clients already spoke GraphQL to the API over HTTP. I did not want a second protocol for live data. The Absinthe library does subscriptions over Phoenix channels, and there is a small link package that lets a client use one socket for queries, mutations, and subscriptions. I contributed a couple of fixes to it. Small ones. The kind where you spend two hours understanding the library to change six lines.

What I liked:

- Pattern matching in function heads. Once you write handlers this way, callbacks with if statements look primitive.
- The pipe operator. Data goes in, flows through transforms, comes out. It matches how I already think about the engine.
- Observability out of the box. The runtime will tell you how many processes are alive and what each one is doing. I have built that by hand in other stacks. Here it is a function call.

What I did not like:

- The ecosystem is small. For anything unusual you either find one library maintained by one person or you write it. I wrote more than I expected.
- Compile times on a big project are not great and the error messages when a macro goes wrong are a wall.
- Dialyzer, the type checker, is slow and its output is a puzzle. I ran it anyway. It found two real bugs.

A smaller lesson. I put a debugging command into the editor launch config so anyone on the team could attach to a running node with one click. Nobody asked for it. Two people thanked me. The small tooling things are still the highest return per hour of anything I do.

I am not an Elixir person now. I am a person who can fix the Elixir when it breaks at eleven at night during the Seoul standup, which is what the team needed. Good enough.
