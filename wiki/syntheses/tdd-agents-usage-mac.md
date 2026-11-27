---
id: tdd-agents-usage-mac
type: synthesis
aliases:
  - "Agents Usage Mac TDD"
  - "기술 설계서 v1.0"
tags:
  - synthesis
  - tdd
  - architecture
  - sqlite
  - swift
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/cli-schema-validation]]"
  - "[[sources/local-cli-telemetry]]"
  - "[[sources/pricing-quotas-2026]]"
  - "[[sources/macos-architecture-constraints]]"
  - "[[sources/oauth-tos-constraints]]"
status: active
---

# 기술 설계서 (TDD v1.0): Agents Usage Mac

- **문서 버전**: v1.0 (Design-Ready PRD v1.2 및 [[concepts/adr-core-decisions|ADR-001~005]] 구현 명세, 기준일 `2026-11-27`)
- **대상 구현 저장소**: `YongsDevStudy/agents-usage-mac`
- **상위 기획서**: [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD v1.2)]]

---

## 1. SwiftPM 4-모듈 아키텍처 및 디렉터리 레이아웃

```text
agents-usage-mac/
├── Package.swift
├── Sources/
│   ├── AgentsCore/                  # 순수 비즈니스 로직·저장소 (macOS UI 비의존, swift test 대상)
│   │   ├── Models/                  # AgentKind, TurnClassification, AuthMode, ConfidenceFlag
│   │   ├── Storage/                 # GRDB DatabasePool, Migrations (v1 DDL), Re-pricing
│   │   ├── Pricing/                 # PricingCatalogLoader, CostCalculator, Opt-in Sync
│   │   ├── Analytics/               # EfficiencyScorer, CompactionDetector, AdaptiveBaseline, ROIEngine
│   │   └── Snapshot/                # WidgetSnapshotPayload, WidgetReloadBudgetGuard
│   ├── AgentsParsers/               # CLI 로그 파서 어댑터 (swift test 골든 픽스처 대상)
│   │   ├── CLIParserProtocol.swift
│   │   ├── GeminiCLIParser.swift    # .jsonl ($set 패치 포함) + 구버전 .json + SHA-256 projectHash
│   │   ├── ClaudeCodeParser.swift   # requestId 병합, isSidechain, isCompactSummary, tool_result
│   │   └── CodexCLIParser.swift     # session_meta, last_token_usage, rate_limits
│   ├── AgentsApp/                   # macOS SwiftUI + FSEvents + SMAppService 메인 앱 타깃
│   │   ├── Services/                # IngestionCoordinator (actor), FSEventsWatcher, SleepWakeObserver
│   │   ├── MenuBar/                 # MenuBarExtra, Notch-Safe Label, PopoverView
│   │   └── Dashboard/               # Overview, PromptEfficiency, Sessions, Settings (EN/KO .xcstrings)
│   └── AgentsWidget/                # WidgetKit Extension (Sandboxed, App Group 읽기 전용)
│       ├── AgentsTimelineProvider.swift
│       └── Views/                   # SmallWidgetView, MediumWidgetView, LargeWidgetView
└── Tests/
    ├── AgentsCoreTests/
    └── AgentsParsersTests/          # Golden Fixtures (익명화 12종)
```

![[visuals/sqlite-erd-and-modules.excalidraw]]

---

## 2. SQLite (GRDB WAL) 물리 스키마 및 수명주기

### 2.1 핵심 테이블 DDL (9 Tables)

