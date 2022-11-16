---
layout    : posts
title     : "A Solana NFT marketplace prototype in Rust"
author    : Dennis Lim
date      : 2022-11-15 22:40:00 -0500
categories: computer science
---

This fall I built an NFT marketplace with auctions on Solana. It is a prototype. It is also, I think, the most complete thing I have built in Rust so far, and given the timing, it is worth writing about honestly.

Why the company cared: our format is a media format. Media that people make and own. In 2022 the question "should this be an NFT" was being asked in every media company, and asking it seriously means building enough to know what the answer costs. I was the blockchain person once, in 2018. I got the ticket.

Why Solana: speed and fees. The Ethereum side of things made a marketplace with real auctions painful for normal users. Solana's transaction costs are small enough that bidding does not feel like a purchase in itself. Also the programs are in Rust, which meant I could reuse a year of engine work in my head.

What I built:

Minting. Our media object becomes a token with metadata pointing at the asset. I used Metaplex for the token standard rather than inventing one. This is the part I want to be clear about. Metaplex did most of the heavy lifting on the standard, the metadata, the wallet integration. What I wrote was the marketplace logic on top and the glue to our format.

Listing and auction. A program that holds the token in escrow, accepts bids above the current one, refunds the previous bidder, and settles at the deadline. Simple to describe. Every line is money, so every line was reviewed twice, and I wrote more tests for it than for anything since the blockchain task force.

A small web front end to actually use it. Connect wallet, see listings, bid, watch the countdown. Enough to demo.

What I learned:

Solana's account model is different from the EVM in a way that took me a week to internalize. Programs are stateless. State lives in accounts you pass in. Once it clicked I liked it. Until it clicked I wrote a lot of code that did not compile for reasons the compiler explained very patiently.

Rust on chain is Rust with the fun parts removed. No std, tight compute budgets, every allocation counted. Good discipline. Not something I want to do every day.

Building on a standard is the right call even when the standard is young. Metaplex changed under me twice in three months. Still better than owning it.

What happened to it: it is a prototype. It works on devnet. The company decided, correctly I think, that the market was not there for us right then, and the timing in late 2022 turned out to make that decision look very good. The code is in a repo, documented, and if the question comes back we will not start from zero.

Two blockchain projects, four years apart, both ending in "not now". I am fine with that. Both times I learned the technology properly, both times the company got a real answer instead of a guess, and this time I got a Rust codebase I am not embarrassed by. That is a good trade for a fall.
