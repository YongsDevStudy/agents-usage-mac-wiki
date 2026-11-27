---
id: entity-competitor-landscape
type: entity
aliases:
  - "경쟁 도구 비교 매트릭스"
tags:
  - entity
  - competitors
  - positioning
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/market-competitors]]"
status: active
---

# 경쟁 도구 비교 및 포지셔닝

- **목적**: 기존 macOS 메뉴 막대·CLI 사용량 도구군 대비 `agents-usage-mac`의 명확한 차별화 영역 정의.

---

## 1. 주요 경쟁자 역량 비교

| 비교 항목 | CodexBar | `ccusage` | OpenUsage | ClaudeBar 류 | **`agents-usage-mac` (목표)** |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **네이티브 macOS 메뉴 막대** | ✅ | ❌ (CLI 전용) | ✅ | ✅ | ✅ (`MenuBarExtra`) |
| **WidgetKit 데스크톱/알림 위젯** | 제한적/앱별 상이 | ❌ | ❌ | 일부 | ✅ (Small/Medium/Large) |
| **Gemini CLI 1순위·영구 적재** | 보조 수준 | ✅ (일회성 파싱) | 제한적 | ❌ | ✅ (`30d`/`50개` 삭제 방어) |
| **Claude + Codex 통합 뷰** | ✅ | ✅ | ✅ | ❌ | ✅ |
| **100% 로컬 처리 기본값 (v1)** | 혼합(쿠키/RPC) | ✅ | 혼합 | 혼합 | ✅ (Strict Local-First) |
| **질문(턴)당 토큰 효율 코칭** | ❌ | ❌ | ❌ | ❌ | ✅ (6대 지표 + 행동 트리거) |
| **요금제 ROI 비교·변경 추천** | ❌ | ❌ | ❌ | ❌ | ✅ (v2 핵심 차별화) |

---

## 2. 차별화 쐐기 (Differentiation Wedge)

1. **Monitor → Diagnose → Optimize 루프**: 단순 잔여 한도 게이지("얼마나 남았나")를 넘어 "왜 이 질문에서 토큰이 많이 들었나"([[concepts/prompt-efficiency-scoring]])와 "현재 요금제가 최적인가"([[concepts/plan-roi-recommendation]])까지 완결된 의사결정 지원.
2. **시각적 검증**: ![[visuals/competitive-positioning.excalidraw]]

---

## 관련 문서

- [[sources/market-competitors|시장·경쟁 도구 조사 요약]]
- [[entities/gemini-cli|Gemini CLI]]
- [[concepts/prompt-efficiency-scoring|프롬프트 효율 점수 모델]]
- [[concepts/plan-roi-recommendation|요금제 ROI 추천 엔진]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