```sql
PRAGMA foreign_keys = ON;

CREATE TABLE ingestion_cursors (
    file_path       TEXT PRIMARY KEY,
    agent           TEXT NOT NULL CHECK (agent IN ('gemini', 'claude', 'codex')),
    parser_mode     TEXT NOT NULL CHECK (parser_mode IN ('byte_offset', 'snapshot_delta')),
    byte_offset     INTEGER NOT NULL DEFAULT 0,
    last_mtime_ns   INTEGER NOT NULL DEFAULT 0,
    last_file_size  INTEGER NOT NULL DEFAULT 0,
    last_turn_count INTEGER NOT NULL DEFAULT 0,
    file_hash       TEXT,
    updated_at      TEXT NOT NULL
);

CREATE TABLE projects (
    id              TEXT PRIMARY KEY,          -- <agent>:<canonical_key>
    agent           TEXT NOT NULL,
    project_hash    TEXT,                      -- Gemini SHA-256 hex (nullable)
    resolved_path   TEXT,
    display_name    TEXT NOT NULL,
    alias_source    TEXT NOT NULL CHECK (alias_source IN ('cli_meta', 'workspace_hash', 'manual', 'hash_fallback')),
    created_at      TEXT NOT NULL
);

CREATE TABLE sessions (
    id              TEXT PRIMARY KEY,          -- <agent>:<session_uuid>
    agent           TEXT NOT NULL,
    project_id      TEXT REFERENCES projects(id) ON DELETE CASCADE,
    source_file     TEXT NOT NULL,
    auth_mode       TEXT NOT NULL CHECK (auth_mode IN ('subscription', 'api_key', 'unknown')),
    auth_source     TEXT NOT NULL CHECK (auth_source IN ('cli_config', 'default_mode', 'manual')),
    kind            TEXT NOT NULL DEFAULT 'main' CHECK (kind IN ('main', 'subagent')),
    started_at      TEXT NOT NULL,
    last_activity_at TEXT NOT NULL,
    is_archived     INTEGER NOT NULL DEFAULT 0 -- 원본 retention 삭제 후에도 1로 보존
);

CREATE TABLE user_turns (
    id                  TEXT PRIMARY KEY,      -- <session_id>:turn:<idx>
    session_id          TEXT NOT NULL REFERENCES sessions(id) ON DELETE CASCADE,
    turn_index          INTEGER NOT NULL,
    started_at          TEXT NOT NULL,
    model_primary       TEXT,
    prompt_byte_len     INTEGER NOT NULL DEFAULT 0, -- 원문 0바이트 저장 원칙
    compaction_detected INTEGER NOT NULL DEFAULT 0, -- isCompactSummary 또는 40% 급감
    uncompacted_streak  INTEGER NOT NULL DEFAULT 0,
    context_bloat_ratio REAL,
    cache_hit_rate      REAL,
    efficiency_score    REAL,                  -- Cold-Start(1~2턴) 시 NULL (Warming Up)
    confidence_flag     TEXT NOT NULL DEFAULT 'exact' CHECK (confidence_flag IN ('exact', 'low_confidence'))
);

CREATE TABLE api_requests (
    id                  TEXT PRIMARY KEY,      -- <agent>:<dedup_request_id>
    session_id          TEXT NOT NULL REFERENCES sessions(id) ON DELETE CASCADE,
    user_turn_id        TEXT REFERENCES user_turns(id) ON DELETE CASCADE,
    turn_class          TEXT NOT NULL CHECK (turn_class IN ('user_prompt', 'agent_loop', 'system_overhead')),
    model               TEXT NOT NULL,
    input_tokens        INTEGER NOT NULL DEFAULT 0,
    output_tokens       INTEGER NOT NULL DEFAULT 0,
    cache_write_tokens  INTEGER NOT NULL DEFAULT 0,
    cache_read_tokens   INTEGER NOT NULL DEFAULT 0,
    thinking_tokens     INTEGER NOT NULL DEFAULT 0,
    tool_tokens         INTEGER NOT NULL DEFAULT 0,
    context_window_cap  INTEGER,               -- Codex model_context_window 등
    estimated_cost_usd  REAL NOT NULL DEFAULT 0.0,
    pricing_version     TEXT NOT NULL,
    is_unlisted_model   INTEGER NOT NULL DEFAULT 0,
    confidence_flag     TEXT NOT NULL DEFAULT 'exact' CHECK (confidence_flag IN ('exact', 'low_confidence')),
    timestamp           TEXT NOT NULL
);

CREATE TABLE daily_rollups (
    bucket_date         TEXT NOT NULL,         -- YYYY-MM-DD (Gemini PT 자정 / 로컬 토글 반영 전 UTC 또는 dual key)
    timezone_mode       TEXT NOT NULL CHECK (timezone_mode IN ('vendor_pt', 'local')),
    agent               TEXT NOT NULL,
    project_id          TEXT NOT NULL,
    auth_mode           TEXT NOT NULL,
    model               TEXT NOT NULL,
    api_request_count   INTEGER NOT NULL DEFAULT 0, -- Gemini 일일 쿼터 분자
    user_turn_count     INTEGER NOT NULL DEFAULT 0, -- 효율 분석 분모
    input_tokens        INTEGER NOT NULL DEFAULT 0,
    output_tokens       INTEGER NOT NULL DEFAULT 0,
    cache_write_tokens  INTEGER NOT NULL DEFAULT 0,
    cache_read_tokens   INTEGER NOT NULL DEFAULT 0,
    thinking_tokens     INTEGER NOT NULL DEFAULT 0,
    estimated_cost_usd  REAL NOT NULL DEFAULT 0.0,
    PRIMARY KEY (bucket_date, timezone_mode, agent, project_id, auth_mode, model)
);

CREATE TABLE rate_limit_events (
    id                  TEXT PRIMARY KEY,
    agent               TEXT NOT NULL,
    source              TEXT NOT NULL CHECK (source IN ('codex_log', 'statusline_v2', 'http_429', 'manual')),
    window_kind         TEXT NOT NULL CHECK (window_kind IN ('five_hour', 'seven_day', 'daily')),
    remaining_pct       REAL,
    reset_at            TEXT,
    observed_at         TEXT NOT NULL
);

CREATE TABLE ingest_errors (
    file_path           TEXT PRIMARY KEY,
    agent               TEXT NOT NULL,
    error_message       TEXT NOT NULL,
    is_ignored          INTEGER NOT NULL DEFAULT 0,
    last_failed_at      TEXT NOT NULL
);

CREATE TABLE app_metadata (
    key                 TEXT PRIMARY KEY,
    value               TEXT NOT NULL
);

CREATE INDEX idx_api_requests_session_ts ON api_requests(session_id, timestamp);
CREATE INDEX idx_api_requests_ts ON api_requests(timestamp);
CREATE INDEX idx_user_turns_session_idx ON user_turns(session_id, turn_index);
CREATE INDEX idx_sessions_last_act ON sessions(last_activity_at DESC);
```

