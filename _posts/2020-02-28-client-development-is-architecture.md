---
layout    : post
title     : "Client development is architecture"
author    : Dennis Lim
date      : 2020-02-28 11:08:00 +0900
categories: computer science
---

Someone said to me this week, half joking, that client work is "just putting buttons on a screen". I laughed. Then I went home and it bothered me, so here is the longer answer.

Client development is like building a building. Anyone can stack bricks. If you stack bricks without knowing anything you get a square brick box. To build something people actually want to be inside, you need geometry, you need structural mechanics, and, this is the part engineers hate hearing, you need some sense of art. The person who dismisses client work looks at Gaudi and says "that's just bricks and tiles, right?"

Let me make it concrete, because "art" is a cop out if I stop there.

A screen has state. Loading, loaded, empty, error, partially loaded, loaded but stale, loaded but the user scrolled while it was loading. A backend endpoint has maybe three states. A single client screen easily has ten, and they combine. The engineering problem is not drawing the button. It is making sure the button is correct in all ten states and that the transitions between them do not flicker or lose the user's place. That is state machine design. That is architecture.

A screen has time. The network is slow, the user is fast. What happens if they tap twice. What happens if they navigate away mid request and the response arrives later. What if they rotate the phone. Backend engineers think about concurrency in terms of threads. Client engineers think about it in terms of a human with a thumb who does not care about your request lifecycle.

A screen has memory that is not yours. The OS will kill your app whenever it feels like it. When the user comes back, they expect to be where they were. So you serialize state, you restore it, you handle the case where the thing they were looking at no longer exists. Servers get to assume they are running. Clients do not.

And yes, a screen has feel. Sixteen milliseconds per frame. If your list stutters, the app is bad, and no amount of correct data fixes that. Animation curves, touch feedback, the way a sheet dismisses. This is where the art comes in, and it is not decoration. It is the difference between an app people use and an app people tolerate.

I have done a lot of both sides now. Game servers, flight search backends, IoT gateways. And iOS, Android, three Flutter apps. The backend work was harder in some ways. Scale, consistency, failure. But I never had a backend where I had to make a thousand tiny decisions that the user would feel but never name.

So, respectfully, to whoever said "just buttons". Come sit with me for a week. Bring a feeler gauge. We are going to level the bed.
