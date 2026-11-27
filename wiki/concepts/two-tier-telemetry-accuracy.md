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
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/local-cli-telemetry]]"
  - "[[sources/macos-architecture-constraints]]"
status: active
---

# 2계층 텔레메트리 정확도 모델 (Two-Tier Accuracy & Privacy)

- **배경**: 100% 로컬 처리 원칙(v1)과 웹·CLI 공유 풀을 가진 벤더 구독 한도의 정확한 실시간 잔여량 파악 요구 사이의 간극 해결.

---

## 1. 2계층 데이터 신뢰도 분류

| 계층 | 수집 방식 | 적용 시점 | 산출 가능 데이터 | UI 표기 배지 |
| :--- | :--- | :---: | :--- | :--- |
| **Tier 1-A (Exact Local)** | `~/.gemini`, `~/.claude`, `~/.codex` 로컬 파일 파싱 | **v1.0** | CLI 토큰(Input/Output/Cache/Thoughts), 턴당 효율, API 환산 비용, 일일 CLI 요청 수 | `실측 (Local)` |
| **Tier 1-B (Adaptive Baseline)** | 로컬 사용량 + 5시간 블록·과거 피크·사용자 기준선 모델링 | **v1.0** | 5시간 경과·리셋 카운트다운·누적 비용·번레이트·과거 피크 대비 바 (`%`는 기준 입력·429 학습 시에만 활성) | `추정 (CLI 기준)` |
| **Tier 2 (Verified, Opt-in)** | 사용자 명시 동의 기반 공식 API/OAuth 또는 CLI `/usage` 상태 프로브 | **v2.0** | 서버 기준 실제 잔여 쿼터·API 크레딧 잔액·조직 한도 | `공식 동기화` |

---

## 2. 5시간 블록 Adaptive Baseline & 시간대·인증 구분 규칙

1. **비공개 분모 억지 `%` 배제 (Adaptive Baseline)**:
   - Anthropic(Claude Pro/Max/Ultra)과 OpenAI(ChatGPT Plus/Pro)는 5시간당 정확한 토큰/달러 상한을 비공개하며 트래픽·모델별로 동적 조정.
   - 따라서 v1 기본 UI에서는 Claude/Codex에 임의 `%`를 강제하지 않고 **5시간 경과·리셋 카운트다운 + 현재 블록 누적 토큰/환산 비용 + 번레이트($/h) + 사용자의 과거 5시간 최대 피크 대비 상대 바**를 기본 표시.
   - `%` 게이지는 (a) 사용자가 직접 기준 한도(예: 블록당 $X 또는 Y만 토큰) 입력, (b) 429 한도 차단 이력 자가 학습, (c) v2 Tier 2 공식 동기화 연결 시에만 활성화.
2. **리셋 시간대 기준**:
   - **Gemini CLI 일일 쿼터**: 벤더 실제 리셋 기준(**Pacific Time 자정**, KST 기준 오후 4~5시) 기본 적용 + 설정에서 로컬 자정(KST) 전환 토글 지원.
   - **Claude/Codex 5시간 블록**: 해당 블록 첫 요청 시각 + 5시간 롤링 윈도우.
3. **인증 모드(Auth Mode) 자동 구분**:
   - 설정·환경변수(`GEMINI_API_KEY`, `ANTHROPIC_API_KEY`, `OPENAI_API_KEY`) 및 로그 메타데이터를 우선 감지해 **구독 로그인(`Subscription`)**과 **종량제 키(`API Key`)** 세션을 분리 집계(사용자 수동 태깅 오버라이드 지원).

---

## 3. 프라이버시·보안 및 네트워크 원칙

1. **Zero-Storage On-Demand 미리보기 (원문 DB 미저장)**:
   - SQLite DB에는 기본값으로 프롬프트 본문·코드·도구 출력 원문을 저장하지 않고 **토큰 수치·타임스탬프·모델명·도구 호출 메타데이터(도구명·바이트 길이·성공 여부)**만 영구 보관.
   - 대시보드 턴 상세 조회 시 사용자의 질문 식별을 돕기 위해 로컬 원본 로그 파일이 디스크에 남아 있는 경우 **메모리 상에서만 첫 80자를 온디맨드 추출 표시**(디스크/스냅샷/위젯에 복제 저장 금지, 원본 삭제 후에는 메타데이터만 표시).
2. **네트워크 격리 및 정적 단가 동기화 (v1)**:
   - v1 기본값은 외부 네트워크 송신 **0건**(Sparkle 업데이트 체크 제외).
   - 외부 단가 카탈로그(`pricing-catalog.json`)는 번들 내장 + 로컬 오버라이드 + Sparkle 앱 업데이트 갱신을 기본으로 하되, 설정에서 **사용자 Opt-in 시에만** GitHub 공식 정적 단가 JSON 읽기 전용 동기화 허용(사용자 사용량·식별 정보 송신 0건).
3. **비공식 스크래핑 기본 배제**:
   - 브라우저 쿠키 탈취 등 비공식 세션 스크래핑을 기본 수집 방식으로 채택하지 않음.
4. **시각화**: ![[visuals/telemetry-pipeline-and-accuracy.excalidraw]]

---

## 관련 문서

- [[sources/local-cli-telemetry|로컬 CLI 텔레메트리 요약]]
- [[entities/gemini-cli|Gemini CLI]]
- [[entities/claude-code|Claude Code]]
- [[entities/codex-cli|OpenAI Codex CLI]]
- [[concepts/macos-menubar-widget-architecture|macOS 메뉴 막대·위젯 아키텍처]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
