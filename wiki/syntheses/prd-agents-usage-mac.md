---
id: prd-agents-usage-mac
type: synthesis
aliases:
  - "Agents Usage Mac PRD"
  - "제품 기획서 v1.1"
tags:
  - synthesis
  - prd
  - planning
  - macos
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/market-competitors]]"
  - "[[sources/local-cli-telemetry]]"
  - "[[sources/pricing-quotas-2026]]"
  - "[[sources/prompt-efficiency-metrics]]"
  - "[[sources/macos-architecture-constraints]]"
status: active
---

# 제품 기획서 (PRD): Agents Usage Mac

- **문서 버전**: **v1.1** (1차 리뷰 P0/P1/P2 보완 반영 · 로컬 픽스처 검증 및 기술 설계 진행 중, 기준일 `2026-11-27`)
- **제품 형태**: macOS 네이티브 메뉴 막대 앱 + WidgetKit 위젯 + 대시보드 윈도우 (오픈소스 / 무료, DMG & Homebrew Cask 배포)
- **저장소 구성**: 기획·설계·검증(`YongsDevStudy/agents-usage-mac-wiki`) ↔ 앱 구현(`YongsDevStudy/agents-usage-mac`)
- **근거 기반**: [[sources/market-competitors]], [[sources/local-cli-telemetry]], [[sources/pricing-quotas-2026]], [[sources/prompt-efficiency-metrics]], [[sources/macos-architecture-constraints]]

---

## 1. Executive Summary & 제품 비전

### 1.1 배경 및 핵심 문제 정의
- 터미널 코딩 에이전트([[entities/gemini-cli|Gemini CLI]], [[entities/claude-code|Claude Code]], [[entities/codex-cli|Codex CLI]]) 병행 사용이 일반화됨에 따라 다음 4가지 실무 페인포인트 심화:
  1. **한도 예측 불능**: Claude Pro/Max·ChatGPT Plus/Pro의 5시간·주간 롤링 한도와 Gemini CLI 일일 요청 한도(1,000~2,000 req/day) 소진 시점 사전 예측 불가.
  2. **질문(턴)당 토큰 누수 미가시화**: 무상태(Stateless) LLM의 컨텍스트 재전송 구조로 인해 세션 장기화 시 비용·토큰 소비가 이차적으로 폭증하나 턴 단위 비효율 원인 식별 불가.
  3. **요금제 ROI 근거 부재**: 현재 월 구독 플랜($20~$500) 대비 실제 소비 가치 및 한도 마찰 빈도를 비교할 정량 데이터 부족(구독·API 키 혼용 시 구분 어려움 포함).
  4. **다중 에이전트 분산**: 에이전트별 로그 포맷·저장 경로·리셋 시간대·과금 체계가 상이해 통합 소비량·모델별 효율 단일 뷰 부재.

### 1.2 제품 포지셔닝
- **한 줄 정의**: *"로컬 코딩 에이전트의 실시간 한도 감시부터 질문당 토큰 효율 코칭, 요금제 ROI 최적화까지 완결하는 프라이버시 우선 네이티브 macOS 컴패니언"*
- **핵심 가치 순환**: **Monitor (메뉴 막대·위젯 실시간 감시) → Diagnose (질문당 효율·컨텍스트 위생 진단) → Optimize (행동 코칭 및 요금제 ROI 최적화)**

### 1.3 북극성 지표(NSM) 및 검증 KPI

| 구분 | 지표명 | 정의 및 산출 기준 | 목표치 (v1/v2 검증 기준) |
| :--- | :--- | :--- | :--- |
| **NSM** | **Weekly Active Efficient Sessions** | 주간 1회 이상 효율 코칭(`/compact`/`/clear`/모델 전환) 반영 또는 한도 임계치 회피 완료 세션 비율 | 활성 세션 중 효율 점수 ≥ 75점 비중 지속 상승 |
| **실시간성** | 로그-UI 반영 지연 | CLI 턴 종료 후 메뉴 막대·대시보드 반영까지 소요 시간 | P95 < `500ms` (FSEvents 증분/델타 파싱) |
| **정확도** | 로컬 파싱 정합성 | `requestId` 중복 제거 및 단가표 대조 기준 `ccusage` 또는 실제 청구서 대비 오차 | 로컬 토큰 집계 오차 < `1%` (플레이스홀더 플래그 제외) |
| **효율 개선** | 평균 Cache Hit Rate & 낭비 절감 | 2주 이상 연속 사용자의 세션 평균 캐시 적중률 및 컨텍스트 비대화 감소율 | Cache Hit Rate ≥ `75%`, 비대화 경고 세션 비율 감소 |
| **자원 효율** | 상주 리소스 점유 | 메뉴 막대 상시 실행 시 유휴 CPU 및 메모리 사용량 | 유휴 CPU < `1%`, RAM < `80MB` |

