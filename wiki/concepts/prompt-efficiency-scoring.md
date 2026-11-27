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

## 1. 턴/세션 단위 6대 정량 지표

| 지표 | 산식 / 정의 | 정상 기준 (참고) | 비효율 경고 트리거 |
| :--- | :--- | :--- | :--- |
| **Cache Hit Rate** | `cache_read / (cache_read + cache_creation + uncached_input)` | 안정 세션 ≥ 70~85% | 프리픽스 연속 2턴 미스 또는 50% 미만 급락 |
| **Context Bloat Index** | 현재 턴 총 입력 $\div$ 세션 첫 턴 입력 | ≤ 4x | 150K~400K 초과 또는 ≥ 6x 팽창 + 15턴 이상 미압축 |
| **Effective Cost / Prompt** | 1개 사용자 질의 유발 전체 API 요청 합산 비용 | 작업 유형별 중앙값 이내 | 단일 질문당 하위 루프 ≥ 10회 또는 비용 상위 10% 이상치 |
| **Tool Output Bloat** | $\Delta \text{Context}_t$ 중 도구 응답 스파이크 비중 | 점진 증가 | 단일 턴 입력 급증 > 40K 토큰(테스트/로그 덤프 의심) |
| **Thinking Overhead** | `thinking_tokens / (output_tokens + thinking_tokens)` | ≤ 50% | 단순 질의/소규모 응답에서 ≥ 75% 초과 |
| **Base Prompt / MCP Tax** | 세션 첫 요청의 고정 입력 토큰 수 | 최소화 권장 | ≥ 25K 토큰(과다 MCP/전역 지침 점검 필요) |

---

## 2. 종합 효율 점수 (0–100점) 가중 구조 (v1 기준)

- `Efficiency Score = 0.35 * CacheScore + 0.25 * ContextHygieneScore + 0.20 * LoopEconomyScore + 0.20 * ReasoningFitScore`
- **데이터 신뢰도 예외**: Claude Code 스트리밍 플레이스홀더(`input/output <= 2`) 감지 턴은 루프·비용 점수 산정에서 분리 또는 `Low-Confidence` 표시([[entities/claude-code]] 참조).

---

## 3. 실행 가능한 코칭 액션 매핑

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
