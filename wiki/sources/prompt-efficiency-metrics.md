---
id: source-prompt-efficiency-metrics
type: source
raw_source: "raw/04-prompt-efficiency-metrics.md"
aliases:
  - "프롬프트 토큰 효율 지표"
tags:
  - source
  - efficiency
  - context-engineering
  - caching
created: 2026-11-27
updated: 2026-11-27
sources: []
status: active
---

# Source: 프롬프트 토큰 효율 및 컨텍스트 엔지니어링 지표

- **원본**: [[raw/04-prompt-efficiency-metrics.md]] (기준일: 2026-11-27)
- **핵심 요약**: LLM의 무상태 재전송 특성으로 인해 세션 장기화 시 비용이 $O(N^2)$ 규모로 팽창하며, 원문 외부 전송 없이 로컬 토큰·이벤트 로그만으로 6대 효율 지표 및 `/compact`·`/clear` 코칭 규칙 산출 가능.

---

## 주요 추출 팩트

1. **핵심 정량 지표 6종**:
   - **Cache Hit Rate**: `cache_read / (cache_read + cache_creation + uncached_input)`.
   - **Context Bloat Index**: 초기 턴 대비 입력 토큰 팽창 배수 및 턴당 증가 기울기.
   - **Effective Cost per User Prompt**: 1개 사용자 질의가 유발한 하위 도구 호출·재시도 루프 합산 비용 및 루프 배수.
   - **Tool Output Bloat**: 단일 도구 응답이 입력 증가분에서 차지하는 급등 스파이크.
   - **Thinking Overhead Ratio**: `thinking_tokens / (output_tokens + thinking_tokens)`.
   - **Base Prompt / MCP Tax**: 세션 첫 턴 고정 시스템·MCP 스키마 토큰 크기.
2. **실행 가능한 개선 레버**:
   - 탐색 후 구현 전 `/compact`, 신규 작업 시 `/clear`, `autoCompactWindow` 하향 조정(예: 400K).
   - 미사용 MCP 정리, `--quiet` 테스트 출력 필터링, 단순 작업 경량 모델 라우팅.

---

## 관련 위키 문서

- [[concepts/prompt-efficiency-scoring|프롬프트 효율 점수 모델]]
- [[entities/gemini-cli|Gemini CLI]]
- [[entities/claude-code|Claude Code]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
