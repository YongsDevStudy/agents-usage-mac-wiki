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
- **라이선스 및 현지화**: **MIT License**, **영어 + 한국어(`.xcstrings`)** 동시 지원, **USD(`$`) 기본 + 선택적 KRW(`₩`) 수동 환율 병기**.

---

## 1. 최소 지원 macOS 정책

- 사용자 환경 기준 최신 OS는 **macOS 27 (Golden Gate)**.
- 제품 완성 시점 기준 Apple 공식 보안 지원 3개 OS 정책 적용:
  - **2026년 말 ~ 2027년 상반기 출시**: **`macOS 15.0 Sequoia`** 최소 지원 (macOS 15 / 26 / 27 커버).
  - **2027년 가을 macOS 28 출시 이후**: **`macOS 26.0 Tahoe`** 최소 지원 상향.

---

## 2. 이원화 수집 파이프라인, Sleep/Wake 복구 및 DB 수명주기

| 대상 CLI | 파일 쓰기 패턴 | 증분 수집 전략 | 프로젝트 식별 전략 |
| :--- | :--- | :--- | :--- |
| **Claude Code (`~/.claude`)** | Append-Only JSONL | `byte_offset` + 라인 체크섬 tail 읽기 (`requestId` 중복 제거) | 디렉터리 경로 디코딩 자동 매핑 |
| **Codex CLI (`~/.codex`)** | Append-Only JSONL | `byte_offset` tail 읽기 (`$CODEX_HOME` 지원) | 세션 메타데이터 / `cwd` 경로 자동 매핑 |
| **Gemini CLI (`~/.gemini`)** | 단일 JSON 덮어쓰기 가능성(`session-*.json`) | `mtime` + `file_size` + `messages.count`/턴 ID 델타 파싱 | **3단계 폴백**: ① CLI 메타데이터 → ② 워크스페이스 루트 해시 매칭 → ③ 수동 별칭 편집 |

- **앱 재시작 & Sleep/Wake Catch-up**: 앱 구동 시 및 `NSWorkspace.didWakeNotification` 수신 시 전체 워치 경로의 변경 파일 Catch-up 스캔과 5시간 블록 타이머 재동기화 실행(잠자기 중 FSEvents 누락 방지).
- **SQLite 수명주기 및 삭제권**:
  - **무제한 영구 보관**: 상세 턴 레코드 + 일별 집계 롤업 테이블 동시 유지.
  - **사용자 삭제권**: Settings에서 특정 프로젝트/세션 단위 영구 삭제 및 전체 DB 초기화(Factory Reset) 제공.
  - **데이터 포터빌리티**: JSON / CSV 리포트 내보내기 지원.

---

## 3. 메뉴 막대 노치 안전성, 멀티 세션 우선순위, 알림 및 생명주기

- **노치(Notch) 안전 메뉴 막대 표시**:
  - 기본값은 **초컴팩트 모드**(앱 아이콘 + 1순위 에이전트 수치 1개 또는 미니 3-바 인디케이터). 다중 에이전트 텍스트 병렬 표시는 설정 내 확장 옵션.
- **멀티 세션 팝오버 배지 우선순위**:
  - 동시 실행 중인 복수 터미널 세션 중 팝오버 상단 효율 배지 선정 순위: ① **경고 발생 활성 세션(최근 30분 내 이벤트)** 최우선 → ② **가장 최근 이벤트 세션**.
- **알림 피로 방지 정책**:
  - 한도 임계치 알림(`70% 주의`, `90% 임박`)은 **동일 윈도우(일일 / 5시간 블록)당 단계별 최대 1회(Once-per-window)** 발송.
  - macOS 집중 모드(Focus/DND) 준수 및 대시보드 포그라운드 시 중복 배너 억제.
- **상주 앱 생명주기 (`LSUIElement` + 동적 ActivationPolicy)**:
  - `SMAppService.mainApp` API로 "로그인 시 자동 실행" 토글 지원.
  - 메뉴 막대 전용 시 `NSApp.setActivationPolicy(.accessory)`, 대시보드 열림 시 `.regular` 전환 후 창 닫기 시 앱 종료 없이 `.accessory` 복귀.

---

## 4. 프로세스 경계, WidgetKit 공유 및 배포

- **메인 앱 (`AgentsUsage.app`)**: **비샌드박스 + Hardened Runtime** (`~/.gemini`, `~/.claude`, `~/.codex` 상시 감시).
- **위젯 익스텐션 (`AgentsUsageWidget.appex`)**: 강제 샌드박스, 동일 **App Group** 컨테이너 `widget-snapshot.json` 읽기 전용 조회(프롬프트 원문 0바이트 포함).
- **리로드 예산 준수**: 일일 **40~70회** 예산 보호(`TimelineProvider` 미래 엔트리 + 1~2초 디바운스 + 15분 쿨다운).
- **배포**: Apple Developer ID 서명 + `notarytool` 공증 + Sparkle 2 (`appcast.xml`) + Homebrew Cask.
- **시스템 구조도**: ![[visuals/system-architecture.excalidraw]]

---

## 관련 문서

- [[sources/macos-architecture-constraints|macOS 플랫폼 제약 요약]]
- [[concepts/two-tier-telemetry-accuracy|2계층 텔레메트리 정확도 모델]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD v1.2)]]
