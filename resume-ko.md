---
layout: post
title: 이력서
permalink: /ko/resume/
---

**임승기 (Seunggi Lim)** · 소프트웨어 엔지니어
seung.gi.lim@gmail.com · [블로그](/) · [GitHub](https://github.com/sglim) · [About (English)](/about/)

## 기술

TypeScript/JavaScript (Node.js·NestJS·Web·React·React Native), Rust (WASM·FFI), Dart (Flutter), Kotlin (Android·서버·Native), Swift (iOS), Java, C++, Python, ReactiveX, ELK, DB (PostgreSQL·MongoDB), DevOps (K8S·Docker·ArgoCD·Pulumi), CI/CD (Jenkins·GitHub Actions), 인프라 (AWS: EKS·Lambda·RDS·SQS, GCP), 블록체인 (Solidity, Solana/Rust), AI 개발 (Claude Code, 에이전트 하네스)

## 경력

### [유닛블랙](https://unitblack.co.kr) — CTO · 2025.03 – 현재

소상공인 대상 세무·회계 소프트웨어. 주 사용 기술: TypeScript (NestJS), Rust, AWS (EKS·Lambda), Pulumi

**주요 성과**
- 히든머니: 소상공인의 종합소득세 5개년을 다시 계산해 경정청구까지 해 주는 환급 서비스 (API·어드민·배치·결제·CI/CD)
- 외부 벤더 블랙박스를 자체 엔진으로 교체: 국세청·여신협회 등 기관 웹 클라이언트를 순수 TypeScript 로 다시 구현해 AWS Lambda 에서 브라우저 없이 수집하고, 서비스마다 원본과 대조 검증 (2,000+ 커밋)
- 기관 서비스 188개 mock 서버를 실데이터 14,910건으로 검증해, 운영 데이터 없이 전체 파이프라인을 시험
- Rust 스프레드시트 수식 엔진 (세무 엑셀 워크북을 셀 단위로 동일하게 계산), AWS Lambda 종합소득세 계산기·신고서 생성기
- 급여 (급여 계산·원천세 전자신고·4대보험). 주민등록번호는 AES-256-GCM 암호문으로만 저장. 7주간 4,000+ 커밋 전량 리뷰
- 개발자와 비개발자가 함께 쓰는 전사 AI 개발 하네스 (에이전트 규칙, 프로젝트별 위험·데이터 등급, 보안 리뷰 게이트, 시크릿 스캔)

**CTO 역할**
- 플랫폼: EKS·ArgoCD·External Secrets, 자체 Forgejo, Fluent Bit → Elasticsearch·APM
- 접근 통제: IAM 강제 MFA·IRSA, RDS IAM 인증, Vaultwarden 전사 비밀번호
- 모든 AI 모델 호출 앞에 LiteLLM 을 두어 에이전트 비용 추적
- 에이전트가 쓴 코드 리뷰 (월 수천 개의 작은 커밋)

### [Koodos Labs](https://koodos.com) — Software Engineer Lead · 2024.03 – 2025.02

뉴욕 (일부 기간 한국에서 원격). 주 사용 기술: TypeScript (Node.js), Python, AWS (Pulumi), PostgreSQL

- 사용자가 읽고·보고·듣고·플레이한 기록을 한곳에 모으는 앱 Shelf 의 데이터 플랫폼 담당
- Netflix (IMDb 메타데이터)·Goodreads·Spotify·Apple Music·Steam·IGDB 싱커 개발
- 외부 서비스의 레이트 리밋을 견디는 샤딩 2세대 싱크 러너 설계 (AWS SQS)
- 핵심 데이터 모델을 골든셋으로 검증하며 마이그레이션, 멀티스레드 백필로 수개월치 이력 복구

### [Momenti](https://momenti.tv) — Technical Architect · 2021.06 – 2024.02

터치로 반응하는 인터랙티브 미디어 회사 (2022.07부터 뉴욕). 주 사용 기술: JavaScript/TypeScript, Flutter (iOS·Android·Web), Rust

**주요 성과**
- 비디오 코덱 지식을 바탕으로 Momenti 미디어 포맷의 설계·아키텍처 문서 작성
- 자원이 제한된 환경에서도 모든 플랫폼에서 끊김 없이 재생되는 Momenti Player 아키텍처 설계
- 프론트엔드 (Flutter·iOS·Web) 와 엔진 (Rust) 의 원인 불명 문제 해결
- Rust 엔진을 WASM·FFI 로 여러 플랫폼에 배포
- 설계 결함을 줄이기 위한 흐름도 정기 작성, 최신 비디오 코덱의 모션 벡터를 활용하는 방식 제안
- 기술 공유 세션 진행, 엔지니어링 의사결정을 위한 RFC 프로세스 운영
- 메이커·플레이어·어드민·엔진 사이 protobuf 계약 관리

**직접 개발**
- 웹·모바일 두 핵심 레포지토리 최다 기여자
- 플랫폼·브라우저 호환성 개선, 플랫폼 의존 코드와 엔진 아키텍처 분리
- 포토카드 B2B2C 서비스 Motiv, B2C 소셜 서비스 Spark 개발 리드
- Solana NFT 마켓플레이스·경매 프로토타입 (Rust)

### [Square Lab](https://squarelab.co) — CTO · 2020.04 – 2021.05

주 사용 기술: Kotlin (백엔드), Node.js, K8S, React, React Native, Flutter

**주요 성과**
- Skelter Labs 에서 분사하며 CTO 로 선임, 엔지니어 20명+ 조직 리드
- Kyte·Playwings (iOS·Android·Web) 팀 관리
- Kyte 를 항공에서 호텔로 확장: 공급사 10여 곳 호텔 통합 매칭, 룰 기반 객실 그룹핑 엔진
- Playwings 핫딜 푸시 알림 시스템, 모바일 앱의 웹 버전 개발
- 모노레포 재구성, Kotlin·Node·TypeScript protobuf 툴체인 통일, EKS 이전

**CTO 역할**
- 설계 문서 작성과 시스템 개발
- 마케팅·영업·고객지원 팀과 협업
- 엔지니어링 조직 구성과 평가
- 일정 산정과 자원 배분

### Skelter Labs — Senior Software Engineer · 2016.03 – 2020.03

주 사용 기술: Node.js, Kotlin (Android·Native), Swift (iOS), Flutter, C++

- 구글코리아 엔지니어링 총괄 출신 조원규 대표가 구글 출신들과 세운 회사의 창립 멤버 (엔지니어 5번째)
- Kyte 팀 테크리드, iOS·Android 누적 20만+ 다운로드
- Kyte iOS 앱 (Swift), Android (Kotlin) 상당 부분, 서버 대부분 개발: Elasticsearch 검색, GDS 발권, 가격 하락 알림
- IoT SW 팀 리드로 스마트 소켓 Brilli 출시: MQTT·RabbitMQ, Kotlin 디바이스 서버, BLE, 펌웨어 업데이트
- 블록체인 TF 테크리드로 암호화폐 기술 검토와 소형 dApp 개발
- 사내 개인화 엔진 Iris 기반 취향 SNS Meerkat 리드 (Flutter, TypeScript)
- 기술 면접 150건+, 사내 해커톤 2회 수상

### XL Games — Backend Software Engineer · 2015.02 – 2016.02

주 사용 기술: C++, Python, CryEngine

- 2K 와 Civilization Online 출시 (2015.12)
- 서버당 동시 접속 3,000+ MMORPG 서버 개발
- Django 마이그레이션으로 게임 DB 마이그레이션 개선, diff 기반 클라이언트 패치

### SAP Labs Korea — Software Engineer · 2012.04 – 2013.08

주 사용 기술: C++, Python, Java, Eclipse RCP

- SAP HANA PlanViz (DB 옵티마이저 실행 계획 시각화) 개발
- DB 옵티마이저 알고리즘 성능 개선, 단위 테스트 커버리지 향상

### Google Korea — Software Engineer Intern · 2011.09 – 2012.03

주 사용 기술: C++, Python

- 모바일 direct link 적용 범위 개선 (CSS 유사도 알고리즘), mobile direct link optimization 기능 프로덕션 출시

## 학력

**연세대학교 컴퓨터과학과 학사** · 2004.03 – 2012.02 (2011년 Google 인턴 병행)
전공 평점 4.1 / 4.3 · 전체 평점 3.7 / 4.3 · 컴퓨터과학과 수석 졸업
2010 가을학기·2009 가을학기 학업 최우수

## 수상

- 2011.11 ACM-ICPC 서울 리저널 동상
- 2010.11 Code Challenge 2위
- 2010.10 ACM-ICPC 서울 리저널 16위

## 기타

- 2026.05 – 현재 [everystat.ai](https://everystat.ai): LoL 프로 1,600명 비교, Riot 공식 경기 타임라인 기반
- 2024.07 – 2024.08 Open Avenues Foundation 펠로: 대학생 대상 8주 웹 크롤러 만들기 지도
- 2013.09 – 2015.01 메시 네트워크·Zigbee 기반 IoT 창업 준비, 외주 개발 (iBeacon 펌웨어 (C), Django 온라인 미술품 쇼핑몰, ML GPU 최적화)
- 2011.05 – 2011.06 Kinect 모션 캡처 개발
- 2009.08 – 2010.06 순수 Java 2D RPG 게임 엔진 (8,000줄)
- 2007.06 – 2009.08 공군 복무. 공군 전체 비상연락망 시스템을 개발해 공군참모총장 표창

## 공개 프로젝트

- [josa](https://pub.dev/packages/josa): Dart 한국어 조사 라이브러리
- [streamdeck-station](https://github.com/sglim/streamdeck-station): Elgato 앱 없이 헤드리스 맥에서 도는 Stream Deck 데몬
- [zonelearn](https://github.com/sglim/zonelearn): 레이더 좌표로 방 재실 구역을 학습