---

## 2. 타깃 페르소나 & 핵심 사용자 여정 (JTBD)

### 2.1 주 타깃 페르소나
- **페르소나 A — 다중 CLI 에이전트 파워유저 (1차 핵심 타깃)**
  - **프로필**: macOS 환경에서 [[entities/gemini-cli|Gemini CLI]]를 주력(1순위)으로 사용하며, 코드 리뷰·아키텍처 설계·복잡 디버깅에 [[entities/claude-code|Claude Code]]와 [[entities/codex-cli|Codex CLI]]를 병행하는 소프트웨어 엔지니어.
  - **구독 조합**: Google AI Pro/Ultra, Claude Pro 또는 Max, ChatGPT Plus/Pro 중 1~3종 복수 구독 또는 프로젝트별 API 키 혼용.
  - **JTBD (Jobs-to-be-Done)**:
    - *"작업 도중 한도 차단을 맞기 전에 메뉴 막대나 위젯으로 리셋 카운트다운·번레이트·피크 대비 소비량을 확인하고 에이전트를 전환하고 싶다."*
    - *"방금 보낸 질문 1건이 왜 그렇게 많은 토큰을 썼는지, 지금 `/compact`를 쳐야 하는지 대시보드에서 즉시 보고 싶다."*
    - *"구독 결제 주기 기준 실제 소비량을 근거로 현재 요금제를 유지할지, 다운그레이드+API 종량제로 내릴지 숫자로 검증받고 싶다."*

### 2.2 일일 워크플로우 접점 매핑

| 시점 | 사용 접점 | 사용자 행동 및 시스템 제공 가치 |
| :--- | :--- | :--- |
| **상시 코딩 중** | **메뉴 막대 아이콘 & WidgetKit 위젯** | 주력 에이전트(Gemini CLI 일일 요청 수, 벤더 PT 자정 리셋 기준) 및 활성 블록(Claude/Codex 5시간 경과·누적 비용·피크 대비 바) 상시 확인 |
| **임계치 근접 시** | **메뉴 막대 팝오버 & macOS 알림** | 윈도우당 단계별 1회 알림 확인 후 번레이트·카운트다운 점검 및 대체 CLI 에이전트로 작업 전환 |
| **세션 지연·비용 체감 시** | **대시보드 (Prompt Efficiency 탭)** | 최근 `User Prompt Turn`별(80자 메모리 미리보기 포함) 토큰 분해, 캐시 적중률 급락 지점, 컨텍스트 팽창 곡선 확인 후 `/compact`·`/clear` 실행 |
| **결제일 전후 (v2)** | **대시보드 (Plan ROI 탭)** | 결제 주기 기준 API 환산 가치($V_{\text{api}}$) 및 낭비 보정 소비액($V_{\text{adj}}$) 대비 구독료 비교·추천 검토 |

---

## 3. 시장·경쟁 분석 및 차별화 전략

### 3.1 경쟁 도구 비교 요약
- 상세 분석: [[entities/competitor-landscape]] 및 [[sources/market-competitors]] 참조.

| 비교 항목 | CodexBar | `ccusage` (CLI) | OpenUsage | ClaudeBar 류 | **`agents-usage-mac`** |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **네이티브 메뉴 막대 + 위젯** | 메뉴 막대 | ❌ | 메뉴 막대 + CLI | 메뉴 막대 | **메뉴 막대 + WidgetKit** |
| **Gemini CLI 1순위·영구 적재** | 보조 수준 | ✅ (일회성) | 제한적 | ❌ | **✅ (30d 삭제 방어 DB)** |
| **Claude + Codex 필수 통합** | ✅ | ✅ | ✅ | ❌ | **✅** |
| **100% 로컬 처리 기본값** | 혼합(쿠키/RPC) | ✅ | 혼합 | 혼합 | **✅ (v1 Strict Local)** |
| **질문당 토큰 효율 진단·코칭** | ❌ | ❌ | ❌ | ❌ | **✅ (6대 지표 + 정규화 점수)** |
| **요금제 ROI 비교·변경 추천** | ❌ | ❌ | ❌ | ❌ | **✅ (v2 반사실적 $V_{\text{adj}}$)** |

### 3.2 차별화 쐐기 (Differentiation Wedge)
1. **모니터링을 넘어선 행동 변화(Actionable Coaching)**: 경쟁 제품이 단순 누적치만 보여줄 때, 본 앱은 `User Prompt Turn` 단위로 *"최근 10턴 동안 미압축 컨텍스트 재전송으로 질문당 비용이 3.4배 증가 — `/compact` 실행 권장"*을 제시.
2. **Gemini CLI 보존 공백 해결**: `general.sessionRetention`(`maxAge: 30d`, `maxCount: 50`) 자동 삭제 이전에 로컬 SQLite에 영구 적재해 월간·분기 ROI 데이터 보존.
3. **데이터 정직성(Honest Telemetry)**: 비공개 5시간 상한에 억지 `%`를 매기지 않고 Adaptive Baseline(경과·비용·피크 바)과 `실측`/`추정` 배지를 엄격히 구분.

