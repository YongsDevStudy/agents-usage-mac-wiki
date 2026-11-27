---
id: concept-prompt-efficiency-scoring
type: concept
aliases:
  - "프롬프트 효율 점수 모델"
tags:
  - concept
  - efficiency
  - heuristics
  - dashboard
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/prompt-efficiency-metrics]]"
  - "[[sources/pricing-quotas-2026]]"
status: active
---

# 프롬프트 효율 점수 모델 (Prompt Efficiency Scoring)

- **원칙**: v1에서 외부 LLM 평가 API 호출 없이 **100% 로컬 로그 기반 결정적 통계·규칙(Deterministic Heuristics)**만으로 질문(턴)과 세션의 토큰 효율 진단.

---

## 1. 로그 이벤트 3분류 정의 (Turn Taxonomy)

API 스펙상 도구 응답(`tool_result`)도 `role: "user"`로 기록되는 혼선을 방지하기 위해 모든 로그 레코드를 아래 3계층으로 엄격 분리:

| 분류 | 식별 기준 | 집계 귀속 |
| :--- | :--- | :--- |
| **`User Prompt Turn`** | 실제 사용자 텍스트 입력이 포함된 턴(도구 결과·시스템 주입 제외) | 질문당 비용·루프 배수 산출의 기본 분모 |
| **`Agent Loop Step`** | 직전 `User Prompt Turn`에 종속된 도구 호출(`tool_use`/`tool_result`) 및 재시도 응답 | 상위 `User Prompt Turn` 1건의 실질 비용에 합산 |
| **`System & Subagent Overhead`** | `/compact` 요약 호출, 서브에이전트 위임 스레드, 자동 시스템 메타 호출 | 질문 단위 루프 배수에서 분리, 세션 오버헤드 항목으로 별도 집계 |

---

## 2. 턴/세션 단위 6대 정량 지표

| 지표 | 산식 / 정의 | 정상 기준 (참고) | 비효율 경고 트리거 |
| :--- | :--- | :--- | :--- |
| **Cache Hit Rate** | `cache_read / (cache_read + cache_creation + uncached_input)` | 안정 세션 ≥ 70~85% | 프리픽스 연속 2턴 미스 또는 50% 미만 급락 |
| **Context Bloat Index** | 현재 턴 총 입력 $\div$ 세션 첫 턴 입력 | ≤ 4x | 150K~400K 초과 또는 ≥ 6x 팽창 + 15턴 이상 미압축 |
| **Effective Cost / Prompt** | 1개 `User Prompt Turn` 유발 `Agent Loop Step` 합산 비용 | 작업 유형별 중앙값 이내 | 단일 질문당 하위 루프 ≥ 10회 또는 비용 상위 10% 이상치 |
| **Tool Output Bloat** | $\Delta \text{Context}_t$ 중 도구 응답 스파이크 비중 | 점진 증가 | 단일 턴 입력 급증 > 40K 토큰(테스트/로그 덤프 의심) |
| **Thinking Overhead** | `thinking_tokens / (output_tokens + thinking_tokens)` | ≤ 50% | 단순 질의/소규모 응답에서 ≥ 75% 초과 |
| **Base Prompt / MCP Tax** | 세션 첫 요청의 고정 입력 토큰 수 | 최소화 권장 | ≥ 25K 토큰(과다 MCP/전역 지침 점검 필요) |

---

## 3. 종합 효율 점수 (0–100점) 정규화 및 예외 규칙

### 3.1 기본 가중 산식
- `Efficiency Score = 0.35 * CacheScore + 0.25 * ContextHygieneScore + 0.20 * LoopEconomyScore + 0.20 * ReasoningFitScore`

### 3.2 서브스코어 구간 선형 정규화 표

| 서브스코어 (가중치) | 측정 입력 | 100점 기준 | 선형 감점 구간 | 0점 하한 |
| :--- | :--- | :--- | :--- | :--- |
| **`CacheScore` (35%)** | Cache Hit Rate ($H$) | $H \ge 85\%$ | $30\% < H < 85\%$ 구간 선형 비례 | $H \le 30\%$ |
| **`ContextHygieneScore` (25%)** | Bloat 배수($B$) & 미압축 턴 수($T$) | $B \le 3\times$ AND $T \le 10$ | $3\times < B < 8\times$ 또는 $10 < T < 25$ 감점 | $B \ge 8\times$ 또는 $T \ge 25$ |
| **`LoopEconomyScore` (20%)** | `User Prompt Turn`당 `Agent Loop Step` 수($L$) | $L \le 3$ | $3 < L < 12$ 구간 선형 감점 | $L \ge 12$ |
| **`ReasoningFitScore` (20%)** | Thinking Overhead ($R_{\text{think}}$) | $R_{\text{think}} \le 40\%$ | $40\% < R_{\text{think}} < 85\%$ 감점 (단순 작업 기준) | $R_{\text{think}} \ge 85\%$ (짧은 출력 대비 과다 추론) |

### 3.3 구조적 예외 처리 (Cold-Start & 모델 특성 보정)
1. **Cold Start (세션 1~2번째 `User Prompt Turn`)**: 캐시 워밍업 구간이므로 종합 점수를 산출하지 않고 **`Warming Up (점수 보류)`** 상태로 표기(초기 턴 부당 감점 방지).
2. **Gemini 4 `< 4,096` 토큰 프리픽스**: Implicit Caching 최소 발동 임계치(`4,096` 토큰) 미만 턴은 `CacheScore` 평가에서 제외하고 나머지 활성 지표로 가중치 재정규화.
3. **Thinking 미지원 모델 / 누락 로그**: `thinking_tokens`가 모델 사양상 미지원이거나 로그에 미기록된 경우 `ReasoningFitScore`를 제외하고 나머지 3개 서브스코어 가중치를 100%로 재정규화 (`CacheScore 43.75% + ContextHygieneScore 31.25% + LoopEconomyScore 25.00%`).
4. **Low-Confidence 턴**: Claude Code 스트리밍 플레이스홀더(`input/output <= 2`) 감지 턴은 루프·비용 점수 계산에서 분리 표기([[entities/claude-code]] 참조).

---

## 4. 실행 가능한 코칭 액션 매핑

- **Context Bloat 경보**: "`/compact` 권장 — 최근 12턴 동안 동일 누적 컨텍스트 재전송으로 추정 $X.XX 추가 소모".
- **세션 주제 전환 의심 / 장기 팽창**: "`/clear` 권장 — 신규 작업은 새 세션에서 시작해 기본 입력 세금 절감".
- **Cache Miss 급증**: "세션 중 전역 규칙 또는 MCP 구성 변경 감지 — 프리픽스 캐시 재구축 비용 발생".
- **Tool Output 스파이크**: "테스트·빌드 출력 필터링(`--quiet`, 에러 라인 제한) 및 서브에이전트 위임 권장".
- **Thinking Overhead 과다**: "단순 작업에 고비용 추론 모델 사용 중 — Flash/Sonnet/Luna 티어 전환 시 약 XX% 절감 가능".

---

## 관련 문서

- [[sources/prompt-efficiency-metrics|프롬프트 토큰 효율 지표 요약]]
- [[entities/gemini-cli|Gemini CLI]]
- [[entities/claude-code|Claude Code]]
- [[concepts/plan-roi-recommendation|요금제 ROI 추천 엔진]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
