---
layout    : posts
title     : "Learning Dart because Flutter hit 1.0"
author    : Dennis Lim
date      : 2019-01-20 22:15:00 +0900
categories: computer science
---

Flutter 1.0 came out in December. In January I'm on a new team building a new app with it. The timing is not a coincidence. We had a project starting, a consumer app that needed iOS and Android at the same time with a small team, and somebody said "what if we just try it". That somebody might have been me.

So now I am learning Dart. My last new language was Kotlin. Before that Swift. Before that I did not count because they were all C-shaped anyway.

Dart, first impressions after three weeks:

It is not exciting. That is a compliment. It reads like Java that went to therapy. Classes, interfaces, generics, async await, nothing weird. If you know two of Java, JavaScript, Kotlin, or Swift you will be writing real Dart in an afternoon. I was skeptical of a language that nobody uses outside of one framework, but the flip side is that the language and the framework were designed together, and it shows.

The thing that took adjustment is that everything is a widget and widgets are code. I wrote about hating Anko on Android because layouts in code were unreadable. Flutter is layouts in code and I do not hate it. I have been trying to figure out why. I think it is because in Flutter there is no other option, so all the tooling assumes it. The IDE shows you the widget tree. Hot reload shows you the result in under a second. With Anko you were fighting a platform that wanted XML. Here the whole platform wants code.

Hot reload is the feature. I keep saying this to people and they nod like it is a nice to have. It is not a nice to have. On native iOS, change a color, rebuild, wait, navigate back to the screen, look. Thirty seconds to a minute for every tiny thing. In Flutter, change the color, save, look. The screen is still where you left it. Multiply that by a few hundred times a day and it changes how you work. You stop batching changes and start playing.

Things that are rough:

- Packages. The ecosystem is young. For anything platform specific you either find a package written by one person six months ago or you write the native bridge yourself. We are writing a lot of bridges.
- Big lists. Performance is good but not free. You have to think about what rebuilds.
- Nobody on the team has done this before, including me. Every decision is a first.

The app we are building talks to a Kotlin backend and I still get to write some of that, which keeps me honest. But most of my day is Dart now, and I am starting to think this is going to be my main thing for a while.

I'll write a real review after we ship something. Right now it is a fun toy that might be a real tool. Ask me in six months.