### 2.2 Re-pricing 및 데이터 삭제권 설계
- **Re-pricing 파이프라인 (ADR-003)**: 단가 카탈로그 버전 상향 또는 사용자 `pricing-overrides.json` 저장 시 백그라운드 트랜잭션으로 `api_requests.pricing_version != currentVersion` 또는 `is_unlisted_model = 1` 레코드를 원시 토큰 컬럼 기준 재계산 후 영향을 받은 `daily_rollups`만 재집계.
- **삭제권 (P0-C)**:
  - 특정 프로젝트/세션 삭제: `ON DELETE CASCADE`로 `sessions` → `user_turns` → `api_requests` 자동 연쇄 삭제 후 해당 `daily_rollups` 재계산 + 스냅샷 갱신.
  - 전체 초기화(Factory Reset): WAL 체크포인트 후 모든 테이블 TRUNCATE + `widget-snapshot.json` 초기화.

---

## 3. CLI 파서 어댑터 계약 (`CLIParserProtocol`)

### 3.1 공통 프로토콜
```swift
public protocol CLIParserProtocol: Sendable {
    var agent: AgentKind { get }
    var parserMode: ParserMode { get } // .byteOffset or .snapshotDelta
    func parseIncremental(fileURL: URL, cursor: IngestionCursor?) throws -> ParsedLogDelta
}
```
- **온디맨드 미리보기 추출기 (`OnDemandPromptReader`)**: DB 저장 없이 UI가 특정 `session_id` + `turn_index` 요청 시 `source_file`이 디스크에 존재하면 스트리밍으로 해당 턴 첫 80자(`String(prefix(80))`)만 메모리 반환 후 즉시 버림.

### 3.2 벤더별 파싱 규칙 ([[sources/cli-schema-validation]] 반영)
1. **`GeminiCLIParser`**:
   - 최신 `.jsonl`: Line 1 `SessionMetadataRecord`에서 `sessionId`, `projectHash`, `kind`(`main`/`subagent`) 추출, 후속 라인의 `{"$set": ...}` 패치는 메타데이터 갱신으로 처리하고 메시지 턴만 `ParsedTurn`으로 적재.
   - 구버전 `.json`: `messages` 배열 전체 파싱 후 `cursor.lastTurnCount` 이후 인덱스만 증분 적재.
   - `TokensSummary` 매핑: `input`, `output`, `cached` → `cache_read_tokens`, `thoughts` → `thinking_tokens`, `tool` → `tool_tokens`.
   - `projectHash` 역매핑: 사용자 지정 워크스페이스 루트(예: `~/Projects`) 하위 폴더의 `SHA256(absPath).hex` 계산 매칭(TCC 거부 시 건너뛰고 `hash_fallback`→수동 별칭 폴백).
2. **`ClaudeCodeParser`**:
   - `requestId` 기준 그룹화: 스트리밍 청크 중 `stop_reason != nil` 또는 최대 `output_tokens` 보유 최종 레코드로 `api_requests` 멱등 업서트.
   - `isSidechain == true` 또는 `subagents/` 경로 → `turn_class = 'system_overhead'`.
   - `type == "user"`: `message.content`에 `type == "tool_result"` 포함 시 `Agent Loop Step`으로 귀속(새 `User Prompt Turn` 생성 금지); `isCompactSummary == true`는 `compaction_detected = 1` + $T=0$ 리셋.
   - 플레이스홀더 가드: 본문 길이가 존재하나 `input_tokens <= 2 && output_tokens <= 2`인 경우 `confidence_flag = 'low_confidence'`.
