---
layout    : posts
title     : "Three months of only Android"
author    : Dennis Lim
date      : 2018-11-20 22:40:00 +0900
categories: computer science
---

Since October I have been doing one thing. Android. Only Android. For the travel app that I used to lead and then left for IoT and blockchain and now came back to as a regular engineer on the mobile team.

It is a good change. Let me explain why being "just" an Android engineer for a while is exactly what I needed.

For the last year my job was mostly deciding. Which protocol for the socket. Whether the blockchain thing had legs. Who works on what. Deciding is tiring in a way that writing code is not. You end the day with nothing to show except a doc and a lot of Slack threads. I missed the feeling of a screen working at 6pm that did not work at 9am.

So now I have that. Kotlin all day. The app has grown a lot since I last touched the Android side. The Rx setup I put in last year survived, which was nice to see. Some of my old decisions did not survive, which was also fine. Somebody replaced my clumsy filter code with something cleaner and I learned from reading it.

Things I worked on this quarter, roughly:

- Dynamic segments in the search. Users wanted to add and remove legs of a trip on the fly instead of picking "one way" or "round trip" up front. Sounds small. Touches everything from the search form to the results parser to the analytics.
- Booking history. Making the drawer and the list not fall over when there are a lot of bookings, and handling the annoying case where the user logs out while looking at it.
- Environment switching. QA, release candidate, production, with the right endpoints and the right keys, without anyone building a special APK by hand. The number of bugs that were actually "wrong environment" was embarrassing.
- Set up a separate Gerrit instance for the mobile project so the review flow could move at the mobile team's pace instead of the monorepo's.

None of this is glamorous. All of it is the difference between an app people keep and an app people delete.

The thing I keep noticing is how much easier it is to be a good engineer when your scope is small. I review every mobile change. I know every screen. When a crash report comes in I usually know which file before I open it. That is only possible because I am not also thinking about firmware and token economics.

I know this is temporary. The company is growing and at some point someone will ask me to lead something again. I'll say yes, probably. But I'm going to remember this quarter as the one where I got to just build, and I'm going to protect time like this in whatever comes next.

Also, the app passed a download count that made the whole team go out for dinner. I won't say the number. It has more zeros than I expected when I set up the first server in 2016.
