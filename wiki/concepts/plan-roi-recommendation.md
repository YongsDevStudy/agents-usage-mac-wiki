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

- **목적**: 최근 30~90일(또는 사용자 결제 주기) 누적 실측 데이터를 기반으로 현재 월 구독 플랜의 경제성과 병목 여부를 평가하고 **다운그레이드·유지·업그레이드·API 종량제 병행** 중 최적 대안 제시.
- **로드맵 위치**: v1에서 데이터 적재·API 환산 비용 산출·Auth 모드 구분 기반 구축 후 **v2.0 핵심 기능**으로 제공.

---

## 1. 집계 주기 및 Auth 모드 분리 원칙

- **월간 결제 주기(Billing Cycle Day)**: 프로바이더별 구독 결제일 설정(기본 매월 1일) 기준으로 1개월 주기 소비액·마찰 횟수 집계.
- **과금 모드 분리 (Subscription vs API Key)**:
  - **구독 세션 (`Subscription`)**: 월 고정 요금제($P_{\text{plan}}$) 내에서 소비된 토큰을 API 단가로 환산해 **$V_{\text{api}}$(구독 환산 가치)** 에 합산.
  - **종량제 세션 (`API Key`)**: 실제 현금 과금 지출(**$C_{\text{cash}}$**)로 별도 집계해 구독 손익분기 왜곡 방지 및 하이브리드(하위 구독 + 초과분 API) 비교 근거로 활용.

---

## 2. 핵심 산출 지표 및 $V_{\text{adj}}$ 반사실적 모델

1. **API 환산 가치 (API-Equivalent Value, $V_{\text{api}}$)**:
   - 구독 세션에서 실제 사용한 모델별 Input / Output / Cache Write / Cache Read / 장문 할증(>272K) 토큰을 공식 API 단가로 환산한 주기 총액.
2. **구독 ROI 배수 (Subscription Leverage Ratio)**:
   - $R = V_{\text{api}} \div P_{\text{plan}}$ (예: $20 Pro 구독 중 API 환산 소비액이 $85이면 $R = 4.25\times$).
3. **한도 차단 마찰 횟수 (Rate-Limit Friction Count)**:
   - 해당 주기 내 5시간 블록·주간 캡·일일 요청 한도(90% 도달 또는 작업 중단) 발생 빈도.
4. **비효율 제거 후 조정 소비액 ($V_{\text{adj}}$ — 반사실적 절감 모델)**:
   - 단순 컨텍스트 방치 낭비 때문에 불필요한 상위 요금제 업그레이드를 추천하지 않도록 3대 개선 레버 절감액을 차감:
   - $$V_{\text{adj}} = \max\left(0.25\, V_{\text{api}},\; V_{\text{api}} - \left(\Delta C_{\text{bloat}} + \Delta C_{\text{cache}} + \Delta C_{\text{reasoning}}\right)\right)$$
   - **$\Delta C_{\text{bloat}}$**: [[concepts/prompt-efficiency-scoring]] 기준 `15턴` 초과 미압축 세션에 15턴 주기 `/compact`를 적용했을 때의 재전송 입력 토큰 차액 추정치.
   - **$\Delta C_{\text{cache}}$**: 캐시 적중률 `80%` 미만 장기 세션(`>= 4,096` 토큰)에 80% 캐시 히트를 가정한 차액(비-Cold-Start 턴 대상).
   - **$\Delta C_{\text{reasoning}}$**: `Thinking Overhead >= 75%` 단순 질의 턴을 동일 벤더 경량 모델(Flash/Sonnet/Luna)로 라우팅했을 때의 차액.
   - **보수적 하한(`0.25 * V_api`)**: 추정치 과대 차감으로 인한 과소 추천 방지.

---

## 3. 추천 의사결정 매트릭스

| 조건 패턴 (최근 1주기 / 30일 기준) | 권고 액션 | 근거 설명 예시 |
| :--- | :--- | :--- |
| $V_{\text{api}} < 0.7 \times P_{\text{plan}}$ AND 마찰 0~1회 | **다운그레이드 / API 종량제 전환** | "월 구독료($100) 대비 실제 API 환산 소비액 $42 — Pro($20)+API 종량제 조합 시 월 $38 절감" |
| 마찰 ≥ 월 4회 AND 상위 티어 API 크레딧 활용 가능 | **업그레이드 (예: Pro→Max / Plus→Pro 100)** | "주 2회 이상 5시간 한도 중단 발생 — Max(5x) 전환 시 크레딧 포함 손익분기 충족" |
| $V_{\text{adj}} < P_{\text{plan}} < V_{\text{api}}$ | **플랜 유지 + 효율 개선 우선** | "총 소비는 크나 미압축 컨텍스트 재전송 비중 45% — `/compact` 습관화 후 재평가 권장" |
| Gemini CLI 일일 1,000 req 빈번 도달 | **Google AI Pro/Ultra 전환 또는 API 키 폴백** | "일 1,000건 이상 요청 6일 발생 — Pro(1,500 req/day) 상향 권장" |

---

## 4. 신뢰도 가드레일

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
