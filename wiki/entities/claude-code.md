---
id: entity-claude-code
type: entity
aliases:
  - "Claude Code"
  - "~/.claude"
tags:
  - entity
  - cli-agent
  - anthropic-claude
  - required
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/local-cli-telemetry]]"
  - "[[sources/pricing-quotas-2026]]"
  - "[[sources/prompt-efficiency-metrics]]"
status: active
---

# Claude Code (`~/.claude`)

- **역할**: 본 프로젝트 v1 MVP **필수 지원** 로컬 코딩 에이전트 CLI.

---

## 1. 로컬 로그 스키마 및 파싱 함정

- **세션 로그 경로**: `~/.claude/projects/<project-path>/<session-id>.jsonl`.
- **레코드 구조**: `type == "assistant"` 라인의 `message.usage` 객체 내 `input_tokens`, `output_tokens`, `cache_creation_input_tokens`, `cache_read_input_tokens`.
- **파서 필수 해결 과제 2건**:
  1. **스트리밍 청크 중복 제거**: 동일 `requestId` / `message.id`가 스트리밍 진행 중 여러 줄로 중복 기록 → `requestId` 기준 최종 완료 레코드만 유지.
  2. **플레이스홀더 왜곡 보정/플래깅**: 일부 버전/스트리밍 시점에서 캐시 필드(`cache_read_input_tokens`, `cache_creation_input_tokens`)는 정확하나 `input_tokens`/`output_tokens`가 `0`, `1`, `2` 등 초기 플레이스홀더로 남거나 extended thinking 토큰이 누락되는 현상 보고 → 이상치 감지 시 Low-Confidence 배지 부여 및 실제 로컬 픽스처 기반 검증 필수.

---

## 2. 5시간 롤링 블록 및 요금제 특성

- **5시간 빌링 블록**: `ccusage blocks` 모델과 동일하게 최초 요청 시점부터 5시간 롤링 윈도우를 로컬에서 재구성해 한도 소진율·번레이트·리셋 카운트다운 추정.
- **공유 풀 제약**: Claude Pro($20), Max 5x($100, +$100 API 크레딧), Ultra($200, +$200 API 크레딧)는 웹(`claude.ai`)·데스크톱·모바일과 Claude Code가 동일 한도 풀 공유([[concepts/two-tier-telemetry-accuracy]] 참조).
- **컨텍스트 위생 지표**: `/compact`, `/clear`, `autoCompactWindow` 설정 여부와 직결되며, Cache Write(`1.25x`/`2.00x`) 대비 Cache Read(`0.025x`~`0.10x`, ITPM 미산입) 비중 극대화가 핵심([[concepts/prompt-efficiency-scoring]] 참조).

---

## 관련 문서

- [[sources/local-cli-telemetry|로컬 CLI 텔레메트리 요약]]
- [[sources/pricing-quotas-2026|2026년 11월 요금제·단가 요약]]
- [[entities/gemini-cli|Gemini CLI]]
- [[entities/codex-cli|OpenAI Codex CLI]]
- [[concepts/plan-roi-recommendation|요금제 ROI 추천 엔진]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
