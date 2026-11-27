# Agents Usage Mac — Planning & Knowledge Wiki Index

도메인: macOS AI Agent Usage, Token Efficiency & Plan ROI Analytics  
최종 갱신: 2026-11-27  

> 질의 시 `index.md`에서 관련 페이지를 먼저 선별한 뒤 본문을 정독.  
> 인제스트·시각화·역축적 완료 시 이 카탈로그를 즉시 갱신.

---

## Overview

- [[overview|Agents Usage Mac — Planning & Knowledge Wiki Overview]] — 지식 베이스 전체 구조 및 핵심 진입 허브

---

## Visuals (Excalidraw)

- [[visuals/system-architecture.excalidraw|System Architecture]] — Dual Parser, Wake Catch-up, SQLite(Turns+Rollup)/App Group 공유, 노치 안전 메뉴 막대·위젯·대시보드
- [[visuals/telemetry-pipeline-and-accuracy.excalidraw|Telemetry Pipeline & Accuracy Boundaries]] — Tier 1 로컬 가드, Memory-Only 80c 미리보기, Adaptive Baseline, v2 ToS 준수 연동 흐름
- [[visuals/competitive-positioning.excalidraw|Competitive Positioning]] — 기존 모니터링 도구 vs 3대 기둥(모니터링·효율 코칭·요금제 ROI) 포지셔닝 대비
- [[visuals/sqlite-erd-and-modules.excalidraw|SwiftPM Modules & SQLite Data Flow]] — `AgentsParsers`/`AgentsCore`/`AgentsApp`/`AgentsWidget` 모듈 및 SQLite 9개 테이블 데이터 흐름

---

## Sources

- [[sources/market-competitors|시장·경쟁 도구 조사]] — CodexBar, ccusage, OpenUsage, ClaudeBar 분석 및 차별화 공백
- [[sources/local-cli-telemetry|로컬 CLI 텔레메트리 및 로그 스키마]] — Gemini CLI, Claude Code, Codex CLI 로그 경로·결함·제약
- [[sources/pricing-quotas-2026|2026년 11월 요금제·쿼터·API 단가]] — Google·Anthropic·OpenAI 구독 한도, 캐싱 배수, 모델별 API 단가
- [[sources/prompt-efficiency-metrics|프롬프트 토큰 효율 및 컨텍스트 엔지니어링 지표]] — 6대 정량 효율 지표 및 `/compact`·`/clear` 개선 레버
- [[sources/macos-architecture-constraints|macOS 플랫폼 제약 및 네이티브 아키텍처 근거]] — Apple 3-version OS 정책, App Group, WidgetKit 리로드 예산, Sparkle 배포
- [[sources/oauth-tos-constraints|3사 OAuth 정책, ToS 제약 및 합법적 쿼터 연동 조사]] — 소비자 OAuth 금지 근거, Claude Statusline `stdin` 브리지, GCP OAuth·Codex 프로브 대안
- [[sources/cli-schema-validation|로컬 CLI 환경 실측 및 업스트림 스키마 검증]] — 로컬 CLI 버전(`claude 2.1.316`, `codex 0.166.0`), 비민감 설정 및 3사 업스트림 JSON/JSONL 스키마 확정

---

## Concepts

- [[concepts/prompt-efficiency-scoring|프롬프트 효율 점수 모델]] — 턴 3분류, 통일 임계치, `/compact` 40% 자동 감지, `min(Score_B, Score_T)` 정규화 및 예외 규칙
- [[concepts/plan-roi-recommendation|요금제 ROI 추천 엔진 (v2)]] — 결제 주기·Auth 모드 분리, 반사실적 $V_{\text{adj}}$ 순차 산식, 플랜 변경 의사결정 매트릭스
- [[concepts/two-tier-telemetry-accuracy|2계층 텔레메트리 정확도 모델]] — Adaptive Baseline, PT 자정 리셋, 비민감 Auth 구분, Zero-Storage 80자 미리보기, v2 ToS 준수 연동
- [[concepts/macos-menubar-widget-architecture|macOS 메뉴 막대·위젯 네이티브 아키텍처]] — 이원화 수집, Sleep/Wake Catch-up, DB 수명주기/삭제권, 노치 안전 UI, `EN/KO` · `MIT`
- [[concepts/adr-core-decisions|핵심 아키텍처 의사결정 (ADR-001 ~ ADR-005)]] — App Group, GRDB WAL, 원시 토큰 SSOT Re-pricing, Zero-Storage, SwiftPM 4-모듈 결정

---

## Entities

- [[entities/gemini-cli|Gemini CLI (`~/.gemini`)]] — v1 1순위 주력 대상, `.jsonl` `$set`/SHA-256 `projectHash`, `API Request` 일일 카운팅, 소비자 OAuth 제약
- [[entities/claude-code|Claude Code (`~/.claude`)]] — v1 필수 대상, `requestId` 중복/플레이스홀더 함정, v2 공식 Statusline Bridge
- [[entities/codex-cli|OpenAI Codex CLI (`~/.codex`)]] — v1 필수 대상, `rollout-*.jsonl`, `last_token_usage`/`rate_limits`, 커스텀 루트 지정
- [[entities/competitor-landscape|경쟁 도구 비교 및 포지셔닝]] — 기존 macOS/CLI 도구 대비 차별화 매트릭스

---

## Syntheses

- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD v1.2 — Design-Ready Freeze): Agents Usage Mac]] — 1·2차 리뷰 및 OAuth/ToS 검증 완료 설계 착수 기준 기획서
- [[syntheses/tdd-agents-usage-mac|기술 설계서 (TDD v1.0): Agents Usage Mac]] — SwiftPM 4-모듈 구조, SQLite(GRDB) 9개 테이블 DDL, 파서 어댑터 계약, 동시성·골든 픽스처 테스트 전략
