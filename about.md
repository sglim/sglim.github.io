---
layout: post
title: About
permalink: /about/
---

I am the CTO of [Unitblack](#unitblack), the engineer behind a [spreadsheet formula engine](#formulas) that accountants trust, a [video format you can touch](#momenti), a [smart plug that actually shipped](#hardware), a [flight search app](#kyte) that ate tens of gigabytes of airline data a day, an [MMO server](#games) that held three thousand players at once, a [query plan visualizer](#sap) inside SAP HANA, a [two-time blockchain skeptic](#blockchain), a [former CTO](#squarelab) of a travel company, a [convert to AI-assisted engineering](#ai), a [three-year New Yorker](#newyork) now back in Seoul, and someone who has [tracked every working hour](#elsewhere) since 2017.

I write here, in English, about what I built and what it taught me. GitHub is [sglim](https://github.com/sglim).

## <a name="unitblack"></a>CTO of Unitblack

Unitblack builds tax and accounting software for Korean small businesses. I joined in 2025, first as a contractor on a [trial run]({% post_url 2025-06-10-tax-software-and-a-fresh-start %}), then as CTO. The work is a platform team's worth of Kubernetes, ArgoCD, and secrets management, an [integration with the national tax service]({% post_url 2025-12-22-reverse-engineering-a-government-login %}) that has to be right to the won, and the formula engine below. The domain was completely new to me. That was the point.

## <a name="formulas"></a>A spreadsheet formula engine

Accountants think in spreadsheets. Instead of translating their workbooks into code and watching the two drift apart every tax year, I built an engine in Rust that [reads the workbook and computes the same answers]({% post_url 2026-01-25-reimplementing-spreadsheet-formulas %}) the spreadsheet would. Spill ranges, error propagation, the long tail of functions decided in 1995. A regression suite of real sheets is the [only reason it works]({% post_url 2026-03-15-spill-ranges-and-value-errors %}).

## <a name="ai"></a>A convert to AI-assisted engineering

I came back from New York a skeptic and changed my mind within a quarter. Most of our code is now written by agents, at a pace of [thousands of small commits a month]({% post_url 2026-06-25-two-thousand-commits-in-two-months %}). All of it is reviewed by a human who has to be able to explain it, and that human is usually me. The bottleneck moved from typing to understanding. I think that is the biggest change to this job since code review itself, and it has already [bitten me once]({% post_url 2026-08-10-dont-log-the-whole-cookie %}) in an instructive way.

This is not my first time around AI. At Skelter Labs I worked on Iris, a hyper-personalization engine built on the company's machine learning research, and then led Meerkat, a taste-based social service built on top of Iris to put that personalization in front of real people. It was the last thing I did at Skelter and the most product-shaped. Flutter on the front, TypeScript on the back, a lot of arguing about what "personal" should feel like.

## <a name="momenti"></a>A video format you can touch

At Momenti I was Technical Architect for an interactive media format. Video that rotates when you drag it and responds when you tap it, on any phone browser, with no plugin. I wrote the format's design docs, shaped the player architecture, and led the work that took the Rust engine to browsers [through WebAssembly]({% post_url 2022-09-25-shipping-a-rust-engine-to-the-browser-with-wasm %}) and to phones through FFI. Top contributor on the web and mobile repositories. Ran the [RFC process]({% post_url 2023-05-10-writing-my-first-rfc %}), kept the [protobuf contract]({% post_url 2021-11-25-protobuf-as-the-contract-between-three-teams %}) between four codebases honest, and proposed [pulling motion vectors out of the codec]({% post_url 2023-07-30-motion-vectors-are-already-in-the-file %}) instead of computing them twice.

## <a name="hardware"></a>A smart plug that actually shipped

In 2019 I led the IoT team at Skelter Labs that built Brilli, a smart power socket. In boxes, in living rooms, not a demo. [MQTT and RabbitMQ]({% post_url 2018-06-15-smart-sockets-and-two-ender-3s %}) on the back, a Kotlin device server, BLE on the phone, and firmware updates designed to never brick anything. The two 3D printers we bought for prototype enclosures became a [hobby I still have]({% post_url 2019-07-10-the-ender-5-arrived %}).

## <a name="kyte"></a>A flight search app with a lot of data behind it

I was engineer number five at Skelter Labs, a startup founded by ex-Googlers, and tech lead of Kyte, a flight search app that passed two hundred thousand downloads. Node.js servers, [Swift and Kotlin clients]({% post_url 2017-10-30-kotlin-anko-and-rxjava2-on-android %}), and a Spark pipeline that pulled tens of gigabytes of airline fares every day and served the interesting parts to a phone in milliseconds. I set up the ELK stack and [watched the GeoIP dots appear]({% post_url 2016-07-25-logstash-geoip-and-the-first-server-i-owned %}), and learned that the worst bugs in a data product [are absences, not crashes]({% post_url 2016-11-20-why-is-shanghai-missing %}). I led Kyte until it spun out of Skelter Labs into its own company in early 2018, then stayed at Skelter for the blockchain task force, the IoT team, and Meerkat, in that order. Four years total, [written up here]({% post_url 2020-04-03-leaving-skelter-labs-after-four-years %}).

## <a name="squarelab"></a>Former CTO of a travel company

Kyte spun out of Skelter Labs into Squarelab in 2018. Two years later, in 2020, I followed it there and became [CTO]({% post_url 2020-06-15-cto-of-a-spinoff %}). Twenty plus engineers, two products, and the travel industry frozen by a pandemic. We built [hotel search on top of flight search]({% post_url 2020-12-20-hotels-are-harder-than-flights %}), a push system for deals, web versions of both apps, and a Kubernetes platform. I [still wrote YAML]({% post_url 2021-04-25-a-cto-who-still-writes-yaml %}). I think that was right.

## <a name="games"></a>An MMO server

At XL Games I worked on Civilization Online, launched with 2K in 2015. [World and lobby servers]({% post_url 2015-03-20-world-servers-lobby-servers-and-a-patcher %}) in C++ holding three thousand players each, a diff-based patcher, and a game database whose migrations I convinced a game studio to [run through Django]({% post_url 2015-09-15-migrating-a-game-database-with-django %}). [Launch day]({% post_url 2015-12-20-launch-day %}) was the first time I shipped something people were waiting outside the door for.

## <a name="sap"></a>Three years inside a database

My first real job after Google was at SAP Labs Korea, from 2012 until I left for XL Games in early 2015. I worked on SAP HANA, the in-memory database, and specifically on PlanViz, the tool that draws what the query optimizer decided to do with your SQL. To draw a plan well you have to understand it, so I spent a lot of time in the optimizer itself, tuned parts of it, and raised the unit test coverage on the way. Java, C++, Python, Eclipse RCP. It taught me that the fastest way to understand a system is to build the smallest real thing that touches it, which is a habit I [still keep as a CTO]({% post_url 2021-04-25-a-cto-who-still-writes-yaml %}).

## <a name="blockchain"></a>A two-time blockchain skeptic

Twice a company asked me to find out whether blockchain was real for them. In 2018 I led a [task force]({% post_url 2018-08-20-six-months-as-a-blockchain-tech-lead %}) that ended in a Solidity dApp and a well-argued "not now". In 2022 I built an [NFT marketplace with auctions on Solana]({% post_url 2022-11-15-a-solana-nft-marketplace-prototype-in-rust %}), in Rust on Metaplex, that ended the same way. Both times the company got a real answer instead of a guess. I count both as wins.

## <a name="newyork"></a>Three years in New York

I [moved to Manhattan in 2022]({% post_url 2022-07-28-moving-to-new-york %}) while at Momenti, working [Seoul's hours from New York]({% post_url 2022-12-20-first-winter-in-nyc-working-korea-hours %}). When the funding stopped I spent [three months not working]({% post_url 2024-01-25-three-months-of-not-working %}) and then joined Koodos Labs, where I built and rebuilt the syncers that pull in what people [read, watch, and play]({% post_url 2024-07-30-syncing-goodreads-again %}) from services that were not designed to share. Smoke tests, backfills, resumable cursors, and a hard-won respect for other people's rate limits. [Two and a half years in]({% post_url 2024-11-20-two-and-a-half-years-in-new-york %}) I wrote down what the city taught me. In 2025 I [came home]({% post_url 2025-03-15-coming-back-to-korea %}).

## <a name="elsewhere"></a>Elsewhere

Yonsei University, computer science, top of the department. ACM-ICPC regional bronze. An internship at Google where my code shipped to production, freelance firmware for iBeacons and a Django shop for an art gallery, and two years in the Air Force building the branch-wide emergency contact system. The Chief of Staff, the highest-ranking officer in the Air Force, gave me an award for it and sat me down to lunch with about twenty other people. I was a conscript. I still think about that lunch.

I have [tracked every working hour since 2017]({% post_url 2021-06-23-five-thousand-entries-later %}). The number is about two thousand a year no matter the job. A Mac mini on a shelf [runs my life]({% post_url 2026-05-20-a-headless-mac-launchd-and-a-stream-deck %}) through launchd and a Stream Deck daemon. A 3D printer prints parts for the 3D printer.
