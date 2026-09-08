---
layout: post
title: About
permalink: /about/
---

I am the CTO of [Unitblack](#unitblack), where I rebuilt the [scraping engine](#unitblack) that reads the tax authority for tens of thousands of businesses. Before that: a [spreadsheet formula engine](#formulas) in Rust, an [AI harness](#ai) a whole company works through, a [video format you can touch](#momenti), a [smart plug that shipped](#hardware), a [flight search and booking app](#kyte) that ate tens of gigabytes of airline data a day, an [MMO server](#games) holding three thousand players, a [query plan visualizer](#sap) inside SAP HANA, [two blockchain projects](#blockchain) that ended in "not now", a year as [CTO of a travel company](#squarelab), [three years in New York](#newyork), and [every working hour tracked](#elsewhere) since 2017.

I write here, in English, about what I built and what it taught me. GitHub: [sglim](https://github.com/sglim).

## <a name="unitblack"></a>CTO of Unitblack

Tax and accounting software for Korean small businesses. Joined 2025 on a [trial run]({% post_url 2025-06-10-tax-software-and-a-fresh-start %}), then CTO.

- **Scraping engine.** Replaced a vendor black box with a pure TypeScript port of the [tax authority's]({% post_url 2025-12-22-reverse-engineering-a-government-login %}) own browser client, line for line, headless on Lambda, verified against the original per service. Then a card clearing house behind a cipher-rotating WAF, delivery-app portals, a certificate flow, and a mock server seeded from tens of thousands of real records for end-to-end tests. Two thousand commits in that repo alone.
- **Hidden Money.** The refund product: five years of filings pulled, recomputed, corrections filed. API, admin, batch, shared models, scraper, and the CI/CD for all of them. Jaeger. Two Korean PSPs.
- **Calculation.** The [Rust formula engine](#formulas), a single-Lambda income tax calculator, a filer. A tax specialist changes a deduction limit by asking an agent in Korean.
- **Payroll.** Withholding e-filing, four social insurances, integrated with the incumbent platform then made independent of it. RRNs stored as AES-256-GCM ciphertext only. Four thousand commits in seven weeks, all reviewed.
- **Platform.** EKS, ArgoCD, External Secrets, Forgejo, Fluent Bit, Elasticsearch with APM, IAM with forced MFA and IRSA, RDS over IAM auth, Vaultwarden, LiteLLM in front of every model call.

## <a name="formulas"></a>A spreadsheet formula engine

Accountants think in workbooks. Instead of translating them to code, an engine in Rust [evaluates the workbook]({% post_url 2026-01-25-reimplementing-spreadsheet-formulas %}) and matches the spreadsheet cell for cell: spill ranges, error propagation, the long tail of functions decided in 1995. A regression suite of real sheets is the [only reason it works]({% post_url 2026-03-15-spill-ranges-and-value-errors %}).

## <a name="ai"></a>A convert to AI-assisted engineering

Came back from New York a skeptic, changed my mind in a quarter. Most of Unitblack's code is written by agents, at [thousands of small commits a month]({% post_url 2026-06-25-two-thousand-commits-in-two-months %}), all reviewed by a human who can explain it. Usually me. The bottleneck moved from typing to understanding, and it has already [bitten me]({% post_url 2026-08-10-dont-log-the-whole-cookie %}).

So I built the harness the company runs on: a submodule in every repo with the judgment rules an agent follows, a risk/data/write tier per project, a named steward per system, a security review gate, a secret scanner that actually runs, scaffolds, and commands that keep each team's harness in sync with the org chart. Developers and non-developers get the same output quality. Not my first time around AI, either: at Skelter I worked on Iris, a hyper-personalization engine, and led Meerkat, a taste-based social app built on it.

## <a name="momenti"></a>A video format you can touch

Technical Architect at Momenti. Video that rotates when you drag it and responds when you tap it, in any phone browser, no plugin. Wrote the format's design docs and kept the [protobuf contract]({% post_url 2021-11-25-protobuf-as-the-contract-between-three-teams %}) between maker, player, admin, and engine honest. Took the Rust engine to browsers [through WASM]({% post_url 2022-09-25-shipping-a-rust-engine-to-the-browser-with-wasm %}) and to phones through FFI. Top contributor to the web player (scene renderer, streaming, overlay) and to the Flutter repo, where I led Spark, a social app on the format, and Motiv, a photo-card product for venues. Ran the [RFC process]({% post_url 2023-05-10-writing-my-first-rfc %}). Proposed [motion vectors from the codec]({% post_url 2023-07-30-motion-vectors-are-already-in-the-file %}) instead of computing them twice.

## <a name="hardware"></a>A smart plug that shipped

Led the IoT team at Skelter Labs in 2019 that built Brilli, a smart socket. In boxes, in living rooms. [MQTT, RabbitMQ]({% post_url 2019-03-20-smart-sockets-and-two-ender-3s %}), a Kotlin device server, BLE, firmware updates that must never brick. The two Ender-3s bought for enclosures became a [hobby I still have]({% post_url 2019-07-10-the-ender-5-arrived %}).

## <a name="kyte"></a>A flight search and booking app

Engineer number five at Skelter Labs, founded by ex-Googlers. Tech lead of Kyte for two years, two hundred thousand downloads. Swift on iOS, Kotlin on Android, and most of the server: search and recommendation on Elasticsearch, GDS integration for real ticketing, price-drop alerts, the REST gateway in front of gRPC, booking and PNR. A Spark fare pipeline underneath, ECS, and the ELK stack where I [watched the GeoIP dots appear]({% post_url 2016-07-25-logstash-geoip-and-the-first-server-i-owned %}). The worst bugs in a data product [are absences, not crashes]({% post_url 2016-11-20-why-is-shanghai-missing %}). Kyte spun out in early 2018; I stayed for the blockchain task force, the IoT team, and Meerkat. [Four years]({% post_url 2020-04-03-leaving-skelter-labs-after-four-years %}).

## <a name="squarelab"></a>CTO of a travel company

Followed Kyte to Squarelab in 2020 as [CTO]({% post_url 2020-06-15-cto-of-a-spinoff %}): twenty plus engineers, two products, travel frozen by a pandemic. Built [hotels on top of flights]({% post_url 2020-12-20-hotels-are-harder-than-flights %}): canonical hotel matching across a dozen suppliers, a rule-based room-grouping engine, coupons, TOPAS booking and ticketing. Kyte as a React web app, its successor prototyped in Flutter and [React Native]({% post_url 2020-10-10-shipping-a-react-native-app-in-a-flutter-shop %}), a white-label for a partner, the deals app and its real-time gateway. Monorepo into client and server workspaces, one protobuf toolchain for Kotlin, Node, and TypeScript, EKS. [Still wrote YAML]({% post_url 2021-04-25-a-cto-who-still-writes-yaml %}).

## <a name="games"></a>An MMO server

XL Games, Civilization Online, launched with 2K in 2015. [World and lobby servers]({% post_url 2015-03-20-world-servers-lobby-servers-and-a-patcher %}) in C++, three thousand players each, a diff-based patcher, and game DB migrations [through Django]({% post_url 2015-09-15-migrating-a-game-database-with-django %}). [Launch day]({% post_url 2015-12-20-launch-day %}).

## <a name="sap"></a>Inside a database

SAP Labs Korea, 2012 to mid-2014. SAP HANA PlanViz, the query optimizer's plan visualizer, and the optimizer itself. Java, C++, Eclipse RCP. To draw a plan well you have to understand it, a habit I [still keep]({% post_url 2021-04-25-a-cto-who-still-writes-yaml %}). Then half a year trying to start an IoT company on mesh networks and Zigbee. It did not work. XL Games in early 2015 with a drawer of dev boards.

## <a name="blockchain"></a>Two blockchain projects

2018: led a [task force]({% post_url 2018-08-20-six-months-as-a-blockchain-tech-lead %}), Solidity dApp, a well-argued "not now". 2022: an [NFT marketplace with auctions on Solana]({% post_url 2022-11-15-a-solana-nft-marketplace-prototype-in-rust %}), Rust on Metaplex, same ending. Both times a real answer instead of a guess.

## <a name="newyork"></a>Three years in New York

[Moved in 2022]({% post_url 2022-07-28-moving-to-new-york %}) with Momenti, working [Seoul's hours]({% post_url 2022-12-20-first-winter-in-nyc-working-korea-hours %}). When the funding stopped, [three months off]({% post_url 2024-01-25-three-months-of-not-working %}), then Koodos Labs, a shelf of everything you have read, watched, listened to, and played. Owned the data side: syncers for a streaming service, a bookshelf site, two music services, a game store, a game database; a sharded second generation that survives rate limits; a data-model migration verified against golden sets; [backfills]({% post_url 2024-05-25-smoke-tests-and-backfilling-scripts %}) that repaired months of history; [Goodreads, twice]({% post_url 2024-07-30-syncing-goodreads-again %}). [What the city taught me]({% post_url 2024-11-20-two-and-a-half-years-in-new-york %}). [Came home]({% post_url 2025-03-15-coming-back-to-korea %}) in 2025.

## <a name="elsewhere"></a>Elsewhere

Yonsei, computer science, top of the department. ACM-ICPC regional bronze. An internship at Google where my code shipped. A Django shop for an art gallery. Two years in the Air Force building the branch-wide emergency contact system; the Chief of Staff gave a conscript an award and a seat at lunch.

[Every working hour since 2017]({% post_url 2021-06-23-five-thousand-entries-later %}): about two thousand a year, whatever the job. A Mac mini on a shelf [runs my life]({% post_url 2026-05-20-a-headless-mac-launchd-and-a-stream-deck %}) through launchd and a Stream Deck daemon. A 3D printer that prints parts for the 3D printer.
