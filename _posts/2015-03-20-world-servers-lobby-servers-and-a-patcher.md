---
layout    : posts
title     : "World servers, lobby servers, and a patcher"
author    : Dennis Lim
date      : 2015-03-20 22:10:00 +0900
categories: computer science
---

One month into the new job. I'm a backend engineer at a game company now, working on an MMORPG that is not released yet.

Before this I did database visualizer stuff at a big enterprise software company. Java, Eclipse RCP, a lot of meetings about the meetings. The code I wrote there was fine but nobody was waiting for it. Here it is different. If the world server goes down, three thousand people in that world get kicked out at the same time. I have not broken it yet. Give me time.

Some things I learned in the first month.

A game server is not one server. There is a lobby server that handles login and character selection. There are world servers, one per shard, that actually run the game. There are a bunch of small services behind them for chat, auction, guilds. When people say "the server is lagging" they mean one of maybe fifteen processes and you have to figure out which one.

The engine is CryEngine, and the server side is C++. I thought I knew C++. I did not know C++ the way these people know C++. There is a lot of custom memory management and a lot of code that exists because somebody measured something two years ago and found it was slow. I am learning to ask "why is it like this" before touching it, because usually there is a reason, and usually the reason is a bug report from a stress test.

The other thing I own is the patcher. Nobody wanted it. It is the program that runs before the game starts and downloads updates. Boring, right? Except a full client is many gigabytes and we push builds constantly, so if the patcher is dumb, every player downloads gigabytes every week. I'm looking at diff based patching now. Only ship the bytes that changed. It sounds obvious. It is not obvious to implement when the files are packed archives and one small change shifts everything after it.

Python is everywhere here too, for tools and build scripts. That part feels like home.

What surprised me most is the stress test culture. Every few weeks we fill a server with bots and watch it. Not a unit test, not a benchmark, an actual server with actual fake players walking around and casting spells. Then we look at the graphs and somebody says "that spike is yours" and you go fix it. It is brutal and it is honest. I like it.

I don't know yet if I want to stay in games. The hours are long and the launch is coming. But I know one thing already. After you have kept three thousand concurrent players alive on one process, a normal web server will feel like a toy.
