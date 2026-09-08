---
layout    : post
title     : "Hotels are harder than flights"
author    : Dennis Lim
date      : 2020-12-20 23:30:00 +0900
categories: computer science
---

We added hotels to the flight app this year. I assumed, after four years of airline data, that hotel data would be the easy version. I was wrong in a way that is interesting enough to write down.

Flights are hard because the data is old and the systems are slow. But a flight is a well defined thing. Origin, destination, date, carrier, flight number. Two providers describing the same flight will disagree on price and availability, but they agree on what the flight is. There is a global identity.

Hotels have no such thing.

Ten providers, ten different names for the same building. "Grand Hotel Seoul", "Seoul Grand", "The Grand (Seoul)", one with the address slightly wrong, one with the old name from before the renovation. There is no flight number. There is a building and a lot of opinions about what to call it.

So the first thing you build is not search. It is matching. Take every hotel from every provider, figure out which ones are the same physical place, and create your own master record. We call it a master hotel. It has a name we chose, a location we verified, and a list of provider ids that map to it. Every search result goes through that mapping before a user sees it.

Matching is a rabbit hole. Name similarity plus distance works for 80 percent. The last 20 percent is chains with three properties on the same street, hotels that moved, hotels with the same name in different cities, and providers that put the parking lot coordinates instead of the entrance. We have a manual review queue. A person looks at pairs and clicks yes or no. It is the most important unglamorous job in the company right now.

Then rooms. You would think a room is a room. A provider says "Deluxe Double". Another says "Double Deluxe Room". Another says "3 Single Beds" for what is functionally a triple. We wrote a tagger that normalizes room descriptions into a small set of types, and I personally argued for twenty minutes about whether three single beds should be tagged as a triple. It should. It is.

And Korean names. Users search in Korean. Providers give English names. So every master hotel needs a Korean name, and for the ones nobody has typed yet, that means someone typing them. We show the Korean name in the admin room table now so the ops team can see what is missing. Small feature, big difference to the people doing the work.

What I took from this year: the hard part of a data product is almost never the query. It is the identity. Deciding what counts as the same thing, and building the machinery to keep that decision consistent when ten sources disagree. Flights gave us that for free. Hotels made us build it, and building it is most of what we did.

Travel is still frozen. We built the hotel engine anyway. When people start moving again the matching will be done. That is the bet.
