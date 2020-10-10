---
layout    : posts
title     : "Shipping a React Native app in a Flutter shop"
author    : Dennis Lim
date      : 2020-10-10 22:40:00 +0900
categories: computer science
---

I am the Flutter guy. Three apps, forty thousand lines, a blog post about hot reload. And this fall I shipped a React Native app and I want to explain why without it sounding like an apology.

The situation: we needed a second mobile app, fast, for a product line that lives mostly on the web. The web team is React. They are good. They are not Dart people and they were not going to become Dart people in a quarter. The Flutter team was full. I could either wait for Flutter capacity in the spring or let the React team build it in React Native now.

I picked now. The thing I keep telling myself is that the best stack is the one the team that is going to maintain it already knows. I believe that. I still had to say it out loud a few times before it felt true.

What went well:

The web team was productive on day one. Same language, same mental model, a lot of shared code for API clients and models. Navigation with react-navigation was fine once we picked one pattern and stuck to it. Splash screen with a provider that loads initial data before the first real screen, boring and solid.

What went badly:

Native bridges. We needed to open a specific channel in a specific external app from Android, which means intents, which means writing Kotlin anyway. So the Flutter guy wrote Kotlin for the React Native app. There is a joke in there.

The dev experience surprised me. Fast refresh in React Native is good. It is not Flutter hot reload. State gets lost more often, and the moment you touch native code you are back to full rebuilds. I had gotten spoiled.

And the thing nobody talks about: two mobile stacks in one company means two sets of build tooling, two sets of CI quirks, two sets of "it works on my machine". I signed up for that cost. It is real. Every week there is one small thing that only one team knows how to do.

Would I do it again? Yes, with the same constraints. The app shipped in a quarter. Users do not know or care what it is written in. The team that built it can fix it at 2am without calling me.

Would I recommend two stacks as a general policy? No. This is a debt I took on knowingly and I have a plan to pay it down when either team has slack. The plan is written in a doc, because plans that live in my head do not survive the next quarter.

Being CTO turns out to mean making decisions I would have argued against as an engineer, and then explaining to the engineer version of myself why. I have that conversation about once a week. Most of the time the CTO wins. Not always.
