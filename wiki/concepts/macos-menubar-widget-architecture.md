---
id: concept-macos-menubar-widget-architecture
type: concept
aliases:
  - "macOS 메뉴 막대·위젯 아키텍처"
tags:
  - concept
  - macos
  - swiftui
  - widgetkit
  - architecture
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/macos-architecture-constraints]]"
status: active
---

# macOS 메뉴 막대·위젯 네이티브 아키텍처

- **핵심 기술 스택**: Swift 6 / SwiftUI (`MenuBarExtra`, `Window`, `Swift Charts`), `WidgetKit`, `CoreServices(FSEvents)`, `SQLite` (GRDB 등), `SMAppService`, `Sparkle 2`.

---

## 1. 최소 지원 macOS 정책

- 사용자 환경 기준 최신 OS는 **macOS 27 (Golden Gate)**.
- 제품 완성 시점 기준 Apple 공식 보안 지원 3개 OS 정책 적용:
  - **2026년 말 ~ 2027년 상반기 출시**: **`macOS 15.0 Sequoia`** 최소 지원 (macOS 15 / 26 / 27 커버).
  - **2027년 가을 macOS 28 출시 이후**: **`macOS 26.0 Tahoe`** 최소 지원 상향.

---

## 2. 이원화 수집 파이프라인 (Append-Only vs Snapshot Delta)

| 대상 CLI | 파일 쓰기 패턴 | 증분 수집 전략 | 프로젝트 식별 전략 |
| :--- | :--- | :--- | :--- |
| **Claude Code (`~/.claude`)** | Append-Only JSONL | `byte_offset` + 라인 체크섬 기반 tail 읽기 (`requestId` 중복 제거) | 디렉터리 경로 디코딩 자동 매핑 |
| **Codex CLI (`~/.codex`)** | Append-Only JSONL | `byte_offset` 기반 tail 읽기 (`$CODEX_HOME` 지원) | 세션 메타데이터 / `cwd` 경로 자동 매핑 |
| **Gemini CLI (`~/.gemini`)** | 단일 JSON 덮어쓰기 가능성(`session-*.json`) | `mtime` + `file_size` + `messages.count`/마지막 턴 ID 비교 델타 파싱 | **3단계 폴백**: ① CLI 메타데이터 → ② 지정 워크스페이스 루트 해시 매칭 → ③ 수동 프로젝트 별칭 편집 |

---

## 3. 메뉴 막대 노치 안전성, 알림 및 상주 앱 생명주기

- **노치(Notch) 안전 메뉴 막대 표시**:
  - 14"/16" MacBook 노치 환경에서 공간 짤림을 방지하기 위해 기본값은 **초컴팩트 모드**(앱 아이콘 + 1순위 에이전트 핵심 수치 1개 또는 미니 3-바 인디케이터).
  - 다중 에이전트 수치 동시 텍스트 표시는 설정에서 선택형 확장 옵션으로 분리.
- **알림 피로 방지 정책**:
  - 한도 임계치 알림(`70% 주의`, `90% 임박`)은 **동일 윈도우(일일 / 5시간 블록)당 단계별 최대 1회(Once-per-window)** 발송.
  - macOS 집중 모드(Focus/DND) 상태를 존중하며, 대시보드 창이 포그라운드 활성 중일 때는 중복 배너 억제.
- **상주 앱 생명주기 (`LSUIElement` + 동적 ActivationPolicy)**:
  - `SMAppService.mainApp` API로 "로그인 시 자동 실행" 토글 지원.
  - 메뉴 막대 전용 상태에서는 `NSApp.setActivationPolicy(.accessory)`로 Dock 아이콘 숨김 유지, 대시보드 윈도우 열림 시 `.regular` 전환 후 창 닫기 시 `.accessory`로 복귀(앱 종료 방지).

---

## 4. 프로세스 경계 및 WidgetKit 공유 구조

- **메인 앱 (`AgentsUsage.app`)**: **비샌드박스 + Hardened Runtime**으로 `~/.gemini`, `~/.claude`, `~/.codex` 상시 FSEvents 감시 → 로컬 SQLite 저장.
- **위젯 익스텐션 (`AgentsUsageWidget.appex`)**: macOS 강제 샌드박스 실행. 동일 **App Group** 컨테이너 내 경량 스냅샷(`widget-snapshot.json`)을 읽기 전용 조회(프롬프트 원문 0바이트 포함).
- **리로드 예산 준수**: 일일 **40~70회** 예산 보호를 위해 `TimelineProvider` 미래 엔트리 선제 생성 + 1~2초 FSEvents 디바운스 + 15분 쿨다운(5% 급변 시 예외).
- **배포 파이프라인**: Apple Developer ID Application 서명 + `notarytool` 공증 + Sparkle 2 (`appcast.xml`) + Homebrew Cask.
- **시스템 구조도**: ![[visuals/system-architecture.excalidraw]]

---

## 관련 문서

- [[sources/macos-architecture-constraints|macOS 플랫폼 제약 요약]]
- [[concepts/two-tier-telemetry-accuracy|2계층 텔레메트리 정확도 모델]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
