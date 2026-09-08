---
layout: post
title: About
permalink: /about/
---

I am the CTO of [Unitblack](#unitblack), where I rebuilt the [scraping engine](#unitblack) that reads the tax authority for tens of thousands of businesses, the engineer behind a [spreadsheet formula engine](#formulas) that accountants trust, the author of an [AI harness](#ai) a whole company works through, a [video format you can touch](#momenti), a [smart plug that actually shipped](#hardware), a [flight search and booking app](#kyte) that ate tens of gigabytes of airline data a day, an [MMO server](#games) that held three thousand players at once, a [query plan visualizer](#sap) inside SAP HANA, a [two-time blockchain skeptic](#blockchain), a [former CTO](#squarelab) of a travel company, a [convert to AI-assisted engineering](#ai), a [three-year New Yorker](#newyork) now back in Seoul, and someone who has [tracked every working hour](#elsewhere) since 2017.

I write here, in English, about what I built and what it taught me. GitHub is [sglim](https://github.com/sglim).

## <a name="unitblack"></a>CTO of Unitblack

Unitblack builds tax and accounting software for Korean small businesses: a service that finds the income tax refunds people are owed, a payroll product, tax filing, and the back offices behind them. I joined in 2025, first as a contractor on a [trial run]({% post_url 2025-06-10-tax-software-and-a-fresh-start %}), then as CTO. The domain was completely new to me. That was the point.

What I have actually touched there, in rough order of how much of my life it took:

**The scraping engine.** Everything starts with pulling a business's records out of the [national tax service]({% post_url 2025-12-22-reverse-engineering-a-government-login %}) and a handful of other institutions that were never designed to be read by software. The first version leaned on a vendor's black-box binary. I led the rewrite to a pure TypeScript engine that reproduces the institutions' own browser clients line for line, runs headless on Lambda, and is verified against the original on every service. Then the parts nobody had a client for: a credit-card clearing house behind a WAF that rotates its cipher every few weeks, delivery-app portals, a government certificate flow, and a mock server seeded from tens of thousands of real records so the whole pipeline can be tested end to end without touching production data. Two thousand commits in that repo alone.

**The refund product.** Hidden Money is the flagship: a user logs in once, we pull five years of filings, recompute what they should have paid, and file the correction. I worked across the API, the admin, the batch workers, the shared data models, and the scraper, and I rebuilt the CI/CD for all of them: multi-service workflows, image tags you can read, a deploy repo per environment, a move from webpack to esbuild, tracing through Jaeger, and a payment integration with two Korean PSPs.

**The calculation layer.** A Rust [spreadsheet formula engine](#formulas) so the accountants' workbooks are the source of truth, an income tax calculator that runs as a single Lambda every other service calls, and a filer that turns the result into the actual return. The calculator repo is written so that a tax specialist, not an engineer, can change a deduction limit by asking an agent in plain Korean.

**Payroll.** A payroll system with employee records, allowances and deductions, withholding tax e-filing, and the four social insurances, integrated with the incumbent accounting platform and then made independent of it. National ID numbers stored only as AES-256-GCM ciphertext. Four thousand commits in seven weeks, most of them by agents, every one of them reviewed.

**The platform.** Kubernetes on EKS, ArgoCD, external secrets, an internal Forgejo, Fluent Bit and Elasticsearch with APM, IAM with enforced MFA and IRSA, RDS access through IAM roles, a self-hosted Vaultwarden for company passwords, and LiteLLM in front of every model call so we know what the agents cost. Plus the smaller services around the edges: an alert-message monitor, a bank-appointment back office with card payments, an SEO blog with a scheduling admin, a GA4 dashboard, a knowledge base built from the company's own Slack archive, an ops agent that runs the refund workflow on Airtable.

**The harness.** See below.

## <a name="formulas"></a>A spreadsheet formula engine

Accountants think in spreadsheets. Instead of translating their workbooks into code and watching the two drift apart every tax year, I built an engine in Rust that [reads the workbook and computes the same answers]({% post_url 2026-01-25-reimplementing-spreadsheet-formulas %}) the spreadsheet would. Spill ranges, error propagation, the long tail of functions decided in 1995. A regression suite of real sheets is the [only reason it works]({% post_url 2026-03-15-spill-ranges-and-value-errors %}).

## <a name="ai"></a>A convert to AI-assisted engineering

I came back from New York a skeptic and changed my mind within a quarter. Most of Unitblack's code is now written by agents, at a pace of [thousands of small commits a month]({% post_url 2026-06-25-two-thousand-commits-in-two-months %}). All of it is reviewed by a human who has to be able to explain it, and that human is usually me. The bottleneck moved from typing to understanding, and it has already [bitten me once]({% post_url 2026-08-10-dont-log-the-whole-cookie %}) in an instructive way.

So I built the harness the company runs on. A shared repository, pulled into every project as a submodule, that carries the rules an agent has to follow before it touches code: how I judge a change, a governance tier per project for risk, data sensitivity, and write scope, a steward who owns each system's coherence, a security review gate, a secret scanner that actually runs, scaffolds for new services, and slash commands that keep every team's harness in sync with the org chart. The goal is that anyone at the company, developer or not, gets the same quality out of an agent that I would. A tax specialist changes tax law constants in plain Korean. An operator runs a refund workflow. The engineers review.

This is not my first time around AI. At Skelter Labs I worked on Iris, a hyper-personalization engine built on the company's machine learning research, and then led Meerkat, a taste-based social service built on top of Iris to put that personalization in front of real people. It was the last thing I did at Skelter and the most product-shaped. Flutter on the front, TypeScript on the back, a lot of arguing about what "personal" should feel like.

## <a name="momenti"></a>A video format you can touch

At Momenti I was Technical Architect for an interactive media format. Video that rotates when you drag it and responds when you tap it, on any phone browser, with no plugin. I wrote the format's design docs and kept the [protobuf contract]({% post_url 2021-11-25-protobuf-as-the-contract-between-three-teams %}) between the maker, the player, the admin, and the Rust engine honest. On the engine side I worked in the maker's editors and led the work that took the player engine to browsers [through WebAssembly]({% post_url 2022-09-25-shipping-a-rust-engine-to-the-browser-with-wasm %}) and to phones through FFI. On the product side I was the top contributor to the web player, from the scene renderer and streaming to the overlay, and to the Flutter repository, where I led Spark, a social app built around the format, and Motiv, a photo-card product for venues, and built the offline player and a flow analyzer for how people move through a clip. I ran the [RFC process]({% post_url 2023-05-10-writing-my-first-rfc %}), proposed [pulling motion vectors out of the codec]({% post_url 2023-07-30-motion-vectors-are-already-in-the-file %}) instead of computing them twice, and prototyped an [NFT marketplace on Solana](#blockchain) when the company needed to know.

## <a name="hardware"></a>A smart plug that actually shipped

In 2019 I led the IoT team at Skelter Labs that built Brilli, a smart power socket. In boxes, in living rooms, not a demo. [MQTT and RabbitMQ]({% post_url 2019-03-20-smart-sockets-and-two-ender-3s %}) on the back, a Kotlin device server, BLE on the phone, and firmware updates designed to never brick anything. The two 3D printers we bought for prototype enclosures became a [hobby I still have]({% post_url 2019-07-10-the-ender-5-arrived %}).

## <a name="kyte"></a>A flight search app with a lot of data behind it

I was engineer number five at Skelter Labs, a startup founded by ex-Googlers, and for two years the tech lead of Kyte, a flight search and booking app that passed two hundred thousand downloads. I wrote the iOS app in Swift and later a good part of the Android app in Kotlin, but most of my commits are on the server side: the search and recommendation service on Elasticsearch, the integration with the airline distribution system that turned a search result into an actual ticket, the price-drop watch that told you when the fare for your route fell, the REST gateway in front of the gRPC services, and the booking and PNR protocols underneath. Behind all of it was a fare pipeline in Spark that pulled tens of gigabytes of airline data every day. I ran the deployments on ECS, set up the ELK stack, and [watched the GeoIP dots appear]({% post_url 2016-07-25-logstash-geoip-and-the-first-server-i-owned %}). I learned that the worst bugs in a data product [are absences, not crashes]({% post_url 2016-11-20-why-is-shanghai-missing %}).

I led Kyte until it spun out of Skelter Labs into its own company in early 2018, then stayed at Skelter for the blockchain task force, the IoT team, and Meerkat, in that order. Four years total, [written up here]({% post_url 2020-04-03-leaving-skelter-labs-after-four-years %}).

## <a name="squarelab"></a>Former CTO of a travel company

Kyte spun out of Skelter Labs into Squarelab in 2018. Two years later, in 2020, I followed it there and became [CTO]({% post_url 2020-06-15-cto-of-a-spinoff %}) of twenty plus engineers and two products, with the travel industry frozen by a pandemic. In that year we built [hotel search on top of flight search]({% post_url 2020-12-20-hotels-are-harder-than-flights %}): a master-hotel service that matched the same building across a dozen suppliers, a room-grouping engine with hand-tuned tagging rules, a coupon service, and a booking service on the national airline distribution platform. We rebuilt Kyte as a web app in React and prototyped its successor in Flutter and [React Native]({% post_url 2020-10-10-shipping-a-react-native-app-in-a-flutter-shop %}), shipped a white-label version for a partner, and kept the deals app and its real-time gateway alive. I also did the unglamorous half: a monorepo restructuring into client and server workspaces, a protobuf toolchain shared by Kotlin, Node, and TypeScript, Kubernetes on EKS, logging, and [upgrades nobody asked for]({% post_url 2021-02-20-node-10-to-12-and-other-unglamorous-upgrades %}). I [still wrote YAML]({% post_url 2021-04-25-a-cto-who-still-writes-yaml %}). I think that was right.

## <a name="games"></a>An MMO server

At XL Games I worked on Civilization Online, launched with 2K in 2015. [World and lobby servers]({% post_url 2015-03-20-world-servers-lobby-servers-and-a-patcher %}) in C++ holding three thousand players each, a diff-based patcher, and a game database whose migrations I convinced a game studio to [run through Django]({% post_url 2015-09-15-migrating-a-game-database-with-django %}). [Launch day]({% post_url 2015-12-20-launch-day %}) was the first time I shipped something people were waiting outside the door for.

## <a name="sap"></a>Three years inside a database

My first real job after Google was at SAP Labs Korea, from 2012 to the middle of 2014. I worked on SAP HANA, the in-memory database, and specifically on PlanViz, the tool that draws what the query optimizer decided to do with your SQL. To draw a plan well you have to understand it, so I spent a lot of time in the optimizer itself, tuned parts of it, and raised the unit test coverage on the way. Java, C++, Python, Eclipse RCP. It taught me that the fastest way to understand a system is to build the smallest real thing that touches it, which is a habit I [still keep as a CTO]({% post_url 2021-04-25-a-cto-who-still-writes-yaml %}).

After SAP I spent half a year trying to start an IoT company. Mesh networks, Zigbee, iBeacon firmware, a lot of soldering, some freelance work to pay for it. It did not work out. I joined XL Games in early 2015 with a drawer full of dev boards and a much better idea of what shipping hardware actually costs, which came in handy [five years later]({% post_url 2019-03-20-smart-sockets-and-two-ender-3s %}).

## <a name="blockchain"></a>A two-time blockchain skeptic

Twice a company asked me to find out whether blockchain was real for them. In 2018 I led a [task force]({% post_url 2018-08-20-six-months-as-a-blockchain-tech-lead %}) that ended in a Solidity dApp and a well-argued "not now". In 2022 I built an [NFT marketplace with auctions on Solana]({% post_url 2022-11-15-a-solana-nft-marketplace-prototype-in-rust %}), in Rust on Metaplex, that ended the same way. Both times the company got a real answer instead of a guess. I count both as wins.

## <a name="newyork"></a>Three years in New York

I [moved to Manhattan in 2022]({% post_url 2022-07-28-moving-to-new-york %}) while at Momenti, working [Seoul's hours from New York]({% post_url 2022-12-20-first-winter-in-nyc-working-korea-hours %}). When the funding stopped I spent [three months not working]({% post_url 2024-01-25-three-months-of-not-working %}) and then joined Koodos Labs, a consumer startup whose product is a shelf of everything you have read, watched, listened to, and played. I owned the data side: the syncers that pull history from a streaming service, a bookshelf site, two music services, a game store, and a game database, each of which fights you in its own way; a second generation of them that shards users across workers and survives rate limits; a data-model migration verified against golden sets; the backfill scripts that repaired months of history; and the logging that let a dashboard show which shard was stuck at six in the morning. [Smoke tests, backfills]({% post_url 2024-05-25-smoke-tests-and-backfilling-scripts %}), resumable cursors, and a [hard-won respect]({% post_url 2024-07-30-syncing-goodreads-again %}) for other people's rate limits. [Two and a half years in]({% post_url 2024-11-20-two-and-a-half-years-in-new-york %}) I wrote down what the city taught me. In 2025 I [came home]({% post_url 2025-03-15-coming-back-to-korea %}).

## <a name="elsewhere"></a>Elsewhere

Yonsei University, computer science, top of the department. ACM-ICPC regional bronze. An internship at Google where my code shipped to production, a Django shop for an art gallery on the side, and two years in the Air Force building the branch-wide emergency contact system. The Chief of Staff, the highest-ranking officer in the Air Force, gave me an award for it and sat me down to lunch with about twenty other people. I was a conscript. I still think about that lunch.

I have [tracked every working hour since 2017]({% post_url 2021-06-23-five-thousand-entries-later %}). The number is about two thousand a year no matter the job. A Mac mini on a shelf [runs my life]({% post_url 2026-05-20-a-headless-mac-launchd-and-a-stream-deck %}) through launchd and a Stream Deck daemon. A 3D printer prints parts for the 3D printer.