3. **`CodexCLIParser`**:
   - `type == "session_meta"` → `session.id`, `cwd`(프로젝트 경로), `model`.
   - `type == "event_msg"` & `payload.type == "token_count"`:
     - **`info.last_token_usage`만 수집**(`info.total_token_usage` 합산 절대 금지).
     - `cached_input_tokens` → `cache_read_tokens`, `reasoning_output_tokens` → `thinking_tokens`.
     - `payload.rate_limits` 비어 있지 않으면 `rate_limit_events (source = 'codex_log')`로 기회적 기록.

---

## 4. 동시성, FSEvents, Sleep/Wake 및 App Group 브리지

### 4.1 `actor IngestionCoordinator`
- `FSEventsWatcher`가 감시 경로(`~/.gemini/tmp`, `~/.claude/projects`, `~/.codex/sessions` + 수동 커스텀 경로) 이벤트를 수집해 파일 단위 **1.5초 디바운서** 통과 후 `IngestionCoordinator.enqueue(urls:)` 호출.
- `NSWorkspace.didWakeNotification` 및 앱 초기 구동 시 `runCatchUpScan()` 실행(누락 파일 델타 스캔 + 5시간 블록 타이머 재계산).

### 4.2 App Group `widget-snapshot.json` & 리로드 예산 제어
- 경로: `FileManager.default.containerURL(forSecurityApplicationGroupIdentifier: "group.dev.yongsdevstudy.agents-usage-mac")!.appendingPathComponent("widget-snapshot.json")`
- **스냅샷 계약**:
  - `generatedAt`, `primaryAgent`, `agents: [AgentWidgetSummary]`(Gemini `apiRequestsToday`, `dailyLimit`, `resetAt`, `todayCostUsd`; Claude/Codex `blockCostUsd`, `blockResetAt`, `burnRatePerHour`, `peakRatio`, `estimatedPct?`), `activeAlertBadge`, `sevenDayCostsUsd`. (프롬프트 텍스트 필드 없음).
- **`WidgetReloadBudgetGuard`**:
  - 최소 쿨다운 `900초(15분)` 유지 + 일일 리로드 횟수(`app_metadata.widget_reloads_today`) `48회` 소프트 캡 관리(단, 한도 `70%`/`90%` 신규 임계치 돌파 시 1회 예외 즉시 리로드).

---

## 5. 골든 픽스처 테스트 전략 (`AgentsParsersTests` & `AgentsCoreTests`)

1. **Gemini `.jsonl` `$set` 패치 + 구버전 `.json` 델타 테스트**: 중복 없이 신규 턴만 적재 확인.
2. **Gemini SHA-256 `projectHash` + TCC 거부 폴백 테스트**.
3. **Gemini 일일 `API Request` 총합 vs `User Prompt Turn` 분리 및 PT 자정 리셋 경계 테스트**.
4. **Claude `requestId` 5중 중복 제거 + 플레이스홀더 `low_confidence` 플래그 테스트**.
5. **Claude `isSidechain` + `tool_result` + `isCompactSummary` 3분류 테스트**.
6. **Codex `last_token_usage` vs `total_token_usage` 비합산 + `rate_limits` 캡처 테스트**.
7. **Cold-Start(1~2턴 `Warming Up`), `<4,096` 캐시 예외, Thinking 미지원 가중치 재정규화(`43.75/31.25/25.00`) 테스트**.
8. **입력 `40%` 이상 급감 `/compact` 자동 감지(`T=0` 리셋) 테스트**.
9. **단가표 `effective_from`(`2026-12-01`) + Re-pricing 배치 재계산 테스트**.
10. **프로젝트/세션 Cascade 삭제 및 Factory Reset 무결성 테스트**.
11. **`WidgetReloadBudgetGuard` 15분 쿨다운 및 48회 캡 테스트**.
12. **`ccusage` JSON 출력 대비 토큰 합계 1% 이내 교차 검증 테스트**.

---

## 관련 문서

- [[sources/cli-schema-validation|로컬 CLI 환경·스키마 검증 요약]]
- [[concepts/adr-core-decisions|핵심 아키텍처 의사결정 (ADR)]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD v1.2)]]
- [[concepts/prompt-efficiency-scoring|프롬프트 효율 점수 모델]]
- [[concepts/macos-menubar-widget-architecture|macOS 메뉴 막대·위젯 아키텍처]]
