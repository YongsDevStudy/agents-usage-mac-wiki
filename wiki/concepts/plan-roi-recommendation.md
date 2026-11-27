---
id: concept-plan-roi-recommendation
type: concept
aliases:
  - "요금제 ROI 추천 엔진"
tags:
  - concept
  - roi
  - pricing
  - v2-roadmap
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/pricing-quotas-2026]]"
  - "[[sources/market-competitors]]"
status: active
---

# 요금제 ROI 추천 엔진 (Plan ROI & Recommendation, v2)

- **목적**: 최근 30~90일 누적 실측 데이터를 기반으로 현재 월 구독 플랜의 경제성과 병목 여부를 평가하고 **다운그레이드·유지·업그레이드·API 종량제 병행** 중 최적 대안 제시.
- **로드맵 위치**: v1에서 데이터 적재·API 환산 비용 산출 기반 구축 후 **v2.0 핵심 기능**으로 제공.

---

## 1. 핵심 산출 지표

1. **API 환산 가치 (API-Equivalent Value, $V_{\text{api}}$)**:
   - 실제 사용한 모델별 Input / Output / Cache Write / Cache Read / 장문 할증(>272K) 토큰을 공식 API 단가로 환산한 월간 총액.
2. **구독 ROI 배수 (Subscription Leverage Ratio)**:
   - $R = V_{\text{api}} \div P_{\text{plan}}$ (예: $20 Pro 구독 중 API 환산 소비액이 $85이면 $R = 4.25\times$).
3. **한도 차단 마찰 횟수 (Rate-Limit Friction Count)**:
   - 최근 30일간 5시간 블록·주간 캡·일일 요청 한도(90% 도달 또는 작업 중단) 발생 빈도.
4. **비효율 제거 후 조정 소비액 ($V_{\text{adj}}$)**:
   - [[concepts/prompt-efficiency-scoring]] 기준 과다 컨텍스트/미압축 낭비 추정치를 제거했을 때의 실질 필요 소비량 (단순 낭비로 인한 불필요한 상위 요금제 추천 방지).

---

## 2. 추천 의사결정 매트릭스

| 조건 패턴 (최근 30일 기준) | 권고 액션 | 근거 설명 예시 |
| :--- | :--- | :--- |
| $V_{\text{api}} < 0.7 \times P_{\text{plan}}$ AND 마찰 0~1회 | **다운그레이드 / API 종량제 전환** | "월 구독료($100) 대비 실제 API 환산 소비액 $42 — Pro($20)+API 종량제 조합 시 월 $38 절감" |
| 마찰 ≥ 월 4회 AND 상위 티어 API 크레딧 활용 가능 | **업그레이드 (예: Pro→Max / Plus→Pro 100)** | "주 2회 이상 5시간 한도 중단 발생 — Max(5x) 전환 시 크레딧 포함 손익분기 충족" |
| $V_{\text{adj}} < P_{\text{plan}} < V_{\text{api}}$ | **플랜 유지 + 효율 개선 우선** | "총 소비는 크나 미압축 컨텍스트 재전송 비중 45% — `/compact` 습관화 후 재평가 권장" |
| Gemini CLI 일일 1,000 req 빈번 도달 | **Google AI Pro/Ultra 전환 또는 API 키 폴백** | "일 1,000건 이상 요청 6일 발생 — Pro(1,500 req/day) 상향 권장" |

---

## 3. 신뢰도 가드레일

- 로컬 적재 기간 **14일 미만** 시 추천 비활성("데이터 수집 중").
- 웹/모바일 병행 사용 비중이 높은 계정은 Tier 1 로컬 추정치임을 경고하고, Tier 2 동기화 또는 사용자 수동 가중치(웹 사용 비율 입력) 보정 지원([[concepts/two-tier-telemetry-accuracy]] 참조).

---

## 관련 문서

- [[sources/pricing-quotas-2026|2026년 11월 요금제·단가 요약]]
- [[concepts/prompt-efficiency-scoring|프롬프트 효율 점수 모델]]
- [[concepts/two-tier-telemetry-accuracy|2계층 텔레메트리 정확도 모델]]
- [[entities/claude-code|Claude Code]]
- [[entities/codex-cli|OpenAI Codex CLI]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
