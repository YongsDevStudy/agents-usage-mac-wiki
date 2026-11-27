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

- **핵심 기술 스택**: Swift / SwiftUI (`MenuBarExtra`, `Window`, `Swift Charts`), `WidgetKit`, `FSEvents`, `SQLite` (GRDB 등), `Sparkle 2`.

---

## 1. 최소 지원 macOS 정책

- 사용자 환경 기준 최신 OS는 **macOS 27 (Golden Gate)**.
- 제품 완성 시점 기준 Apple 공식 보안 지원 3개 OS 정책 적용:
  - **2026년 말 ~ 2027년 상반기 출시**: **`macOS 15.0 Sequoia`** 최소 지원 (macOS 15 / 26 / 27 커버).
  - **2027년 가을 macOS 28 출시 이후**: **`macOS 26.0 Tahoe`** 최소 지원 상향.

---

## 2. 프로세스 경계 및 데이터 공유

- **메인 앱 (`AgentsUsage.app`)**:
  - **비샌드박스 + Hardened Runtime** (`LSUIElement` 선택 토글 지원 — 대시보드 창 열림 시 Dock 아이콘 동적 표시).
  - `~/.gemini`, `~/.claude`, `~/.codex` FSEvents 감시 → 증분 파서 → 로컬 SQLite 저장.
- **위젯 익스텐션 (`AgentsUsageWidget.appex`)**:
  - macOS 강제 샌드박스 실행.
  - 메인 앱과 동일 **App Group** 컨테이너 내 경량 스냅샷(`widget-snapshot.json`)을 읽고 표시.
  - 일일 **40~70회 리로드 예산** 준수를 위해 `TimelineProvider` 미래 엔트리 선제 생성 + 디바운스/임계치 기반 조건부 리로드 호출.
- **배포 파이프라인**: Apple Developer ID 서명 + `notarytool` 공증 + Sparkle 2 (`appcast.xml`) + Homebrew Cask.
- **시스템 구조도**: ![[visuals/system-architecture.excalidraw]]

---

## 관련 문서

- [[sources/macos-architecture-constraints|macOS 플랫폼 제약 요약]]
- [[concepts/two-tier-telemetry-accuracy|2계층 텔레메트리 정확도 모델]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
