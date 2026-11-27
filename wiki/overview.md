---
id: overview
type: overview
aliases:
  - "Agents Usage Mac — Planning & Knowledge Wiki"
tags:
  - overview
  - planning
  - prd
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/market-competitors]]"
  - "[[sources/local-cli-telemetry]]"
  - "[[sources/pricing-quotas-2026]]"
  - "[[sources/prompt-efficiency-metrics]]"
  - "[[sources/macos-architecture-constraints]]"
status: active
---

# Agents Usage Mac — Planning & Knowledge Wiki

- **도메인**: macOS AI Agent Usage, Token Efficiency & Plan ROI Analytics
- **저장소 구조**: 기획·설계·검증(`YongsDevStudy/agents-usage-mac-wiki`) ↔ 앱 구현(`YongsDevStudy/agents-usage-mac`)
- **기준일**: 2026-11-27

---

## 1. 핵심 산출물 (진입 허브)

- **최종 제품 기획서 (PRD v1.1)**: [[syntheses/prd-agents-usage-mac|Agents Usage Mac 제품 기획서 (PRD v1.1)]]
- **전체 카탈로그**: [[index|Wiki Index]]
- **작업 이력**: [[log|Wiki Log]]

---

## 2. 프로젝트 방향 요약 (확정 스코프)

1. **v1.0 MVP**: **100% 로컬 로그 파싱**(JSONL `byte_offset` + Gemini JSON 델타) 기반 노치 안전 메뉴 막대(`MenuBarExtra`) + `WidgetKit` 위젯 + 별도 대시보드(`Swift Charts`).
2. **필수 지원 CLI (3종)**:
   - 1순위 주력: [[entities/gemini-cli|Gemini CLI]] (`~/.gemini`, PT 자정 리셋 기준 + `<project_hash>` 3단계 매핑)
   - 필수 통합: [[entities/claude-code|Claude Code]] (`~/.claude`), [[entities/codex-cli|OpenAI Codex CLI]] (`~/.codex`)
3. **핵심 차별화**: [[entities/competitor-landscape|기존 한도 트래커]]의 공백을 채우는 **[[concepts/prompt-efficiency-scoring|3분류 턴 기반 효율 점수·코칭]]** 및 **[[concepts/plan-roi-recommendation|반사실적 $V_{\text{adj}}$ 요금제 ROI 추천(v2.0)]]**.
4. **아키텍처·정확도**: [[concepts/macos-menubar-widget-architecture|비샌드박스 메인 앱 + App Group + 샌드박스 WidgetKit]] (`macOS 15.0 Sequoia` 최소 지원 기준) 및 [[concepts/two-tier-telemetry-accuracy|Adaptive Baseline & Zero-Storage On-Demand 프라이버시 원칙]].

---

## 3. 아키텍처 개요

![[visuals/system-architecture.excalidraw]]
