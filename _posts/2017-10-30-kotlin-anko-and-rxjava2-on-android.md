---
layout    : post
title     : "Kotlin, Anko, and RxJava2 on Android"
author    : Dennis Lim
date      : 2017-10-30 22:20:00 +0900
categories: computer science
---

I have been an iOS person on this project. Swift, storyboards, the whole thing. This fall I crossed over. Our Android app needed hands and I had them.

Kotlin was the easy part. If you know Swift, Kotlin feels like a cousin who grew up in a different country. Optionals are nullable types. Extensions exist. Data classes are structs that know how to print themselves. I was productive on day two. Java interop is the one thing that keeps biting. Every time I call into an old Java library, the nullability information disappears and I'm back to guessing.

Anko was the mistake. Not a big one, but a mistake.

Anko lets you write Android layouts in Kotlin code instead of XML. It looked great in the readme. Type safe, no findViewById, everything in one file. We built the flight list screen with it. Then we tried to change the design. Then we tried to have a designer look at it. Then we tried to use the layout preview in the IDE. None of that works well when your layout is code. Two weeks ago I converted the flight list back to XML. It took a day and I felt lighter after.

The lesson is not "Anko bad". The lesson is that a layout is something other people need to read, and the people who need to read it most are not engineers. XML is ugly but it is the shared language. Choosing a tool that only I can read was choosing to be the bottleneck.

RxJava2 was the right call and I want to be clear about that because I was skeptical.

A flight search screen is basically a pile of async things. User types, we debounce, we hit the API, results stream in, a filter changes, we recompute, the user scrolls, we load more. Doing that with callbacks is possible. I did it. It was a mess of flags and "isLoading" booleans. With Rx it becomes a chain. Text changes, debounce 300ms, switchMap to the search, combine with the filter subject, observe on main thread, render. When the filter changes, the chain reruns. When the user types again, the old search gets cancelled by switchMap for free. That "for free" is the whole value.

RxBinding on top gives you observables for the UI events. Click, text change, scroll. Small library, big difference.

Things that hurt:

- Rx stack traces are useless. When something throws inside a chain, the trace points at the Rx internals, not your code. You learn to put doOnError everywhere.
- Threading is explicit, which is good, until you forget one observeOn and update the UI from a background thread and the app dies with a message that does not mention Rx at all.
- The learning curve is real. The second engineer on the app took two weeks to stop fighting it.

Would I do it again? Kotlin yes, obviously. Rx yes, for anything with more than two async sources. Anko no. Some things should stay boring.

Back to iOS next month probably. It will be strange to miss Kotlin.