![[visuals/competitive-positioning.excalidraw]]

---

## 4. 제품 범위 & 단계별 로드맵 (v1 / v2 / v3) 및 Non-Goals

### 4.1 MoSCoW 범위 정의

| 우선순위 | 범위 항목 | 대상 버전 |
| :--- | :--- | :---: |
| **Must Have** | Gemini CLI(1순위)·Claude Code·Codex CLI 이원화 로그 수집(JSONL `byte_offset` / JSON 델타) 및 SQLite 영구 적재 | **v1.0** |
| **Must Have** | 노치 안전 메뉴 막대(`MenuBarExtra`) + Adaptive Baseline 팝오버 + WidgetKit(S/M/L) 위젯 | **v1.0** |
| **Must Have** | 질문(`User Prompt Turn`)당 효율 분석 대시보드(6대 지표, Cold-Start 예외 반영 점수, `/compact`·`/clear` 코칭, 메모리 80자 미리보기) | **v1.0** |
| **Must Have** | 다중 에이전트·프로젝트(Gemini 해시 3단계 역매핑)·Auth 모드(`Subscription`/`API Key`) 통합 뷰 및 외부 JSON 단가표 | **v1.0** |
| **Should Have** | 요금제 ROI 비교기($V_{\text{api}}$ / $V_{\text{adj}}$) & 업/다운그레이드/API 하이브리드 추천 엔진 | **v2.0** |
| **Should Have** | 사용자 Opt-in 기반 Tier 2 공식 API/OAuth 및 CLI `/usage` 상태 동기화 | **v2.0** |
| **Could Have** | Gemini CLI 로컬 OTel(`4317`) 수신기, 선택적 로컬 LLM(Apple Intelligence/Ollama) 프롬프트 리뷰 | **v2.1+** |
| **Won't Have (v1/v2)** | AI IDE(Cursor·Antigravity·Copilot·Windsurf), 종량제 API 키 전용 대시보드, 웹 챗봇 통합 | **v3.0+** |

### 4.2 마일스톤 로드맵
- **v1.0 (MVP — Local Monitor & Efficiency Coach)**: 100% 로컬 처리, CLI 3종 파서, SQLite 영구 적재, 메뉴 막대 + WidgetKit + 대시보드 4개 탭(`Overview`, `Prompt Efficiency`, `Sessions & Projects`, `Settings`) + 단가 카탈로그(Opt-in 정적 JSON 동기화 포함).
- **v1.1 (Hardening & UX Polish)**: 임계치 도달 알림 규칙 고도화, 프로젝트별 예산 알림, CSV/JSON 리포트 내보내기, Sparkle 2 자동 업데이트·Homebrew Cask 배포.
- **v2.0 (Plan ROI & Opt-in Official Sync)**: `Plan ROI` 추천 탭 정식 탑재, Opt-in Tier 2 공식 API/OAuth·CLI 상태 동기화, API 크레딧 잔액 추적.
- **v3.0+ (Ecosystem Expansion)**: AI IDE 확장(Cursor, Antigravity, Copilot, Windsurf), 종량제 API 키, 웹 챗봇 통합.

### 4.3 명시적 비목표 (Non-Goals)
1. **프롬프트·소스코드 원문 저장·외부 전송 금지**: DB에 원문 미저장(로컬 파일 잔존 시 메모리 80자 온디맨드 미리보기만 허용), v1에서 사용자 데이터 네트워크 송신 0건.
2. **비공식 브라우저 쿠키 탈취 기본 비활성**: 브라우저 쿠키·세션 토큰을 무단 추출하는 스크래핑 배제.
3. **LLM 기반 실시간 프롬프트 채점(v1 제외)**: v1 효율 점수는 100% 결정적 수학/규칙 모델만 사용.
4. **팀/서버 동기화 SaaS(제외)**: 개인 로컬 macOS 워크스테이션 단일 사용자 경험에 집중.

---

## 5. 상세 기능 요구사항 (FR-01 ~ FR-07)

### FR-01. 다중 CLI 로컬 로그 이원화 수집·정규화 엔진 (v1.0)
- **목적**: 로컬 코딩 에이전트 3종의 로그를 비침습적으로 감지·파싱해 통합 데이터 모델로 영구 저장.
- **이원화 수집 전략**([[concepts/macos-menubar-widget-architecture]]):
  - **Append-Only JSONL (`byte_offset` 증분)**: Claude Code(`~/.claude/projects/<project-path>/<session-id>.jsonl`), Codex CLI(`~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl`, `$CODEX_HOME` 우선).
  - **Full-JSON Snapshot Delta (`mtime` + `file_size` + `messages.count`/턴 ID 델타)**: Gemini CLI(`~/.gemini/tmp/<project_hash>/chats/session-*.json`).
