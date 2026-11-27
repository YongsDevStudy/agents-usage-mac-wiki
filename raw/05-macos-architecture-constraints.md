# 05. macOS 플랫폼 제약 및 네이티브 아키텍처 근거 (macOS Architecture Constraints)

- **기준일**: 2026-11-27
- **대상 스택**: Swift / SwiftUI (`MenuBarExtra`), `WidgetKit`, `Swift Charts`, `Sparkle 2`, DMG / Homebrew Cask
- **주요 출처 도메인**: `apple.com`, `sparkle-project.org`, `nilcoalescing.com`, `versio.io`, `setapp.com`, `install.guide`

---

## 1. macOS 버전 생명주기 및 최소 지원 버전 기준

### 1.1 2026년 11월 기준 Apple 지원 현황
- Apple은 관례적으로 **최신 3개 메이저 macOS 버전**에 대해 보안 업데이트 제공:
  1. **macOS 27 (Golden Gate, 2026년 9월 출시)**: 현재 최신 OS. Apple Silicon 전용(Intel 지원 완전 종료), "Liquid Glass" UI 및 확장된 Apple Intelligence 도입.
  2. **macOS 26 (Tahoe, 2025년 9월 출시)**: 연도 기반 명명법 최초 적용, 마지막 Intel Mac 지원 OS.
  3. **macOS 15 (Sequoia, 2024년 9월 출시)**: 기존 순차 명명 마지막 OS, 현재 보안 패치 유지 하한선.
- macOS 14 (Sonoma) 이하 버전은 정규 3년 보안 지원 창에서 이탈(EOL).

### 1.2 제품 최소 OS 버전 정책
- **정책 원칙**: "애플리케이션 완성·배포 시점 기준 Apple 공식 보안 지원 최소 버전" 채택.
  - **2026년 말 ~ 2027년 상반기 출시 기준**: **`macOS 15.0 Sequoia`** (현재 보안 지원 3개 OS 중 최소 버전, `SwiftUI` `MenuBarExtra`·`WidgetKit`·`Swift Charts` 안정 API 완비).
  - **2027년 가을 macOS 28 출시 이후 배포/메이저 업데이트 시**: **`macOS 26.0 Tahoe`**로 최소 지원 버전 상향.

---

## 2. 비샌드박스 메인 앱 + 샌드박스 WidgetKit 아키텍처

### 2.1 App Sandbox 제외 근거 (메인 앱)
- 사용자의 로컬 홈 디렉터리 하위 숨김 경로(`~/.gemini`, `~/.claude`, `~/.codex`) 및 사용자 지정 `CODEX_HOME`을 FSEvents로 상시 감시해야 함.
- Mac App Store(MAS) 샌드박스 환경에서는 해당 경로마다 사용자가 폴더 선택 패널(`NSOpenPanel`)로 반복 권한 부여 필요 + CLI 하위 프로세스 실행/상태 점검 제한 → **DMG 및 Homebrew Cask 직접 배포(Non-Sandboxed + Hardened Runtime)** 채택.

### 2.2 App Group 브리지 제약
- 메인 앱이 비샌드박스더라도 **WidgetKit 익스텐션(`.appex`)은 macOS 시스템 정책상 반드시 샌드박스 프로세스로 실행**.
- 따라서 메인 앱과 WidgetKit 타깃 간 데이터 공유를 위해 다음 요건 필수:
  1. 양쪽 타깃 모두에 동일 **App Group**(`group.<domain>.agents-usage-mac`) Entitlement 설정.
  2. Apple Developer 계정을 통한 **Provisioning Profile** 매칭 및 메인 앱 번들 내 `Contents/PlugIns/` 임베드.
  3. 메인 앱은 `FileManager.default.containerURL(forSecurityApplicationGroupIdentifier:)` 경로에 경량 스냅샷(JSON 또는 WAL 모드 읽기 최적화 SQLite)을 기록하고, 위젯은 해당 컨테이너에서 읽기 전용 조회.

---

## 3. WidgetKit 리로드 예산(Reload Budget) 및 타임라인 전략

- **시스템 제약**: macOS WidgetKit은 배터리·자원 보호를 위해 활성 위젯당 **24시간 내 40~70회** 리로드 예산 적용(평균 15~60분 간격).
- **예외**: 메인 앱이 포그라운드에서 활성 실행 중일 때의 리로드 동작은 상대적으로 유연하나, 백그라운드 파일 변경 이벤트마다 `WidgetCenter.shared.reloadAllTimelines()`를 무분별 호출하면 일일 예산 조기 고갈 위험.
- **설계 원칙**:
  - `TimelineProvider`가 향후 시간대별 `TimelineEntry`(예: 5시간 블록 리셋 카운트다운·15~30분 단위 예정 엔트리)를 선제 생성.
  - 메인 앱은 FSEvents 수신 시 **디바운스(예: 1~2초)** 처리 후 로컬 DB·App Group 스냅샷을 갱신하고, 위젯 리로드는 **임계치(예: 5% 이상 변동, 한도 경고 단계 진입, 사용자 수동 새로고침) 또는 최소 쿨다운(예: 15분) 충족 시**에만 트리거.

---

## 4. 배포 및 자동 업데이트 파이프라인

- **서명·공증**: Apple Developer ID Application 인증서 서명 + Hardened Runtime 활성화 + `xcrun notarytool` 공증(Notarization) 및 스테이플링(Gatekeeper 통과 필수).
- **자동 업데이트**: **Sparkle 2** 프레임워크 + GitHub Releases 호스팅 `appcast.xml`. 비샌드박스 앱이므로 별도 XPC 설치 헬퍼 없이 표준 Sparkle 구성으로 무중단 DMG/ZIP 업데이트 가능.
- **패키지 배포**: GitHub Releases DMG 제공 + Homebrew Cask (`brew install --cask ...`) 등록.
