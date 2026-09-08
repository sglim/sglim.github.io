---
layout    : post
title     : "Six months as a blockchain tech lead"
author    : Dennis Lim
date      : 2018-08-20 23:30:00 +0900
categories: computer science
---

From March to now I led a small task force looking at blockchain. It is winding down, so here is the honest version.

The setup: it was 2018. Everyone's board was asking about blockchain. Ours was too. The company asked a few of us to figure out whether there was anything real in it for us, and if so, what. I was the tech lead. Three people, six months, no product requirement, which is both a gift and a trap.

What we actually did:

We learned Solidity. Properly, not blog post deep. Wrote contracts, deployed to testnets, broke them, read the post mortems of every famous hack, understood why reentrancy is a thing. If you come from normal backend work, the mental shift is that your code runs on a machine you do not own, costs money per instruction, and cannot be patched. Every bug is permanent. I have never reviewed code that carefully in my life.

We built a small dApp. A real one, with a contract on one side and a web front end talking to it through a wallet. Small in scope but complete. It worked. It was also slow, awkward for users, and cost real money to use in a way that no normal person would accept. That was the point of building it. You cannot learn that from a whitepaper.

We wrote an ICO proposal. This is the part I have mixed feelings about. We designed a token, wrote the economics, drew the diagrams, put together the whole thing the way a company would if it were going to do it. We did not do it. The company decided not to, and I think that was right. What I did not expect was that the proposal itself ended up mattering. I heard later that having a serious, technically grounded answer to "what about blockchain" helped in investor conversations. So the document did work, just not the work it was written for.

What I believe now, after six months:

- The technology is real and interesting. Consensus without trust is a genuine idea.
- Almost every application people proposed in 2018 does not need it. A database with an audit log solves most of them, faster and cheaper.
- The good use cases are narrow: things where the parties genuinely do not trust each other and there is no acceptable middle man. Those exist. They are rare.
- The gap between "works on a testnet" and "a normal person can use it" is enormous and mostly not a technology problem.

What I got out of it personally: I learned to say "we looked at it and the answer is no" without feeling like I failed. Six months of work that ends in a clear no is not wasted. It is the cheapest no the company will ever buy.

Back to Android next month. The travel app needs hands and honestly I miss shipping things people can download.