- **로그 이벤트 3분류 및 메타데이터 보강**:
  - `User Prompt Turn` / `Agent Loop Step` / `System & Subagent Overhead` 엄격 분리(`tool_result`가 `role: "user"`로 기록되는 경우 턴 분모에서 제외).
  - **Auth 모드 구분**: 설정/환경변수(`GEMINI_API_KEY`, `ANTHROPIC_API_KEY`, `OPENAI_API_KEY`) 및 로그 메타데이터로 `Subscription` vs `API Key` 자동 감지 + 수동 태깅 오버라이드.
  - **Gemini `<project_hash>` 3단계 역매핑**: ① CLI 메타데이터 자동 추출 → ② 사용자 지정 워크스페이스 루트 경로 해시 매칭 → ③ 수동 프로젝트 별칭 편집.
  - **시간대 기준**: Gemini CLI 일일 쿼터는 벤더 실제 리셋 기준(**Pacific Time 자정**) 기본 적용 + 로컬 자정 토글 지원.
- **중복·이상치·영구 보존 가드**:
  - Claude Code `requestId`/`message.id` 기준 최종 레코드 병합, 플레이스홀더(`input/output <= 2`) 감지 시 `confidence = low` 부여.
  - Gemini CLI `sessionRetention` 삭제 이후에도 이미 적재된 `sessions`, `turns`, `api_requests` 레코드 영구 유지.
  - 파싱 실패 파일은 앱 크래시 없이 격리 테이블(`ingest_errors`)에 기록 후 경고 배지 표시.
- **인수 조건 (Acceptance Criteria)**:
  - `Given` Claude Code JSONL에 동일 `requestId`가 5줄 중복 기록되고 `tool_result`(`role: "user"`)가 포함될 때 `When` `byte_offset` 증분 파싱 수행 `Then` `api_requests`에는 최종 1건만 적재되고 `User Prompt Turn` 수는 실제 사용자 질문 수만 카운트됨.
  - `Given` Gemini CLI `session-*.json`이 턴 추가 시 전체 덮어쓰기될 때 `When` `mtime` 델타 파싱 수행 `Then` 기존 적재 턴은 중복 없이 유지되고 신규 턴만 추가 적재됨.
  - `Given` Gemini CLI 원본 채팅 파일이 CLI retention으로 삭제됨 `When` 앱 재시작 또는 리프레시 `Then` 과거 일간/결제 주기 집계 수치가 유실 없이 유지됨.

### FR-02. 노치 안전 메뉴 막대 팝오버 & WidgetKit 실시간 모니터링 (v1.0)
- **목적**: 작업 컨텍스트 전환 없이 에이전트별 사용량·리셋 카운트다운·번레이트·피크 대비 소비량을 1초 내 파악.
- **메뉴 막대 아이콘 & 노치 안전 레이아웃**:
  - 기본값은 **초컴팩트 모드**(앱 아이콘 + 1순위 에이전트 핵심 수치 1개 또는 미니 3-바 인디케이터)로 14"/16" MacBook 노치 공간 짤림 방지.
  - 다중 에이전트 텍스트 병렬 표시는 설정 내 선택형 확장 옵션.
- **팝오버 구성 (폭 ~360pt) & Adaptive Baseline**:
  - **Gemini CLI (`실측`)**: 오늘 요청 수(`예: 412 / 1,500 req`), 리셋 카운트다운(PT 자정 기준), 오늘 환산 비용.
  - **Claude Code / Codex CLI (`추정`)**: 공식 5시간 상한이 비공개인 기본 상태에서는 **경과·리셋 카운트다운(`2h 14m 남음`) + 현재 블록 누적 토큰/환산 비용 + 번레이트(`$/h`) + 과거 5시간 최대 피크 대비 상대 바** 표시.
  - `%` 게이지는 사용자가 기준 한도를 직접 입력하거나, 과거 429 한도 차단 이력을 학습하거나, v2 Tier 2 동기화 연결 시에만 활성화.
  - 최근 활성 세션 효율 배지 1줄 요약 + "대시보드 열기(`Cmd+D`)" + "수동 새로고침".
- **알림 및 WidgetKit 정책**:
  - 임계치 알림(`70% 주의`, `90% 임박` — 기준 설정된 에이전트 또는 Gemini 일일 한도 대상)은 **동일 윈도우당 단계별 1회(Once-per-window)** 발송 및 macOS 집중 모드(Focus/DND) 존중.
  - WidgetKit Small / Medium / Large 3종은 App Group `widget-snapshot.json` 기반 표시, 일일 40~70회 리로드 예산 보호를 위해 15분 쿨다운(5% 급변 시 예외) 적용.
