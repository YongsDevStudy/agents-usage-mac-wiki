---
id: source-pricing-quotas-2026
type: source
raw_source: "raw/03-pricing-quotas-2026.md"
aliases:
  - "2026년 11월 요금제·쿼터·API 단가"
tags:
  - source
  - pricing
  - quotas
  - roi
created: 2026-11-27
updated: 2026-11-27
sources: []
status: active
---

# Source: 2026년 11월 기준 요금제·쿼터·API 단가

- **원본**: [[raw/03-pricing-quotas-2026.md]] (기준일: 2026-11-27)
- **핵심 요약**: Google·Anthropic·OpenAI 3사의 구독 한도 구조(일일 요청 수 vs 5시간/주간 롤링 풀)와 캐시 할인 단가가 급변 중이며, 2026년 12월 예정 단가 인상 등 시점 기반 카탈로그 외부화 필요.

---

## 주요 추출 팩트

1. **Google Gemini**:
   - CLI 일일 쿼터: 개인 계정 1,000 req/day, Google AI Pro 1,500 req/day, Ultra 2,000 req/day, 무료 API 키 250 req/day.
   - Gemini 4 Carbon/Argon 단가: $2.00/$10.00 → 2026-12-01 기준 $4.00/$20.00 인상 예정. Gemini 3.8 Flash 프로모션($0.75/$3.75)은 2026-12-31 종료 예정.
   - Gemini 4는 Implicit Caching 전용(최소 4,096 토큰, 최대 95% 할인).
2. **Anthropic Claude**:
   - 플랜: Pro $20(연 $17), Max(5x) $100(+$100 API 크레딧), Ultra $200(10x 주간, +$200 API 크레딧). 웹·CLI 풀 공유.
   - 캐싱: Write `1.25x`(5m)/`2.00x`(1h), Read `0.025x`~`0.10x`, 캐시 히트분 ITPM 미산입.
3. **OpenAI ChatGPT / Codex**:
   - 플랜: Plus $20(5시간·주간 롤링), Pro 100/200/500 ($100/$200/$500, 10x/20x/50x 풀 확장 및 전액 API 크레딧 환급). Instant 모드는 풀 미차감.
   - 캐싱 Read 90~95% 할인, 272K 토큰 초과 장문 입력 2배 할증.

---

## 관련 위키 문서

- [[concepts/plan-roi-recommendation|요금제 ROI 추천 엔진]]
- [[entities/gemini-cli|Gemini CLI]]
- [[entities/claude-code|Claude Code]]
- [[entities/codex-cli|OpenAI Codex CLI]]
