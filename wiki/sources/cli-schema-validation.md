---
id: source-cli-schema-validation
type: source
raw_source: "raw/07-cli-schema-validation.md"
aliases:
  - "CLI 스키마 및 로컬 환경 검증"
tags:
  - source
  - schema
  - validation
  - gemini-cli
  - claude-code
  - codex-cli
created: 2026-11-27
updated: 2026-11-27
sources: []
status: active
---

# Source: 로컬 CLI 환경 실측 및 업스트림 스키마 검증

- **원본**: [[raw/07-cli-schema-validation.md]] (기준일: 2026-11-27)
- **핵심 요약**: 현재 머신의 설치 CLI(`claude 2.1.316`, `codex-cli 0.166.0`) 및 비민감 설정(`~/.gemini/settings.json` `security.auth.selectedType = "oauth-personal"`)을 실측하고, 로컬 세션 파일 0건 구간을 보완하기 위해 3사 업스트림 오픈소스 스키마 계약을 확정.

---

## 주요 추출 팩트

1. **로컬 실측 결과**:
   - `claude 2.1.316`, `codex-cli 0.166.0` 확인 (`gemini` PATH 미설치, `~/.gemini/settings.json` `security.auth.selectedType` 확인).
   - 로컬 세션 트랜스크립트(`chats/`, `projects/*.jsonl`, `sessions/*.jsonl`) 현재 0건 → 1회 실행 스모크 검증 항목을 오픈 상태로 유지.
2. **파서 계약 결정 사항**:
   - **[[entities/gemini-cli|Gemini CLI]]**: 최신 `.jsonl`(`SessionMetadataRecord` + `$set` 패치 + `kind: main|subagent` + `TokensSummary`) 및 구버전 `.json` 양방향 지원, SHA-256 `projectHash`.
   - **[[entities/claude-code|Claude Code]]**: `isSidechain`, `isCompactSummary`, `message.content[].type == "tool_result"`, `requestId` 중복 제거.
   - **[[entities/codex-cli|OpenAI Codex CLI]]**: `info.last_token_usage`만 증분 수집(`total_token_usage` 합산 금지), `model_context_window` 및 `payload.rate_limits` 기회적 수집.

---

## 관련 위키 문서

- [[concepts/adr-core-decisions|핵심 아키텍처 의사결정 (ADR)]]
- [[syntheses/tdd-agents-usage-mac|기술 설계서 (TDD v1.0)]]
- [[entities/gemini-cli|Gemini CLI]]
- [[entities/claude-code|Claude Code]]
- [[entities/codex-cli|OpenAI Codex CLI]]