- **인수 조건**:
  - `Given` Claude Code 기준 한도가 미입력된 기본 상태 `When` 메뉴 막대 팝오버 조회 `Then` 억지 `%` 대신 5시간 리셋 카운트다운, 누적 비용, 번레이트, 과거 피크 대비 바가 `추정 (CLI 기준)` 배지와 함께 표시됨.
  - `Given` Gemini 일일 사용량이 `70%`에 도달해 주의 알림 1회 발송 후 동일 PT 일자 내 `75%`로 증가 `Then` 중복 알림이 발송되지 않으며 `90%` 도달 시에만 다음 단계 알림 1회 발송됨.

### FR-03. 질문(`User Prompt Turn`)당 토큰 효율 분석 대시보드 & 컨텍스트 위생 코칭 (v1.0)
- **목적**: 질문 단위 비용과 효율 저하 원인을 정량 분해하고 즉각 실행 가능한 CLI 개선 액션 제시.
- **상세 기능**:
  - [[concepts/prompt-efficiency-scoring]]의 6대 지표 및 4개 서브스코어 선형 정규화(0–100점) 산출.
  - **구조적 예외 처리**:
    - 세션 1~2번째 `User Prompt Turn`은 **`Warming Up (점수 보류)`** 처리(Cold-Start 감점 방지).
    - Gemini 4 `< 4,096` 토큰 프리픽스 턴은 `CacheScore` 제외 후 활성 지표 재정규화.
    - Thinking 미지원/미기록 모델은 `ReasoningFitScore` 제외 후 `43.75% / 31.25% / 25.00%` 재정규화.
  - **Zero-Storage On-Demand 미리보기**: DB에 프롬프트 원문 저장 없이 대시보드 턴 상세 조회 시 로컬 원본 파일이 존재하면 **메모리 상에서만 첫 80자 온디맨드 미리보기** 제공.
  - **코칭 트리거**: `/compact`(15턴+ 미압축 또는 `150K~400K` / `≥ 6x` 팽창), `/clear`(신규 주제·장기 노이즈), 도구 출력 필터링(`> 40K` 스파이크), 경량 모델 전환(`Thinking Overhead ≥ 75%`).
- **인수 조건**:
  - `Given` 신규 세션의 1~2번째 질문 `When` 효율 대시보드 조회 `Then` `0~40점` 등 저점수 대신 `Warming Up (점수 보류)` 상태 배지가 표시됨.
  - `Given` 로컬 원본 JSONL 파일이 존재하는 세션의 특정 턴 클릭 `When` 메모리 온디맨드 미리보기 실행 `Then` UI에 첫 80자만 표시되며 SQLite DB 및 `widget-snapshot.json`에는 원문 텍스트 0바이트 저장 확인.

### FR-04. 다중 에이전트·프로젝트·모델 통합 소비 대시보드 (v1.0)
- **목적**: 여러 코딩 에이전트·프로젝트·과금 모드에 분산된 소비량·비용·효율을 단일 화면에서 비교.
- **기능 명세**:
  - 기간 필터(오늘 / 현재 결제 주기 / 7일 / 30일 / 90일 / 전체) 및 에이전트·모델·프로젝트·Auth 모드(`Subscription` vs `API Key`) 다중 필터.
  - 사용자별 **구독 결제일(Billing Cycle Day, 기본 매월 1일)** 설정 지원.
  - 프로젝트별 소비 Top-N(Gemini `<project_hash>` 3단계 매핑 이름 반영) 및 구독 환산 가치($V_{\text{api}}$) vs 실제 API 현금 지출($C_{\text{cash}}$) 분리 표시.
- **인수 조건**:
  - `Given` 사용자가 동일 머신에서 구독 로그인 세션과 `GEMINI_API_KEY` 세션을 모두 실행 `When` Overview 탭에서 현재 결제 주기 조회 `Then` 구독 소비($V_{\text{api}}$)와 종량제 API 지출($C_{\text{cash}}$)이 분리 합산되어 표시됨.

### FR-05. 버전 관리 외부 단가·요금제 카탈로그 및 로컬 오버라이드 (v1.0)
- **목적**: AI 벤더 단가·프로모션 종료일·모델명 변경 시 앱 재빌드 없이 정확한 비용 계산 유지.
- **기능 명세**:
  - 번들 내 기본 `pricing-catalog.json` 탑재(`effective_from` / `effective_until`, 캐시 배수, 장문 `>272K` 할증 지원) + Sparkle 앱 업데이트 시 번들 갱신.
  - 사용자 로컬 오버라이드(`pricing-overrides.json`) 및 UI 편집기 지원.
  - **Opt-in 정적 단가 자동 동기화**: 기본값 **비활성(100% 오프라인)**, 설정에서 Opt-in 시에만 GitHub 공식 정적 `pricing-catalog.json` 읽기 전용 동기화(사용자 데이터 송신 0건).
  - 미등록 모델 발견 시 패밀리 폴백 단가 + `미등록 모델(추정 단가)` 알림.
