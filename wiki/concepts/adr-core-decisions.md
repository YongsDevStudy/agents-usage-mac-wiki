---
id: concept-adr-core-decisions
type: concept
aliases:
  - "핵심 아키텍처 의사결정 (ADR)"
  - "ADR-001..005"
tags:
  - concept
  - adr
  - architecture
  - swift
  - sqlite
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/macos-architecture-constraints]]"
  - "[[sources/cli-schema-validation]]"
  - "[[sources/oauth-tos-constraints]]"
status: active
---

# 핵심 아키텍처 의사결정 기록 (ADR-001 ~ ADR-005)

- **목적**: [[syntheses/prd-agents-usage-mac|PRD v1.2]]를 실제 Swift/macOS 코드로 구현하기 전, 구조적 변경 비용이 큰 5대 핵심 설계 결정을 확정.

---

## ADR-001: 비샌드박스 메인 앱 + 샌드박스 WidgetKit App Group 브리지

- **컨텍스트**: 메인 앱은 `~/.gemini`, `~/.claude`, `~/.codex`를 `FSEvents`로 무마찰 감시해야 하나, `WidgetKit` 익스텐션(`.appex`)은 macOS 정책상 항상 샌드박스로 실행.
- **결정**: 메인 앱(`AgentsUsage.app`)은 **Non-Sandboxed + Hardened Runtime**으로 배포(DMG + Sparkle 2 + Homebrew Cask)하고, 메인 앱과 위젯 타깃 모두에 동일 **App Group (`group.dev.yongsdevstudy.agents-usage-mac`)** 을 부여해 경량 `widget-snapshot.json`으로 상태 공유.
- **검토 대안**: Mac App Store 샌드박스 앱(매 경로 `NSOpenPanel` 권한 요청 및 하위 프로세스 제약으로 기각).
- **결과**: Apple Developer ID 서명·공증 및 App Group Provisioning Profile 필수; 위젯은 DB 직접 열기 대신 원자적 스냅샷 JSON만 읽기 전용 조회.

---

## ADR-002: 로컬 저장소 — SQLite + GRDB.swift (WAL DatabasePool)

- **컨텍스트**: 100% 영구 보관, `requestId` 멱등 업서트, `daily_rollups` 집계, 단가표 변경 시 일괄 Re-pricing, 백그라운드 쓰기 중 UI 동시 읽기 필요.
- **결정**: **SQLite + GRDB.swift (`DatabasePool` WAL 모드)** 채택.
- **검토 대안**:
  - *SwiftData / CoreData*: 스키마 마이그레이션·복잡한 집계 쿼리·단가 Re-pricing 배치 UPDATE 성능/제어력 부족으로 기각.
  - *Raw JSON 파일 캐시*: 수천 개 세션 시계열 집계 및 삭제/롤업 무결성 보장 불가로 기각.
- **결과**: 단일 `DatabasePool`로 다중 reader + 직렬화된 writer 동시성 보장, 백업·초기화·CSV/JSON 내보내기 단순화.

---

## ADR-003: Dual-Format Parser Strategy & 원시 토큰 SSOT Re-pricing

- **컨텍스트**: Claude/Codex는 Append-Only JSONL이며 Gemini는 JSONL(`$set` 패치 포함)과 단일 `.json` 덮어쓰기 형태가 혼재. 또한 벤더 단가는 수시 변경.
- **결정**:
  1. **이원화 수집**: Append-Only JSONL은 `byte_offset` tail 파서, Gemini `.jsonl`(`$set` 포함)/`.json`은 `mtime`/`file_size` 변경 시 상태 델타 파서 적용.
  2. **원시 토큰 SSOT**: DB에는 원시 토큰(`input`, `output`, `cache_write`, `cache_read`, `thinking`, `tool`)을 불변 원본으로 저장하고, `estimated_cost_usd`와 `pricing_version`을 함께 기록해 단가표 갱신 시 과거 이력 일괄 Re-pricing 가능 구조 채택.
- **검토 대안**: 파싱 시점 비용만 저장(단가 정정·모델 폴백 해제 시 소급 재계산 불가로 기각).
- **결과**: 파서와 가격 계산기(`PricingEngine`) 완전 분리.

---

## ADR-004: Zero-Storage On-Demand 미리보기 & ToS 준수 Tier 2 경계

- **컨텍스트**: 턴 식별 UX(첫 80자 표시) 요구와 프라이버시(DB 원문 미저장) 및 3사 OAuth/ToS 제약([[sources/oauth-tos-constraints]]) 동시 충족 필요.
- **결정**:
  1. SQLite와 `widget-snapshot.json`에는 프롬프트·코드 원문을 0바이트만 저장하고, 대시보드 턴 클릭 시 원본 파일이 존재할 때만 메모리에서 80자 온디맨드 추출.
  2. v2 Tier 2는 소비자 OAuth 스크래핑을 금지하고 Claude Code 공식 Statusline `stdin`, Codex 로그 `payload.rate_limits` 기회적 캡처·`/status` 프로브, GCP Service Usage API로 한정.
- **결과**: 보안 감사·오픈소스 신뢰 확보 및 계정 제재 리스크 제거.

---

## ADR-005: SwiftPM 4-모듈 클린 아키텍처 & Swift 6 Actor 격리

- **컨텍스트**: 비즈니스 로직(파서·효율 점수·단가)은 Xcode GUI 타깃과 분리해 `swift test`로 고속 CI 검증 필요.
- **결정**: SwiftPM 로컬 패키지 기반 4계층 모듈 분리:
  - `AgentsCore` (도메인 모델, SQLite/GRDB 저장소, `PricingEngine`, `EfficiencyScorer`, `ROIEngine`)
  - `AgentsParsers` (`CLIParserProtocol`, Gemini/Claude/Codex 파서 어댑터, 픽스처 테스트)
  - `AgentsApp` (SwiftUI `MenuBarExtra`, `DashboardWindow`, `FSEventsWatcher`, `SleepWakeObserver`)
  - `AgentsWidget` (`WidgetKit` 타임라인 프로바이더 및 S/M/L 뷰)
  - 수집 파이프라인은 Swift 6 `actor IngestionCoordinator`로 직렬화.
- **결과**: `AgentsCore`·`AgentsParsers`는 macOS UI 없이 순수 `swift test`로 CI 자동화 가능.

---

## 관련 문서

- [[sources/cli-schema-validation|로컬 CLI 환경·스키마 검증 요약]]
- [[syntheses/tdd-agents-usage-mac|기술 설계서 (TDD v1.0)]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD v1.2)]]
- [[concepts/macos-menubar-widget-architecture|macOS 메뉴 막대·위젯 아키텍처]]
- [[concepts/two-tier-telemetry-accuracy|2계층 텔레메트리 정확도 모델]]
