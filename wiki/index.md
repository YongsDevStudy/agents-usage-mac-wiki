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

- [[visuals/system-architecture.excalidraw|System Architecture]] — 로컬 CLI 로그 수집, SQLite/App Group 공유, 메뉴 막대·위젯·대시보드 아키텍처
- [[visuals/telemetry-pipeline-and-accuracy.excalidraw|Telemetry Pipeline & Accuracy Boundaries]] — Tier 1 로컬 가드(중복/플레이스홀더/보존주기) 및 Tier 2 Opt-in 정확도 배지 흐름
- [[visuals/competitive-positioning.excalidraw|Competitive Positioning]] — 기존 모니터링 도구 vs 3대 기둥(모니터링·효율 코칭·요금제 ROI) 포지셔닝 대비

---

## Sources

- [[sources/market-competitors|시장·경쟁 도구 조사]] — CodexBar, ccusage, OpenUsage, ClaudeBar 분석 및 차별화 공백
- [[sources/local-cli-telemetry|로컬 CLI 텔레메트리 및 로그 스키마]] — Gemini CLI, Claude Code, Codex CLI 로그 경로·결함·제약
- [[sources/pricing-quotas-2026|2026년 11월 요금제·쿼터·API 단가]] — Google·Anthropic·OpenAI 구독 한도, 캐싱 배수, 모델별 API 단가
- [[sources/prompt-efficiency-metrics|프롬프트 토큰 효율 및 컨텍스트 엔지니어링 지표]] — 6대 정량 효율 지표 및 `/compact`·`/clear` 개선 레버
- [[sources/macos-architecture-constraints|macOS 플랫폼 제약 및 네이티브 아키텍처 근거]] — Apple 3-version OS 정책, App Group, WidgetKit 리로드 예산, Sparkle 배포

---

## Concepts

- [[concepts/prompt-efficiency-scoring|프롬프트 효율 점수 모델]] — 질문(턴) 단위 6대 지표 산식, 0–100점 가중 모델, 코칭 액션 트리거
- [[concepts/plan-roi-recommendation|요금제 ROI 추천 엔진 (v2)]] — API 환산 가치($V_{\text{api}}$), 낭비 보정 소비액($V_{\text{adj}}$), 플랜 변경 의사결정 매트릭스
- [[concepts/two-tier-telemetry-accuracy|2계층 텔레메트리 정확도 모델]] — Tier 1(실측/추정) vs Tier 2(Opt-in 공식 동기화) 경계 및 프라이버시 원칙
- [[concepts/macos-menubar-widget-architecture|macOS 메뉴 막대·위젯 네이티브 아키텍처]] — 최소 OS 기준, 비샌드박스+App Group, WidgetKit 리로드 제어, 공증 배포

---

## Entities

- [[entities/gemini-cli|Gemini CLI (`~/.gemini`)]] — v1 1순위 주력 대상, `sessionRetention` 데이터 유실 방어, 일일 쿼터·Implicit Caching
- [[entities/claude-code|Claude Code (`~/.claude`)]] — v1 필수 대상, `requestId` 중복/플레이스홀더 함정, 5시간 롤링 블록
- [[entities/codex-cli|OpenAI Codex CLI (`~/.codex`)]] — v1 필수 대상, `rollout-*.jsonl`, `CODEX_HOME`, 장문 컨텍스트 할증
- [[entities/competitor-landscape|경쟁 도구 비교 및 포지셔닝]] — 기존 macOS/CLI 도구 대비 차별화 매트릭스

---

## Syntheses

- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD): Agents Usage Mac]] — 실무 현업 수준 종합 기획서 (비전, 로드맵, FR-01~07, IA/UX, NFR, 리스크 매트릭스)
