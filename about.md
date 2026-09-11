---
layout: post
title: About
permalink: /about/
---

I am the CTO of [Unitblack](#unitblack), where I built the engine behind [Hidden Money](#unitblack), a service that finds the tax refunds Korean small businesses are owed. Before that: a [spreadsheet formula engine](#formulas) in Rust, an [AI harness](#ai) a whole company works through, a [video format you can touch](#momenti), a [smart plug that shipped](#hardware), a [flight search and booking app](#kyte) that ate tens of gigabytes of airline data a day, an [MMO server](#games) holding three thousand players, a [query plan visualizer](#sap) inside SAP HANA, and [two blockchain projects](#blockchain) that ended in "not now". I spent [three years in New York](#newyork) and I watch [a lot of League of Legends](#elsewhere).

I write here, in English, about what I built and what it taught me. GitHub: [sglim](https://github.com/sglim).

## <a name="unitblack"></a>CTO of Unitblack

Tax and accounting software for Korean small businesses. Joined 2025 on a [trial run]({% post_url 2025-06-10-tax-software-and-a-fresh-start %}), then CTO.

- **Hidden Money.** A business signs in once, authorizes us, and we retrieve its own filings from the [tax authority]({% post_url 2025-12-22-reverse-engineering-a-government-login %}) on its behalf, recompute five years of returns, and file the corrections. I worked across the API, admin, batch, shared models, and the CI/CD for all of them.
- **The retrieval engine.** The tax authority has a website, not an API, so the industry runs on vendor black boxes. I led the replacement: a TypeScript client that does exactly what the authority's own browser client does, headless on Lambda, verified against the original for every service. Then the same for a card clearing house whose front end changes under you, delivery-app portals, and a certificate flow. Plus a mock of all of it, seeded from tens of thousands of real records, so the whole pipeline is tested end to end without touching production data. Two thousand commits in that repo alone.
- **Calculation.** The [Rust formula engine](#formulas), a single-Lambda income tax calculator, a filer. A tax specialist changes a deduction limit by asking an agent in Korean.
- **Payroll.** Withholding e-filing, four social insurances, integrated with the incumbent platform and then made independent of it. National IDs stored only as AES-256-GCM ciphertext. Four thousand commits in seven weeks, all reviewed.
- **Platform.** The cluster and everything around it: EKS with ArgoCD and External Secrets, a self-hosted Forgejo, Fluent Bit into Elasticsearch with APM, IAM with forced MFA and IRSA, RDS over IAM auth, Vaultwarden for company passwords, and LiteLLM in front of every model call so we know what the agents cost.

## <a name="formulas"></a>A spreadsheet formula engine

Accountants think in workbooks. Instead of translating them to code, an engine in Rust [evaluates the workbook]({% post_url 2026-01-25-reimplementing-spreadsheet-formulas %}) and matches the spreadsheet cell for cell: spill ranges, error propagation, the long tail of functions decided in 1995. A regression suite of real sheets is the [only reason it works]({% post_url 2026-03-15-spill-ranges-and-value-errors %}).

## <a name="ai"></a>A convert to AI-assisted engineering

Came back from New York a skeptic, changed my mind in a quarter. Most of Unitblack's code is written by agents, at [thousands of small commits a month]({% post_url 2026-06-25-two-thousand-commits-in-two-months %}), all reviewed by a human who can explain it. Usually me. The bottleneck moved from typing to understanding, and it has already [bitten me]({% post_url 2026-08-10-dont-log-the-whole-cookie %}).

So I built the harness the company runs on: a submodule in every repo with the judgment rules an agent follows, a risk, data, and write tier per project, a named steward per system, a security review gate, a secret scanner that actually runs, scaffolds, and commands that keep each team's harness in sync with the org chart. Developers and non-developers get the same output quality. Not my first time around AI, either: at Skelter I worked on Iris, a hyper-personalization engine, and led Meerkat, a taste-based social app built on it.

## <a name="momenti"></a>A video format you can touch

Technical Architect at Momenti. Video that rotates when you drag it and responds when you tap it, in any phone browser, no plugin. Wrote the format's design docs and kept the [protobuf contract]({% post_url 2021-11-25-protobuf-as-the-contract-between-three-teams %}) between maker, player, admin, and engine honest. Took the Rust engine to browsers [through WASM]({% post_url 2022-09-25-shipping-a-rust-engine-to-the-browser-with-wasm %}) and to phones through FFI. Top contributor to the web player (scene renderer, streaming, overlay) and to the Flutter repo, where I led Spark, a social app on the format, and Motiv, a photo-card product for venues. Ran the [RFC process]({% post_url 2023-05-10-writing-my-first-rfc %}). Proposed [motion vectors from the codec]({% post_url 2023-07-30-motion-vectors-are-already-in-the-file %}) instead of computing them twice.

## <a name="hardware"></a>A smart plug that shipped

Led the IoT team at Skelter Labs in 2019 that built Brilli, a smart socket. In boxes, in living rooms. [MQTT, RabbitMQ]({% post_url 2019-03-20-smart-sockets-and-two-ender-3s %}), a Kotlin device server, BLE, firmware updates that must never brick. The two Ender-3s bought for enclosures became a [hobby I still have]({% post_url 2019-07-10-the-ender-5-arrived %}).

## <a name="kyte"></a>A flight search and booking app

Engineer number five at Skelter Labs, founded by ex-Googlers. I built Kyte: tech lead for two years, two hundred thousand downloads. The iOS app in Swift, much of Android in Kotlin, and most of the server: search and recommendation on Elasticsearch, the GDS integration that turned a result into a ticket, price-drop alerts, the REST gateway in front of gRPC. A Spark fare pipeline underneath, ECS, and the ELK stack where I [watched the GeoIP dots appear]({% post_url 2016-07-25-logstash-geoip-and-the-first-server-i-owned %}). The worst bugs in a data product [are absences, not crashes]({% post_url 2016-11-20-why-is-shanghai-missing %}). Kyte spun out into its own company in early 2018; I stayed at Skelter for the blockchain task force, the IoT team, and Meerkat. [Four years]({% post_url 2020-04-03-leaving-skelter-labs-after-four-years %}).

## <a name="squarelab"></a>CTO of a travel company

In 2020 I followed Kyte to Squarelab, the company it had become, as [CTO]({% post_url 2020-06-15-cto-of-a-spinoff %}). Different job: twenty plus engineers, two products, and a travel market frozen by a pandemic. The team shipped [hotels on top of flights]({% post_url 2020-12-20-hotels-are-harder-than-flights %}), with canonical hotel matching across a dozen suppliers and a rule-based room-grouping engine, a React web version of Kyte, a white-label for a partner, and a [React Native]({% post_url 2020-10-10-shipping-a-react-native-app-in-a-flutter-shop %}) app from the web team. I restructured the monorepo, unified the protobuf toolchain across Kotlin, Node, and TypeScript, moved us onto EKS, and [still wrote YAML]({% post_url 2021-04-25-a-cto-who-still-writes-yaml %}).

## <a name="games"></a>An MMO server

XL Games, Civilization Online, launched with 2K in 2015. [World and lobby servers]({% post_url 2015-03-20-world-servers-lobby-servers-and-a-patcher %}) in C++, three thousand players each, a diff-based patcher, and game DB migrations [through Django]({% post_url 2015-09-15-migrating-a-game-database-with-django %}). [Launch day]({% post_url 2015-12-20-launch-day %}).

## <a name="sap"></a>SAP HANA

SAP Labs Korea, 2012 to mid-2014. PlanViz, the query optimizer's plan visualizer, and the optimizer itself. Java, C++, Eclipse RCP. To draw a plan well you have to understand it, a habit I [still keep]({% post_url 2021-04-25-a-cto-who-still-writes-yaml %}). Then half a year trying to start an IoT company on mesh networks and Zigbee. It did not work. XL Games in early 2015 with a drawer of dev boards.

## <a name="blockchain"></a>Two blockchain projects

2018: led a [task force]({% post_url 2018-08-20-six-months-as-a-blockchain-tech-lead %}), Solidity dApp, a well-argued "not now". 2022: an [NFT marketplace with auctions on Solana]({% post_url 2022-11-15-a-solana-nft-marketplace-prototype-in-rust %}), Rust on Metaplex, same ending. Both times a real answer instead of a guess.

## <a name="newyork"></a>Three years in New York

[Moved in 2022]({% post_url 2022-07-28-moving-to-new-york %}) with Momenti, working [Seoul's hours]({% post_url 2022-12-20-first-winter-in-nyc-working-korea-hours %}). In 2024 I moved to Koodos Labs, a shelf of everything you have read, watched, listened to, and played. Owned the data side: syncers for a streaming service, a bookshelf site, two music services, a game store, a game database; a sharded second generation that survives rate limits; a data-model migration verified against golden sets; [backfills]({% post_url 2024-05-25-smoke-tests-and-backfilling-scripts %}) that repaired months of history; [Goodreads, twice]({% post_url 2024-07-30-syncing-goodreads-again %}). That summer I was also a fellow at Open Avenues, [teaching university students to build a crawler]({% post_url 2024-08-28-teaching-students-to-build-a-crawler %}) from zero over eight weekly sessions; the [workshop code](https://github.com/sglim/openavenues) is public. [What the city taught me]({% post_url 2024-11-20-two-and-a-half-years-in-new-york %}). [Came home]({% post_url 2025-03-15-coming-back-to-korea %}) in 2025.

## <a name="elsewhere"></a>Elsewhere

I watch a lot of League of Legends, LCK mostly, and got tired of arguing about players with no numbers, so I built [everystat](https://everystat.ai): 1,600 pros compared on six axes from match data. It runs on a Mac mini on a shelf that also [runs my life]({% post_url 2026-05-20-a-headless-mac-launchd-and-a-stream-deck %}) through launchd and a [Stream Deck daemon](https://github.com/sglim/streamdeck-station) I wrote because the official app wanted a monitor. Smaller things in the open: [josa](https://pub.dev/packages/josa), a Dart library for Korean particles; [zonelearn](https://github.com/sglim/zonelearn), which learns room occupancy zones from radar; [ccswitch](https://github.com/sglim/ccswitch), which juggles Claude accounts.

Before all that: Yonsei, computer science, top of the department. ACM-ICPC regional bronze. An internship at Google where my code shipped. Two years in the Air Force building the branch-wide emergency contact system; the Chief of Staff gave a conscript an award and a seat at lunch. [Every working hour since 2017]({% post_url 2021-06-23-five-thousand-entries-later %}) is in a spreadsheet: about two thousand a year, whatever the job. A 3D printer that prints parts for the 3D printer.
