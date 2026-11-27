# 07. 로컬 CLI 환경 실측 및 업스트림 스키마 검증 (CLI Schema Validation)

- **기준일**: 2026-11-27
- **목적**: Pre-Design Validation Gate(이슈 `#3`) 수행을 위한 로컬 머신 실측 결과 및 3사 CLI 업스트림 스키마 확정 팩트 기록
- **주요 출처 도메인**: 로컬 macOS 환경 실측, `github.com`, `claude-dev.tools`, `ccusage.com`, `openai.com`

---

## 1. 로컬 머신 실측 결과 (2026-11-27 기준)

### 1.1 설치 CLI 버전 및 바이너리 경로
| 도구 | 경로 | 확인된 버전 |
| :--- | :--- | :--- |
| **Claude Code (`claude`)** | `/Users/yong/.local/bin/claude` | `2.1.316 (Claude Code)` |
| **OpenAI Codex CLI (`codex`)** | `/Users/yong/.local/bin/codex` | `codex-cli 0.166.0` |
| **Gemini CLI (`gemini`)** | 표준 PATH 미설치 (`~/.gemini/bin/agy` Antigravity CLI 존재) | 미설치 (별도 설치 또는 실행 후 스모크 테스트 필요) |

### 1.2 비민감 설정 메타데이터 실측 (시크릿 미열람)
- **`~/.gemini/settings.json`**:
  - `security.auth.selectedType == "oauth-personal"` 확인 → P0-A의 비민감 CLI 설정 기반 Auth 모드 감지 실증 완료.
  - 기타 최상위 키: `hooks` (`BeforeTool`, `AfterTool`, `AfterAgent`), `mcpServers`.
- **`~/.claude/settings.json`**:
  - 최상위 키: `enabledPlugins`, `extraKnownMarketplaces`, `theme`, `tui`, `hooks`. (`statusLine` 미설정).
- **`~/.codex/config.toml`**:
  - 최상위 키: `model`, `model_reasoning_effort`, `approval_policy`, `sandbox_mode`, `[features] multi_agent`, `[tui]`, `[mcp_servers.*]`.

### 1.3 로컬 세션 트랜스크립트 현황 (검증 경계)
- 현재 머신에 `~/.gemini/tmp/*/chats/`, `~/.claude/projects/*/*.jsonl`, `~/.codex/sessions/` 실물 세션 로그 파일은 `0건` (`~/.claude/sessions/` 빈 디렉터리).
- 따라서 파서 인터페이스는 업스트림 공식 오픈소스 스키마를 기준으로 설계·계약화하고, 실제 CLI 1회 실행 후 생성된 실물 로그 스모크 검증 및 Apple Developer Team ID 예약은 오픈 항목으로 유지.

---

## 2. 업스트림 오픈소스 스키마 확정 팩트

### 2.1 Gemini CLI (`google-gemini/gemini-cli`)
- **경로·포맷**: `~/.gemini/tmp/<project_hash>/chats/session-<timestamp>-<shortId>.jsonl` (최신 JSONL 전환, 구버전 `.json` 폴백 유지 필요).
- **JSONL 라인 구조**:
  - Line 1 (`SessionMetadataRecord`): `sessionId` (UUID), `projectHash` (SHA-256 hex of absolute project path; 신규 버전 슬러그 레지스트리 폴백 병행), `startTime`, `lastUpdated`, `kind` (`"main" | "subagent"`), `summary`(선택).
  - 후속 라인: 메시지 턴(`user`, `gemini`/`model`) 또는 부분 패치 블록(`{"$set": {"lastUpdated": "..."}}`).
- **`TokensSummary` 필드**: `input`, `output`, `cached`, `thoughts`, `tool`, `total`.

### 2.2 Claude Code (`~/.claude/projects/<encoded-path>/<session-id>.jsonl`)
- **핵심 구분 필드**:
  - `isSidechain: true` (및 `subagents/`): 서브에이전트/포크 스레드 → `System & Subagent Overhead`로 분리(중복 합산 방지).
  - `isCompactSummary: true`: 컨텍스트 압축(`/compact` 또는 auto-compact) 이후 생성된 합성 요약 레코드(`T = 0` 명시적 리셋 트리거).
  - `tool_result`: `type: "user"` 엔트리의 `message.content` 배열 내 `{"type": "tool_result", "tool_use_id": ...}` 블록 → `Agent Loop Step`으로 분류(`User Prompt Turn` 분모 제외).
  - `requestId` (`req_...`): 스트리밍 청크 중복 제거 기준 키. `message.usage` 내 `cache_creation_input_tokens`, `cache_read_input_tokens`.

### 2.3 OpenAI Codex CLI (`~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl`)
- **핵심 이벤트 타입**:
  - `type == "session_meta"`: `payload.id`, `payload.timestamp`, `payload.originator` (`cli | app | sdk`), `payload.model`, `payload.cwd`.
  - `type == "event_msg"` + `payload.type == "token_count"`:
    - `info.total_token_usage`: 세션 시작 이후 **누적 합계** (행 합산 시 심각한 이중 집계 발생 — 절대 합산 금지).
    - `info.last_token_usage`: **직전 단일 API 호출 증분치**(`input_tokens`, `cached_input_tokens`, `output_tokens`, `reasoning_output_tokens`, `total_tokens`) → 파서 기본 수집 대상.
    - `info.model_context_window`: 활성 모델 컨텍스트 상한(예: `272000`).
    - `payload.rate_limits`: 로그 내 플랫폼 레이트 리밋 스냅샷 객체(`primary`, `secondary`) — 기록 시 무인증·ToS 준수 방식으로 기회적 활용 가능.
