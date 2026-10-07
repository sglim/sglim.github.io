---
layout: post
title: Resume
permalink: /resume/
---

**Seung-gi Lim** · Software Engineer
seung.gi.lim@gmail.com · [Blog](/) · [GitHub](https://github.com/sglim) · [About](/about/)

## Skills

TypeScript/JavaScript (Node.js/NestJS/Web/React/RN), Rust (WASM/FFI), Dart (Flutter), Kotlin (Android/Server/Native), Swift (iOS), Java, C++, Python, ReactiveX (RxJava/RxJS/ReactiveKit), ELK stack, DB (PostgreSQL/MongoDB), DevOps (K8S/Docker/ArgoCD/Pulumi), CI/CD (Jenkins, GitHub Actions), Infrastructure (AWS: EKS/Lambda/RDS/SQS, GCP), Blockchain (Solidity, Solana/Rust), AI-assisted engineering (Claude Code, agent harnesses)

## Experience

### [Unitblack](https://unitblack.co.kr) (Korea) / CTO · Mar 2025 – Present

Mainly used platforms/languages: TypeScript (NestJS), Rust, AWS (EKS/Lambda), Pulumi

**Key Achievements**
- Work with AI at scale: 50+ Claude Code sessions in parallel across three Macs (73 open as I write this, up to 35 working in the same hour) and 253.7B tokens since March 2026, over 70x Anthropic's published enterprise average per active day. Every commit reviewed by a human
- Built Savetax Refund (formerly Hiddenmoney), which recomputes five years of income tax returns for Korean small businesses and files the corrections
- Wrote a Rust compiler that turns Google Sheets tax calculators into standalone engines; 98.5% of cells match on the full income tax sheet (151,779 cells)
- Built SAVE PRO, an in-house payroll and tax platform on a double-entry ledger, to replace a third-party payroll SaaS: 6,100+ commits in 11 weeks, ~75 a day
- Built the tax-filing back office alone in five weeks: ~200,000 filings in its first season, replacing a 122-sheet refund spreadsheet at 98.1% parity
- Built BearTable, a self-hosted Airtable replacement on Postgres, and cut a full recompute of 610K rows across 29 formula tables from ~130 to 59.5 minutes
- Built an internal knowledge hub that archives Slack into Postgres search, with a chatbot on the company LLM gateway

**CTO Responsibilities**
- Run the platform: EKS with ArgoCD GitOps, self-hosted Forgejo with Google SSO and CI, Pulumi state moved off Pulumi Cloud to KMS-encrypted S3
- Led an ISMS-P sweep: access logging went from 0 to all 41 personal-data buckets, deletion protection to all 11 databases, prod writes to MFA-only roles
- Fixed internet-facing gaps found in the ISMS-P audit, including a world-writable bucket, MySQL and FTP open to the internet, and months-old privileged pods
- Protected personal data: national IDs encrypted and stored only after filing consent, the key removed from app configs, dev keys cut off from prod PII
- Security-audited all 852 Notion knowledge pages, out of 68,848 items
- Designed the AI citizen-development program (risk tiers, developer review) and a Claude Code harness for every employee and a partner firm, 222 people in all
- Enabled a non-developer to ship 270+ tax-rule commits, over half of all commits, to the income tax calculator through Claude Code
- Put a LiteLLM gateway on Amazon Bedrock to meter Claude Code use per user, and built a PR review bot that reads each diff through a call graph

### [Koodos Labs](https://koodos.com) (NY, remote) / Software Engineer Lead · Mar 2024 – Feb 2025

Mainly used platforms/languages: TypeScript (Node.js/Next.js/tRPC), Supabase (Postgres), Kysely, Zod, AWS (ECS/Lambda/SQS/S3, Pulumi), Cloudflare Images, Grafana Loki

- Owned the data platform of Shelf, an app that keeps everything a user reads, watches, listens to, and plays
- Built data syncers for Netflix (with IMDb metadata), Goodreads, Spotify, Apple Music, Steam, and IGDB
- Designed a sharded second-generation sync runner on AWS SQS that survives third-party rate limits
- Migrated the core data model against golden sets and repaired months of history with multithreaded backfills

### [Momenti](https://momenti.tv/) (NY) / Technical Architect · Jun 2021 – Feb 2024

A company building touch-based interactive media. Mainly used platforms/languages: JavaScript/TypeScript, Flutter (iOS/Android/Web), Rust

**Key Achievements**
- Created comprehensive design and architecture documents for the Momenti media format, leveraging expertise in video codecs
- Designed an efficient architecture for the Momenti Player to ensure seamless playback on any platform, even under resource constraints
- Led the distribution of the Rust engine to multiple platforms using technologies like WASM and FFI
- Suggested a different approach using motion vectors in modern video codecs
- Kept the protobuf contract between maker, player, admin, and engine consistent

**Hands-On Contributions**
- #1 contributor in both main repositories, web and mobile
- Improved compatibility of the player across various platforms and browsers
- Decoupled platform-dependency and engine-specific architecture
- Led the development of the photocard-based B2B2C service, Motiv
- Led the B2C social service, Spark
- Prototyped an NFT marketplace and auction with Solana using Rust

### [Squarelab](https://squarelab.co/) (Korea) / CTO · Apr 2020 – May 2021

Mainly used platforms/languages: Kotlin (Backend), Node.js, K8S, React, React Native, Flutter

**Key Achievements**
- Became CTO at the spinoff from Skelter Labs and led 20+ engineers across two products, Kyte and Playwings (iOS/Android/Web)
- Expanded Kyte from flights to hotels, matching hotels across a dozen suppliers
- Implemented a robust notification system for pushing hot deals in Playwings
- Developed a Web version of the mobile applications to improve user engagement with the app

### [Skelter Labs](https://www.skelterlabs.com/) (Korea) / Senior Software Engineer · Mar 2016 – Mar 2020

Mainly used platforms/languages: Node.js, Kotlin (Android, Native), Swift (iOS), Flutter, C++

- Became one of the founding members (initial engineer #5) at Skelter Labs, founded by the former head of engineering at Google Korea, [Ted Cho](https://www.crunchbase.com/person/ted-cho), with ex-Googlers
- Led the Kyte team as a Tech Lead, achieving a total download of 200k+ in both mobile marketplaces ([iOS](https://apps.apple.com/kr/app/kyte-%EC%9A%B0%EB%A6%AC%EB%8A%94-%ED%98%84%EC%9E%AC-%EC%97%AC%ED%96%89%ED%98%95-%EC%B9%B4%EC%9D%B4%ED%8A%B8/id1208047465)/[Android](https://play.google.com/store/apps/details?id=co.squarelab.kyte&hl=en_US)). Kyte is an online travel agency app that sells flight tickets directly, with a real-time fare calendar from Global Distribution Systems (GDS)
- Led the IoT SW team that shipped Brilli, a smart socket: MQTT, RabbitMQ, a Kotlin device server, BLE, and firmware updates
- Explored cryptocurrency technology and developed a small dApp as the Blockchain TF Team Tech Lead
- Led Meerkat, a taste-based social app on Iris; ran 150+ technical interviews

### [XL Games](https://company.xlgames.com/en) (Korea) / Backend Software Engineer · Feb 2015 – Feb 2016

Mainly used platforms/languages: C++, Python, CryEngine

- Launched [Civilization Online](https://civilizationonline.com/) with 2K Games in Dec 2015
- Developed high-concurrency MMORPG server (3000+ players in a server)
- Improved Game DB migration process by applying Django migration
- Applied a diff-based patching method to the game client for efficient update

### [SAP Labs Korea](https://www.sap.com/korea/about/labs-korea.html) (Korea) / Software Engineer · Apr 2012 – Aug 2013

Mainly used platforms/languages: C++, Python, Java, Eclipse RCP

- Developed SAP HANA PlanViz (DB Optimizing Plan Visualizer)
- Enhanced performance of DB Optimizer algorithm
- Raised coverage of unit test for the robustness

### Google Korea (Korea) / Software Engineer Intern · Sep 2011 – Mar 2012

Mainly used platforms/languages: C++, Python

- Improved coverage of direct links for mobile (CSS similarity algorithm)
- Launched a feature called mobile direct link optimization to the production

## Education

**Yonsei University** (#2 in Korea) / Bachelor of Computer Science · Mar 2004 – Feb 2012, Seoul, Korea
Major GPA: 4.1 / 4.3 · Total GPA: 3.7 / 4.3
Graduated as the Valedictorian of the Department of Computer Science
- 2010 Fall semester Summa cum laude
- 2009 Fall semester Summa cum laude

## Awards

- Nov 2011 ACM-ICPC Seoul Regional Bronze Prize
- Nov 2010 Code Challenge 2nd Prize
- Oct 2010 ACM-ICPC Seoul Regional 16th place

## Special

- May 2026 – Present: Built [everystat.ai](https://everystat.ai), LoL esports stats comparing 1,600 pro players, from Riot's official match timelines
- Jul 2024 – Aug 2024: Open Avenues Foundation Fellow: taught university students to build a web crawler over eight weeks
- Sep 2013 – Jan 2015: Prepared an IoT startup on mesh networks and Zigbee, with freelance work: iBeacon firmware (C), a Django online art shop, GPU optimization for ML
- May 2011 – Jun 2011: Developed Kinect Motion Capture ([YouTube](https://youtu.be/A0QnMm2-qLs))
- Aug 2009 – Jun 2010: Created 2D RPG game engine in pure Java (SLOC: 8000)
- Jun 2007 – Aug 2009: Fulfilled mandatory military service in the Korean Air Force; built the Air Force-wide emergency contact system and received a commendation from the Chief of Staff