- **인수 조건**:
  - `Given` 단가 자동 동기화가 기본 비활성인 상태 `When` 앱 실행 중 `Then` 외부 단가 서버 요청 0건이며, 사용자가 Opt-in 토글 시에만 읽기 전용 JSON GET 요청이 1일 1회 또는 수동 실행됨.

### FR-06. 요금제 ROI 분석기 & 플랜 변경 추천 엔진 (v2.0)
- **목적**: 결제 주기(최소 14~30일) 누적 데이터를 바탕으로 구독 플랜 적정성 평가 및 변경 추천.
- **기능 명세**: [[concepts/plan-roi-recommendation]] 산식($V_{\text{api}}$, $R$, 한도 마찰 횟수, 반사실적 낭비 보정 소비액 $V_{\text{adj}} = \max(0.25\, V_{\text{api}}, V_{\text{api}} - (\Delta C_{\text{bloat}} + \Delta C_{\text{cache}} + \Delta C_{\text{reasoning}}))$) 및 의사결정 매트릭스 구현.
- **인수 조건**:
  - `Given` 데이터 수집 기간 14일 미만 `When` Plan ROI 탭 진입 `Then` 추천 보류("데이터 수집 중 X/14일") 표시.
  - `Given` $V_{\text{adj}} < P_{\text{plan}} < V_{\text{api}}$ `When` ROI 분석 실행 `Then` 업그레이드 대신 "플랜 유지 + `/compact`·모델 라우팅 효율 개선 우선"이 1순위 권고로 제시됨.

### FR-07. 선택적 Tier 2 공식 API/OAuth 및 CLI 상태 동기화 (v2.0)
- **목적**: Opt-in 사용자에 한해 공유 풀 구독 계정의 서버 측 실제 잔여 한도(%) 동기화.
- **기능 명세**: `macOS Keychain` 자격 증명 보호, 조회 성공 시 UI 배지를 `추정 (CLI 기준)` → `공식 동기화`로 승격.
- **인수 조건**:
  - `Given` Tier 2 연동 비활성(기본값) `When` 앱 실행 중 `Then` 외부 벤더 API 요청 0건 보장.

---

## 6. 정보 구조 (IA), UX 플로우 및 상주 앱 생명주기

### 6.1 메뉴 막대 & 팝오버 구조
- **메뉴 막대 상단 바 (기본 초컴팩트)**: `[아이콘] 412/1.5k` (1순위 에이전트) 또는 미니 3-바 인디케이터 (확장 다중 텍스트는 옵션).
- **팝오버 레이아웃 (폭 ~360pt)**: 헤더(동기화 시각·새로고침·대시보드) → 에이전트 카드 3단(Gemini `실측` / Claude `추정` / Codex `추정`) → 활성 세션 효율 배너.

### 6.2 WidgetKit 위젯 3종 규격
- **Small (2x2)**: 1순위 에이전트 핵심 게이지 + 오늘 소비량 + 리셋 카운트다운.
- **Medium (4x2)**: CLI 3종 요약 바 + 오늘 총 API 환산 비용 + 평균 효율 배지.
- **Large (4x4)**: CLI 3종 상세 상태 + 7일 소비 스파크라인 + 최근 세션 효율 경고 최대 2건.

### 6.3 대시보드 윈도우 IA (4개 메인 탭)
1. **Overview**: 결제 주기·기간 필터, KPI 카드($V_{\text{api}}$·$C_{\text{cash}}$ 분리), 에이전트/모델별 추이 차트, 프로젝트 Top-N.
2. **Prompt Efficiency**: 6대 지표 & 정규화 점수 카드(Cold-Start 보류 표시 포함), 턴 워터폴 차트, 80자 메모리 미리보기, `/compact`·`/clear`·모델 전환 시뮬레이션.
3. **Sessions & Projects**: 세션/프로젝트 탐색기, Auth 모드(`Subscription`/`API Key`) 태그, Gemini 프로젝트 별칭 편집기, `Low-Confidence` 필터.
4. **Plan ROI & Settings**:
   - v1: 구독 플랜·결제일(Billing Cycle Day) 설정, 리셋 시간대(PT/로컬) 토글, 5시간 기준선 수동 입력, CLI 경로 & 워크스페이스 루트 설정, 단가 오버라이드 & Opt-in 단가 동기화, 로그인 시 자동 실행(`SMAppService`).
   - v2: Plan ROI 추천 카드, Tier 2 Opt-in 연동.

