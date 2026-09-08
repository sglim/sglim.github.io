---
layout    : post
title     : "Why is Shanghai missing"
author    : Dennis Lim
date      : 2016-11-20 23:55:00 +0900
categories: computer science
---

A user wrote in. Short message. "I searched for Shanghai and there is nothing. Is your app broken?"

The app was not broken. The app was doing exactly what we told it to do. That is worse.

Here is the setup. We show flight fares from Seoul to a lot of cities. Behind that is a pipeline. A fare updater pulls prices from providers on a schedule, a discovery service decides which routes are interesting, and the app shows what discovery says. Somewhere in there we have a blacklist and a whitelist. Blacklist for carriers we don't want to show, whitelist for routes we do.

Shanghai has two airports. Pudong and Hongqiao. Most people fly into Pudong. The whitelist had Pudong. Fine. Then a provider changed how they returned the city code for Shanghai. Instead of the airport code they started returning the metropolitan area code, which covers both airports. Our whitelist did not have the metro code. The filter did its job. Shanghai vanished. No error, no log, no alert. Just a city quietly not existing anymore.

It took me most of a day to find. Not because it was hard once you looked, but because nothing pointed there. The server was healthy. The fare updater ran. The discovery service returned results. It just returned results without Shanghai and nobody had written a check for "hey, a major city has zero fares, that's weird".

Things I changed:

- Added a sanity check to the pipeline. If any top twenty destination has zero fares after an update, it fails loudly. It would have caught this in an hour instead of a week.
- Normalized city codes at the edge. Every provider response gets mapped to our own city id before it touches any filter. The whitelist now speaks our language, not theirs.
- Wrote a test for the segment filter. Should have existed. Didn't.

Thing I did not change but keep thinking about: a filter that silently removes data is a landmine. Every whitelist is a promise that you will maintain it forever. We had three people and a whitelist with a few hundred entries. That math was never going to work.

The user got a reply and a coupon. Shanghai is back.

What stays with me is the shape of the bug. It was not a crash. It was an absence. Systems are good at telling you when something is wrong. They are terrible at telling you when something is missing. You have to build that yourself, and you only think to build it after the first time a city disappears.

Next time a user says "is your app broken", I'm going to assume the answer is yes until I can prove it is not.
