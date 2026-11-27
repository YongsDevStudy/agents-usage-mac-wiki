---
id: source-local-cli-telemetry
type: source
raw_source: "raw/02-local-cli-telemetry.md"
aliases:
  - "로컬 CLI 텔레메트리 및 로그 스키마"
tags:
  - source
  - telemetry
  - logs
  - gemini-cli
  - claude-code
  - codex-cli
created: 2026-11-27
updated: 2026-11-27
sources: []
status: active
---

# Source: 로컬 CLI 텔레메트리 및 로그 스키마

- **원본**: [[raw/02-local-cli-telemetry.md]] (기준일: 2026-11-27)
- **핵심 요약**: v1 필수 타깃 CLI 3종(`Gemini CLI`, `Claude Code`, `Codex CLI`) 모두 로컬 디스크에 세션·토큰 로그를 기록하나, 벤더별 저장 경로·보존 정책·스트리밍 기록 결함이 상이해 정규화·중복제거·영구 DB 적재 설계 필수.

---

## 주요 추출 팩트

1. **[[entities/gemini-cli|Gemini CLI]] (`~/.gemini`)**:
   - 경로: `~/.gemini/tmp/<project_hash>/chats/session-<timestamp>-<uuid>.json`.
   - `input`, `output`, `cached`, `thoughts` 토큰 및 도구 실행 결과 포함.
   - `general.sessionRetention`(`maxAge: "30d"`, `maxCount: 50`)으로 과거 로그 자동 삭제 위험 → 증분 SQLite 적재 필수.
   - OpenTelemetry(`target: "local"`, `otlpEndpoint`) 지원, 단 `--telemetry-outfile`은 `EFBIG` 위험으로 비권장.
2. **[[entities/claude-code|Claude Code]] (`~/.claude`)**:
   - 경로: `~/.claude/projects/<project-path>/<session-id>.jsonl`.
   - `requestId`/`message.id` 스트리밍 청크 중복 기록 → 중복 제거 필수.
   - `cache_read_input_tokens`·`cache_creation_input_tokens`는 정확하나 `input_tokens`/`output_tokens`가 스트리밍 플레이스홀더(`0`/`1`/`2`)로 남거나 extended thinking이 누락되는 사례 존재 → 신뢰도 플래그 및 픽스처 검증 필수.
3. **[[entities/codex-cli|OpenAI Codex CLI]] (`~/.codex`)**:
   - 경로: `~/.codex/sessions/YYYY/MM/DD/rollout-<timestamp>-<id>.jsonl` (`CODEX_HOME` 지원).
   - 프롬프트·응답·쉘/패치/도구 이벤트와 `input`/`output`/`cache read`/`cache write` 토큰 기록.
4. **계측 한계**: 웹·모바일·타 기기 공유 풀 사용량은 로컬 로그만으로 관측 불가.

---

## 관련 위키 문서

- [[entities/gemini-cli|Gemini CLI]]
- [[entities/claude-code|Claude Code]]
- [[entities/codex-cli|OpenAI Codex CLI]]
- [[concepts/two-tier-telemetry-accuracy|2계층 텔레메트리 정확도 모델]]