### 6.4 최초 실행 온보딩 플로우
1. 자동 경로 스캔(`~/.gemini`, `~/.claude`, `~/.codex`, `$CODEX_HOME`) + 워크스페이스 루트 폴더 지정(Gemini `<project_hash>` 매핑용).
2. 프로바이더별 현재 요금제·결제일·기본 Auth 모드 선택.
3. 메뉴 막대 초컴팩트/확장 모드 및 WidgetKit 추가 안내 → 백그라운드 초기 적재 시작.

### 6.5 앱 생명주기 및 빈 상태 / 오류 복구 UX
- **상주 생명주기**: `SMAppService.mainApp` 로그인 자동 실행 지원. 메뉴 막대 전용 시 `NSApp.setActivationPolicy(.accessory)` 유지, 대시보드 열림 시 `.regular` 전환 후 창 닫기 시 앱 종료 없이 `.accessory` 복귀.
- **빈 상태(Empty State)**: CLI 미설치 또는 로그 0건 시 에이전트별 설치/실행 가이드 카드 표시(빈 차트 방치 금지).
- **손상 로그 격리 UX**: 파싱 오류 파일 발생 시 전체 수집 중단 없이 해당 파일만 격리(`ingest_errors`)하고 Settings 내 "손상 로그 목록 확인·무시/재시도" 제공.

---

## 7. 비기능 요구사항 (NFR) 및 macOS 아키텍처 기준

### 7.1 지원 OS 정책 및 기술 스택
- **최소 OS 버전 정책**: 완성·배포 시점 기준 Apple 공식 보안 지원 3개 메이저 버전 정책 적용([[concepts/macos-menubar-widget-architecture]]).
  - **2026년 말 ~ 2027년 상반기 출시 기준**: **`macOS 15.0 Sequoia`** 최소 지원 (`macOS 15`, `macOS 26`, `macOS 27` 커버).
  - **2027년 가을 macOS 28 출시 이후**: **`macOS 26.0 Tahoe`**로 최소 버전 상향.
- **스택**: Swift 6 / SwiftUI (`MenuBarExtra`, `Window`, `Swift Charts`), `WidgetKit`, `CoreServices(FSEvents)`, `SQLite` (GRDB.swift 등), `SMAppService`, `Sparkle 2`.

### 7.2 성능 및 안정성 예산

| 항목 | 목표 기준 | 검증 방법 |
| :--- | :--- | :--- |
| **유휴 CPU 점유율** | `< 1.0%` | Activity Monitor / Instruments Time Profiler |
| **상주 메모리(메뉴 막대 전용 시)** | `< 80MB` (대시보드 열림 시 `< 180MB`) | Instruments Allocations / VM Tracker |
| **증분/델타 파싱 지연** | P95 `< 500ms` (일반 턴 추가 시) | 10MB JSONL & 대형 Gemini JSON 벤치마크 |
| **초기 대량 적재(1,000 세션)** | `< 5초` (백그라운드 스레드, UI 프리징 0) | 샘플 픽스처 스위트 부하 테스트 |
| **WidgetKit 리로드 예산** | 일일 `< 48회` (시스템 상한 40~70회 이내) | 리로드 카운터 로깅 검증 |

### 7.3 프로세스·배포 아키텍처
- **비샌드박스 메인 앱 + 샌드박스 WidgetKit**: 메인 앱 비샌드박스 + Hardened Runtime, WidgetKit `.appex`는 App Group 컨테이너 경량 스냅샷(`widget-snapshot.json`) 읽기 전용 조회.
- **배포 파이프라인**: Apple Developer ID Application 서명 → `xcrun notarytool` 공증 → GitHub Releases `.dmg` + Sparkle 2 `appcast.xml` → Homebrew Cask 배포.

![[visuals/system-architecture.excalidraw]]

---

## 8. 데이터 정확도 거버넌스 & 프라이버시/보안 원칙

### 8.1 정확도 거버넌스 (Two-Tier Accuracy)
- [[concepts/two-tier-telemetry-accuracy]]에 따라 UI 배지로 출처 신뢰도 명시:
  1. **`실측 (Local)`**: 로컬 로그 파싱 토큰, Gemini 일일 요청 수, API 환산 비용($V_{\text{api}}$ / $C_{\text{cash}}$).
  2. **`추정 (CLI 기준)`**: Claude/Codex 5시간 블록 Adaptive Baseline 및 조건부 `%` 추정치.
  3. **`Low-Confidence`**: Claude Code 스트리밍 플레이스홀더(`0`/`1`/`2`) 감지 턴.
  4. **`공식 동기화` (v2)**: Opt-in Tier 2 동기화 완료 값.

