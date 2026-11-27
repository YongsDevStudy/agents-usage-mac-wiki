---
id: source-macos-architecture-constraints
type: source
raw_source: "raw/05-macos-architecture-constraints.md"
aliases:
  - "macOS 플랫폼 제약 및 아키텍처 근거"
tags:
  - source
  - macos
  - swiftui
  - widgetkit
  - sparkle
created: 2026-11-27
updated: 2026-11-27
sources: []
status: active
---

# Source: macOS 플랫폼 제약 및 네이티브 아키텍처 근거

- **원본**: [[raw/05-macos-architecture-constraints.md]] (기준일: 2026-11-27)
- **핵심 요약**: Apple 최신 3개 OS 지원 정책에 맞춘 최소 OS 기준(`macOS 15.0 Sequoia` → 향후 `macOS 26.0 Tahoe`), 비샌드박스 메인 앱 + 샌드박스 WidgetKit App Group 브리지, 일일 40~70회 리로드 예산 관리, Sparkle/DMG 공증 배포 규격 정리.

---

## 주요 추출 팩트

1. **OS 지원 정책 (2026-11-27 기준)**:
   - Apple 보안 지원 대상: `macOS 15 Sequoia` / `macOS 26 Tahoe`(마지막 Intel) / `macOS 27 Golden Gate`(Apple Silicon 전용, Liquid Glass).
   - 완성 시점 기준 공식 지원 최소 버전 적용: 2026~2027 상반기 `macOS 15.0 Sequoia`, 2027 가을 macOS 28 출시 이후 `macOS 26.0 Tahoe`.
2. **비샌드박스 + 샌드박스 하이브리드 경계**:
   - 메인 앱: `~/.gemini`, `~/.claude`, `~/.codex` 상시 FSEvents 감시를 위해 비샌드박스 + Hardened Runtime.
   - WidgetKit 익스텐션(`.appex`): 시스템 강제 샌드박스 → **App Group** 컨테이너를 통한 스냅샷 공유 및 Developer ID + Provisioning Profile 필수.
3. **WidgetKit 리로드 예산**: 일일 40~70회(15~60분) 제한 → `TimelineProvider` 미래 엔트리 선제 생성 + 디바운스·임계치 기반 조건부 리로드.
4. **배포**: Developer ID 서명 + `notarytool` 공증 + Sparkle 2(`appcast.xml`) + Homebrew Cask.

---

## 관련 위키 문서

- [[concepts/macos-menubar-widget-architecture|macOS 메뉴 막대·위젯 아키텍처]]
- [[concepts/two-tier-telemetry-accuracy|2계층 텔레메트리 정확도 모델]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
