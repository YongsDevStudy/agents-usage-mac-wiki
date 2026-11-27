---
id: concept-two-tier-telemetry-accuracy
type: concept
aliases:
  - "2계층 텔레메트리 정확도 모델"
tags:
  - concept
  - accuracy
  - privacy
  - telemetry
  - oauth
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/local-cli-telemetry]]"
  - "[[sources/macos-architecture-constraints]]"
  - "[[sources/oauth-tos-constraints]]"
status: active
---

# 2계층 텔레메트리 정확도 모델 (Two-Tier Accuracy & Privacy)

- **배경**: 100% 로컬 처리 원칙(v1)과 벤더별 OAuth/ToS 제약([[sources/oauth-tos-constraints]])을 준수하면서 구독 한도·실제 잔여량을 가장 정직하고 합법적으로 제공하는 설계.

---

## 1. 2계층 데이터 신뢰도 분류

| 계층 | 수집 방식 | 적용 시점 | 산출 가능 데이터 | UI 표기 배지 |
| :--- | :--- | :---: | :--- | :--- |
| **Tier 1-A (Exact Local)** | `~/.gemini`, `~/.claude`, `~/.codex` 로컬 파일 파싱 | **v1.0** | CLI 토큰(Input/Output/Cache/Thoughts), 턴당 효율, API 환산 비용, 일일 `API Request` 수 | `실측 (Local)` |
| **Tier 1-B (Adaptive Baseline)** | 로컬 사용량 + 5시간 블록·과거 피크·사용자 기준선 모델링 | **v1.0** | 5시간 경과·리셋 카운트다운·누적 비용·번레이트·과거 피크 대비 바 (`%`는 기준 입력·429 학습 시에만 활성) | `추정 (CLI 기준)` |
| **Tier 2 (Verified, Opt-in)** | 벤더별 ToS 준수 공식/로컬 브리지 (v2.0) | **v2.0** | Claude `rate_limits` 실측 %, GCP/Vertex Service Usage, Codex CLI `/status`·Org API 한도 | `공식 동기화` |

---

## 2. 벤더별 v2.0 Tier 2 합법 연동 경로 (소비자 OAuth 스크래핑 금지)

- **Claude Code (ToS 준수 최우선 경로)**: 비공식 `/api/oauth/usage` 토큰 탈취 대신, 사용자가 Opt-in 시 `~/.claude/settings.json` Statusline에 경량 브리지 연결 → Claude Code가 `stdin`으로 전달하는 공식 **`rate_limits.five_hour` / `rate_limits.seven_day`** 수신.
- **Gemini CLI**: 개인 구독은 소비자 OAuth 무임승차 금지 및 쿼터 API 부재로 Tier 1-A(`API Request` 총합 + PT 자정 리셋) 유지. GCP/Vertex AI API 사용자에 한해 공식 Google OAuth + Service Usage API 확장.
- **OpenAI Codex CLI**: 비공식 `wham/usage` 엔드포인트 호출 배제, 사용자 동의 시 CLI `/status`·`/usage` 로컬 실행 파싱 또는 API Org 공식 Usage API 연동.

---

## 3. Adaptive Baseline, 시간대 및 Auth 모드 구분 규칙

1. **비공개 분모 억지 `%` 배제 (Adaptive Baseline)**:
   - Claude/Codex 5시간 블록 기본 UI는 **경과·리셋 카운트다운 + 누적 토큰/환산 비용 + 번레이트($/h) + 과거 5시간 최대 피크 대비 상대 바** 표기.
   - `%` 게이지는 사용자 기준 입력, 429 차단 이력 자가 학습, 또는 v2 Tier 2 연동 시에만 활성화.
2. **단위 및 시간대 기준**:
   - **Gemini 일일 쿼터 분자**: `User Prompt Turn`이 유발한 `Agent Loop Step` 및 `System Overhead`를 모두 포함한 **`API Request` 총합** (효율 분석 분모인 `User Prompt Turn`과 엄격 분리).
   - **리셋 시간대**: Gemini CLI 일일 쿼터는 **Pacific Time(PT) 자정** 기본 + 로컬 자정 토글 지원.
3. **Auth 모드 구분 (시크릿 미열람 원칙)**:
   - macOS `launchd` GUI 앱은 터미널 환경변수에 접근 불가하며 `.zshrc`/`.env` 스캔도 금지.
   - **① CLI 설정 파일의 비민감 인증 타입 메타데이터(`selectedAuthType` 등, 키·토큰 원문 접근 금지) + ② 프로바이더 기본 과금 모드 설정 + ③ 프로젝트/세션 수동 태깅**으로 `Subscription` vs `API Key` 분리.

---

## 4. 프라이버시·보안 및 네트워크 원칙

1. **Zero-Storage On-Demand 미리보기**: SQLite DB 및 `widget-snapshot.json`에 프롬프트 원문 저장 금지(로컬 원본 파일 잔존 시 대시보드 조회 순간 메모리에서만 첫 80자 온디맨드 추출).
2. **네트워크 격리 (v1)**: 기본 오프라인 100%(Sparkle 체크 제외). 단가 카탈로그(`pricing-catalog.json`) 자동 동기화는 Opt-in 시에만 정적 JSON 읽기 전용 GET 요청(사용자 데이터 송신 0건).
3. **시각화**: ![[visuals/telemetry-pipeline-and-accuracy.excalidraw]]

---

## 관련 문서

- [[sources/local-cli-telemetry|로컬 CLI 텔레메트리 요약]]
- [[sources/oauth-tos-constraints|OAuth 정책 및 ToS 제약 요약]]
- [[entities/gemini-cli|Gemini CLI]]
- [[entities/claude-code|Claude Code]]
- [[entities/codex-cli|OpenAI Codex CLI]]
- [[concepts/macos-menubar-widget-architecture|macOS 메뉴 막대·위젯 아키텍처]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD v1.2)]]