### 8.2 프라이버시·보안 가드레일
- **Zero-Storage On-Demand 원칙**: SQLite DB와 `widget-snapshot.json`에 프롬프트 원문·코드 스니펫·도구 출력 원문 저장 금지(대시보드 상세 80자 미리보기는 원본 파일 잔존 시 메모리에서만 온디맨드 추출).
- **네트워크 격리 (v1)**: 기본 오프라인 100%(Sparkle 체크 및 사용자 Opt-in 정적 단가 JSON 동기화 제외).
- **자격 증명 보호 (v2)**: Opt-in API/OAuth 토큰은 `Keychain Services` (`kSecAttrAccessibleAfterFirstUnlock`)에만 저장.

![[visuals/telemetry-pipeline-and-accuracy.excalidraw]]

---

## 9. 기술 리스크·가정 및 완화 전략 (Risk Matrix)

| 리스크 ID | 리스크 내용 | 발생 가능성 | 영향도 | 완화 전략 |
| :--- | :--- | :---: | :---: | :--- |
| **R-01** | **Claude Code JSONL 플레이스홀더/중복**: `input/output_tokens`가 `1`/`2`로 기록되거나 `requestId` 중복 | 높음 | 높음 | `requestId` 최종 청크 병합, `Low-Confidence` 플래그 부여, 실제 로컬 픽스처 사전 검증 |
| **R-02** | **Gemini CLI `sessionRetention` 자동 삭제 & 덮어쓰기**: `30d`/`50개` 경과 시 삭제 및 단일 JSON 덮어쓰기 | 높음 | 높음 | FSEvents 즉시 `mtime` 델타 파싱 + 로컬 SQLite 영구 적재 + 백그라운드 누락 스캔 |
| **R-03** | **WidgetKit 리로드 예산(40~70회/일) 고갈**: 잦은 CLI 응답 시 위젯 갱신 멈춤 | 중간 | 중간 | 1~2초 디바운스 + 15분 쿨다운(5% 급변 예외) + `TimelineProvider` 미래 엔트리 생성 |
| **R-04** | **CLI 업데이트에 따른 로그 스키마 변경**: 업그레이드 후 경로·키·`tool_result` 구조 변경 | 높음 | 중간 | 버전별 Parser Strategy 어댑터, 손상 파일 `ingest_errors` 격리·경고 배지 노출 |
| **R-05** | **벤더 단가·구독 한도 급변**: 2026-12-01 Gemini 단가 인상 등 빈번한 가격 변동 | 높음 | 중간 | 외부 `pricing-catalog.json`(`effective_from`) + 사용자 오버라이드 + Opt-in 정적 동기화 |
| **R-06** | **공유 풀 오차 및 비공개 5h 분모**: 웹/모바일 혼용 및 미공개 한도로 `%` 괴리 | 높음 | 중간 | Adaptive Baseline(카운트다운·비용·피크 바) 기본 표기, 기준선 수동/429 학습, v2 Tier 2 동기화 |
| **R-07** | **Auth 모드 혼용 및 `<project_hash>` 미매핑**: 구독/API 키 혼재 및 Gemini 해시 식별 불가 | 중간 | 중간 | 환경변수/설정 기반 Auth 자동 감지 + 수동 태깅, 3단계 프로젝트명 폴백 매핑 |

---

## 10. 설계 착수 전 검증 체크리스트 (Pre-Design Validation Gate)

구체적인 Swift 아키텍처 설계(TDD/ADR, `agents-usage-mac-wiki#3`) 착수 전, 로컬 머신의 실제 로그를 대상으로 아래 6개 항목 검증 필수:

1. **실제 Gemini CLI 로그 픽스처 검증**: `~/.gemini/tmp/*/chats/*.json` 샘플 3건 이상에서 파일 덮어쓰기 패턴 여부, `input`/`output`/`cached`/`thoughts` 키 경로, 모델명 및 프로젝트 메타데이터 포함 여부 확인.
2. **Gemini `<project_hash>` 해시 알고리즘 역추적**: 프로젝트 루트 경로 문자열과 `<project_hash>`(SHA-256 등) 일치 여부 실증.
3. **실제 Claude Code 로그 픽스처 검증**: `~/.claude/projects/*/*.jsonl` 샘플에서 `requestId` 중복 빈도, `input_tokens`/`output_tokens` 플레이스홀더, `tool_result`(`role: "user"`)·`/compact`·서브에이전트 레코드 구분 키 확인.
4. **실제 Codex CLI 로그 픽스처 검증**: `~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl` (및 `$CODEX_HOME`) 샘플에서 `cache_read`/`cache_write`/모델명/`cwd` 및 Auth 모드 단서 확인.
5. **CLI 버전별 차이 기록**: 설치된 `gemini`, `claude`, `codex` CLI 현재 버전 확인 및 파서 어댑터 v1 타깃 버전 고정.
6. **Apple Developer Team / App Group ID 예약 확인**: WidgetKit + App Group 프로비저닝에 사용할 Bundle Identifier 및 App Group 문자열 확정.
