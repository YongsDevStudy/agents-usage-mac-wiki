---
id: overview
type: overview
aliases:
  - "Agents Usage Mac — Planning & Knowledge Wiki"
tags:
  - overview
  - planning
  - prd
  - tdd
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/market-competitors]]"
  - "[[sources/local-cli-telemetry]]"
  - "[[sources/pricing-quotas-2026]]"
  - "[[sources/prompt-efficiency-metrics]]"
  - "[[sources/macos-architecture-constraints]]"
  - "[[sources/oauth-tos-constraints]]"
  - "[[sources/cli-schema-validation]]"
status: active
---

# Agents Usage Mac — Planning & Knowledge Wiki

- **도메인**: macOS AI Agent Usage, Token Efficiency & Plan ROI Analytics
- **저장소 구조**: 기획·설계·검증(`YongsDevStudy/agents-usage-mac-wiki`) ↔ 앱 구현(`YongsDevStudy/agents-usage-mac`)
- **라이선스 / 현지화**: MIT License · 영어/한국어(`.xcstrings`) · USD(선택 KRW)
- **기준일**: 2026-11-27

---

## 1. 핵심 산출물 (진입 허브)

- **제품 기획서 (PRD v1.2 — Design-Ready Freeze)**: [[syntheses/prd-agents-usage-mac|Agents Usage Mac 제품 기획서 (PRD v1.2)]]
- **기술 설계서 (TDD v1.0 — SwiftPM & SQLite DDL)**: [[syntheses/tdd-agents-usage-mac|Agents Usage Mac 기술 설계서 (TDD v1.0)]]
- **핵심 아키텍처 의사결정**: [[concepts/adr-core-decisions|ADR-001 ~ ADR-005]]
- **전체 카탈로그**: [[index|Wiki Index]] · **작업 이력**: [[log|Wiki Log]]

---

## 2. 프로젝트 방향 요약 (확정 스코프)

1. **v1.0 MVP**: **100% 로컬 로그 파싱**(JSONL `byte_offset` + Gemini `.jsonl`/`$set`/`.json` 델타 + Sleep/Wake Catch-up) 기반 노치 안전 메뉴 막대(`MenuBarExtra`) + `WidgetKit` 위젯 + 별도 대시보드(`Swift Charts`).
2. **필수 지원 CLI (3종)**:
   - 1순위 주력: [[entities/gemini-cli|Gemini CLI]] (`~/.gemini`, `API Request` 일일 카운팅 + PT 자정 리셋 + `<project_hash>` SHA-256 3단계 매핑)
   - 필수 통합: [[entities/claude-code|Claude Code]] (`~/.claude`, `2.1.316`), [[entities/codex-cli|OpenAI Codex CLI]] (`~/.codex`, `0.166.0`)
3. **핵심 차별화**: [[entities/competitor-landscape|기존 한도 트래커]]의 공백을 채우는 **[[concepts/prompt-efficiency-scoring|3분류 턴 기반 효율 점수·코칭]]** 및 **[[concepts/plan-roi-recommendation|반사실적 $V_{\text{adj}}$ 요금제 ROI 추천(v2.0)]]**.
4. **아키텍처·정확도·ToS 준수**: [[concepts/macos-menubar-widget-architecture|비샌드박스 메인 앱 + App Group + 샌드박스 WidgetKit]] (`macOS 15.0 Sequoia` 최소 지원), [[concepts/two-tier-telemetry-accuracy|Adaptive Baseline & Zero-Storage On-Demand 프라이버시 원칙]], [[sources/oauth-tos-constraints|소비자 OAuth 스크래핑 금지(v2.0 Claude Statusline Bridge 등 합법 연동)]], [[sources/cli-schema-validation|로컬 CLI 환경 및 업스트림 스키마 검증]].

---

## 3. 아키텍처 개요

![[visuals/system-architecture.excalidraw]]
