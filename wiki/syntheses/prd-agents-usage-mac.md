---
id: prd-agents-usage-mac
type: synthesis
aliases:
  - "Agents Usage Mac PRD"
  - "제품 기획서 v1.2"
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
  - "[[sources/oauth-tos-constraints]]"
status: active
---

# 제품 기획서 (PRD): Agents Usage Mac

- **문서 버전**: **v1.2 (Design-Ready Freeze)** — 1·2차 리뷰(P0/P1/P2) 및 OAuth/ToS 제약 검증 완료, 기준일 `2026-11-27`
- **제품 형태**: macOS 네이티브 메뉴 막대 앱 + WidgetKit 위젯 + 대시보드 윈도우 (MIT 오픈소스 · 무료, DMG & Homebrew Cask 배포)
- **지원 언어 / 통화**: 영어 + 한국어(`.xcstrings` 동시 지원) · USD(`$`) 기본 + 선택적 KRW(`₩`) 수동 환율 참고 병기
- **저장소 구성**: 기획·설계·검증(`YongsDevStudy/agents-usage-mac-wiki`) ↔ 앱 구현(`YongsDevStudy/agents-usage-mac`)
- **근거 기반**: [[sources/market-competitors]], [[sources/local-cli-telemetry]], [[sources/pricing-quotas-2026]], [[sources/prompt-efficiency-metrics]], [[sources/macos-architecture-constraints]], [[sources/oauth-tos-constraints]]

---

## 1. Executive Summary & 제품 비전

### 1.1 배경 및 핵심 문제 정의
- 터미널 코딩 에이전트([[entities/gemini-cli|Gemini CLI]], [[entities/claude-code|Claude Code]], [[entities/codex-cli|Codex CLI]]) 병행 사용이 일반화됨에 따라 다음 4가지 실무 페인포인트 심화:
  1. **한도 예측 불능**: Claude Pro/Max·ChatGPT Plus/Pro의 5시간·주간 롤링 한도와 Gemini CLI 일일 요청 한도(1,000~2,000 req/day) 소진 시점 사전 예측 불가.
  2. **질문(턴)당 토큰 누수 미가시화**: 무상태(Stateless) LLM의 컨텍스트 재전송 구조로 인해 세션 장기화 시 비용·토큰 소비가 이차적으로 폭증하나 턴 단위 비효율 원인 식별 불가.
  3. **요금제 ROI 근거 부재**: 현재 월 구독 플랜($20~$500) 대비 실제 소비 가치 및 한도 마찰 빈도를 비교할 정량 데이터 부족(구독·API 키 혼용 시 구분 어려움 포함).
  4. **다중 에이전트 분산**: 에이전트별 로그 포맷·저장 경로·리셋 시간대·과금 체계가 상이해 통합 소비량·모델별 효율 단일 뷰 부재.

### 1.2 제품 포지셔닝
- **한 줄 정의**: *"로컬 코딩 에이전트의 실시간 한도 감시부터 질문당 토큰 효율 코칭, 요금제 ROI 최적화까지 완결하는 프라이버시·ToS 준수 네이티브 macOS 컴패니언"*
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
| **상시 코딩 중** | **메뉴 막대 아이콘 & WidgetKit 위젯** | 주력 에이전트(Gemini CLI 일일 `API Request` 총합, PT 자정 리셋 기준) 및 활성 블록(Claude/Codex 5시간 경과·누적 비용·피크 대비 바) 상시 확인 |
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
| **100% 로컬 · ToS 준수 기본값** | 혼합(쿠키/토큰) | ✅ | 혼합 | 혼합 | **✅ (Strict Local & No Scraping)** |
| **질문당 토큰 효율 진단·코칭** | ❌ | ❌ | ❌ | ❌ | **✅ (6대 지표 + 정규화 점수)** |
| **요금제 ROI 비교·변경 추천** | ❌ | ❌ | ❌ | ❌ | **✅ (v2 반사실적 $V_{\text{adj}}$)** |

### 3.2 차별화 쐐기 (Differentiation Wedge)
1. **모니터링을 넘어선 행동 변화(Actionable Coaching)**: `User Prompt Turn` 단위로 *"최근 10턴 동안 미압축 컨텍스트 재전송으로 질문당 비용 3.4배 증가 — `/compact` 실행 권장"* 제시.
2. **Gemini CLI 보존 공백 해결**: `general.sessionRetention`(`maxAge: 30d`, `maxCount: 50`) 자동 삭제 이전에 로컬 SQLite(상세 턴 + 일별 롤업)에 무제한 영구 적재.
3. **데이터 정직성 & ToS 안전성**: 비공개 5시간 상한에 억지 `%`를 매기지 않고 Adaptive Baseline 적용, 소비자 OAuth 스크래핑 배제(v2 Claude Code Statusline Bridge 등 합법 경로만 사용).

![[visuals/competitive-positioning.excalidraw]]

---

## 4. 제품 범위 & 단계별 로드맵 (v1 / v2 / v3) 및 Non-Goals

### 4.1 MoSCoW 범위 정의

| 우선순위 | 범위 항목 | 대상 버전 |
| :--- | :--- | :---: |
| **Must Have** | Gemini CLI(1순위)·Claude Code·Codex CLI 이원화 로그 수집(JSONL `byte_offset` / JSON 델타), Wake Catch-up, SQLite 무제한 영구 저장(삭제권·내보내기 포함) | **v1.0** |
| **Must Have** | 노치 안전 메뉴 막대(`MenuBarExtra`) + Adaptive Baseline 팝오버(멀티 세션 우선순위) + WidgetKit(S/M/L) 위젯 | **v1.0** |
| **Must Have** | 질문(`User Prompt Turn`)당 효율 분석 대시보드(6대 지표, 통일 임계치, `/compact` 40% 자동 감지, 메모리 80자 미리보기) | **v1.0** |
| **Must Have** | 다중 에이전트·프로젝트(Gemini 해시 3단계 역매핑)·Auth 모드(비민감 CLI 설정+기본 모드+수동 태깅) 통합 뷰 및 `EN/KO` · `USD/KRW` 지원 | **v1.0** |
| **Should Have** | 요금제 ROI 비교기($V_{\text{api}}$ / $V_{\text{adj}}$) & 업/다운그레이드/API 하이브리드 추천 엔진 | **v2.0** |
| **Should Have** | Opt-in Tier 2 합법 연동: **Claude Code 공식 Statusline `stdin` 브리지**, Gemini GCP/Vertex OAuth, Codex CLI `/status` 프로브·Org API | **v2.0** |
| **Could Have** | Gemini CLI 로컬 OTel(`4317`) 수신기, 선택적 로컬 LLM(Apple Intelligence/Ollama) 프롬프트 리뷰 | **v2.1+** |
| **Won't Have (v1/v2)** | AI IDE(Cursor·Antigravity·Copilot·Windsurf), 종량제 API 키 전용 대시보드, 웹 챗봇 통합 | **v3.0+** |

### 4.2 마일스톤 로드맵
- **v1.0 (MVP — Local Monitor & Efficiency Coach)**: 100% 로컬 처리, CLI 3종 파서, SQLite 영구 적재(무제한·삭제·내보내기), 메뉴 막대 + WidgetKit + 대시보드 4개 탭 + 외부 단가표(Opt-in 정적 JSON 동기화), `EN/KO (.xcstrings)`.
- **v1.1 (Hardening & UX Polish)**: 임계치 알림 고도화, 프로젝트별 예산 알림, Sparkle 2 자동 업데이트·Homebrew Cask 배포.
- **v2.0 (Plan ROI & ToS-Compliant Tier 2 Sync)**: `Plan ROI` 추천 탭, **Claude Code 공식 Statusline Bridge**, Codex CLI 프로브/Org API, Gemini GCP/Vertex OAuth 연동.
- **v3.0+ (Ecosystem Expansion)**: AI IDE 확장(Cursor, Antigravity, Copilot, Windsurf), 종량제 API 키, 웹 챗봇 통합.

### 4.3 명시적 비목표 (Non-Goals)
1. **프롬프트·소스코드 원문 저장·외부 전송 금지**: DB에 원문 미저장(로컬 파일 잔존 시 메모리 80자 온디맨드 미리보기만 허용), v1에서 사용자 데이터 네트워크 송신 0건.
2. **소비자 OAuth 무임승차 및 브라우저 쿠키 스크래핑 영구 배제**: 3사 약관 위반 소지가 있는 소비자 자격 증명 수집·중개·비공식 내부 쿼터 엔드포인트(`wham/usage`, `/api/oauth/usage` 등) 호출 금지([[sources/oauth-tos-constraints]]).
3. **시크릿 파일·터미널 환경변수 스캔 금지**: `.env`, `.zshrc`, API 키/OAuth 토큰 원문 파일 열람 금지.
4. **LLM 기반 실시간 프롬프트 채점(v1 제외)** 및 **팀/서버 동기화 SaaS(제외)**.

---

## 5. 상세 기능 요구사항 (FR-01 ~ FR-07)

### FR-01. 다중 CLI 로컬 로그 이원화 수집·정규화 엔진 (v1.0)
- **목적**: 로컬 코딩 에이전트 3종의 로그를 비침습적으로 감지·파싱해 통합 데이터 모델로 영구 저장.
- **이원화 수집 전략**([[concepts/macos-menubar-widget-architecture]]):
  - **Append-Only JSONL (`byte_offset` 증분)**: Claude Code(`~/.claude/projects/<project-path>/<session-id>.jsonl`), Codex CLI(`~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl`, `$CODEX_HOME` 지원).
  - **Full-JSON Snapshot Delta (`mtime` + `file_size` + `messages.count`/턴 ID 델타)**: Gemini CLI(`~/.gemini/tmp/<project_hash>/chats/session-*.json`).
  - **Catch-up 복구**: 앱 실행 시 및 `NSWorkspace.didWakeNotification`(잠자기 복귀) 수신 시 전체 감시 경로 델타 스캔 및 5시간 블록 타이머 재동기화.
- **이벤트 3분류 및 단위 분리**:
  - `User Prompt Turn` / `Agent Loop Step` / `System & Subagent Overhead` 엄격 분리(`tool_result`가 `role: "user"`로 기록되는 경우 질문 턴 분모에서 제외).
  - **일일 쿼터 분자 vs 효율 분석 분모**: Gemini CLI 일일 한도(`1,000/1,500/2,000 req/day`) 카운팅 분자는 **`API Request` 총합**(`User Prompt Turn` + 유발 `Agent Loop Step` + `System Overhead`), 질문당 효율 분석 분모만 **`User Prompt Turn`** 적용.
- **Auth 모드 및 메타데이터 보강 (시크릿 미열람)**:
  - macOS `launchd` GUI 앱은 터미널 환경변수를 상속하지 못하므로 환경변수 감지 배제.
  - **① CLI 설정 파일의 비민감 인증 타입 메타데이터(`selectedAuthType` 등, 키·토큰 원문 접근 금지) + ② 프로바이더 기본 과금 모드 + ③ 프로젝트/세션 수동 태깅**으로 `Subscription` vs `API Key` 구분.
  - **Gemini `<project_hash>` 3단계 역매핑**: ① CLI 메타데이터 자동 추출 → ② 지정 워크스페이스 루트 경로 해시 매칭 → ③ 수동 프로젝트 별칭 편집.
  - **시간대 기준**: Gemini CLI 일일 쿼터는 **Pacific Time(PT) 자정** 기본 적용 + 로컬 자정 토글 지원.
- **중복·이상치·영구 보존 및 삭제권**:
  - Claude Code `requestId`/`message.id` 기준 최종 레코드 병합, 플레이스홀더(`input/output <= 2`) 감지 시 `confidence = low` 부여.
  - SQLite **무제한 영구 보관**(상세 턴 + 일별 롤업 테이블), Settings에서 **특정 프로젝트/세션 영구 삭제·전체 DB 초기화·JSON/CSV 내보내기** 제공.
  - 파싱 실패 파일은 `ingest_errors` 격리 테이블에 기록 후 경고 배지 표시.
- **인수 조건 (Acceptance Criteria)**:
  - `Given` Gemini CLI 질문 1건(`User Prompt Turn = 1`) 동안 8회 도구 호출 루프 발생 `When` 일일 쿼터 및 효율 통계 집계 `Then` Gemini 일일 쿼터 카운터에는 `9 requests`로 반영되고, 효율 대시보드 질문당 비용은 `1 User Prompt Turn`에 합산 표시됨.
  - `Given` Mac이 3시간 잠자기 후 복귀(`didWakeNotification`) `When` 그사이 외부 스크립트·이전 세션 변경분 존재 `Then` 2초 이내 Catch-up 스캔으로 누락 턴이 DB에 적재되고 5시간 타이머가 재동기화됨.
  - `Given` 사용자가 Settings에서 특정 프로젝트 삭제 실행 `When` 삭제 확인 `Then` 해당 프로젝트의 `sessions`, `turns`, `api_requests`, 일별 롤업 및 캐시 스냅샷에서 즉시 영구 제거됨.

### FR-02. 노치 안전 메뉴 막대 팝오버 & WidgetKit 실시간 모니터링 (v1.0)
- **목적**: 작업 컨텍스트 전환 없이 에이전트별 사용량·리셋 카운트다운·번레이트·피크 대비 소비량을 1초 내 파악.
- **메뉴 막대 아이콘 & 노치 안전 레이아웃**:
  - 기본값 **초컴팩트 모드**(앱 아이콘 + 1순위 에이전트 핵심 수치 1개 또는 미니 3-바 인디케이터), 확장 다중 텍스트는 옵션.
- **팝오버 구성 (폭 ~360pt) & Adaptive Baseline**:
  - **Gemini CLI (`실측`)**: 오늘 `API Request` 수(`예: 412 / 1,500 req`), PT 자정 리셋 카운트다운, 오늘 환산 비용.
  - **Claude Code / Codex CLI (`추정`)**: 공식 5시간 상한 미공개 기본 상태에서는 **경과·리셋 카운트다운(`2h 14m 남음`) + 현재 블록 누적 토큰/환산 비용 + 번레이트(`$/h`) + 과거 5시간 최대 피크 대비 상대 바** 표기 (`%`는 기준선 입력·429 학습·v2 Tier 2 연결 시에만 활성).
  - **멀티 세션 1줄 효율 배지 우선순위**: 동시 실행 세션 중 ① 경고 발생 활성 세션(최근 30분 내 이벤트) 최우선 → ② 가장 최근 이벤트 세션 선정.
- **알림 및 WidgetKit 정책**:
  - 임계치 알림(`70% 주의`, `90% 임박`)은 **동일 윈도우당 단계별 1회(Once-per-window)** 발송 및 macOS 집중 모드(Focus/DND) 존중.
  - WidgetKit Small / Medium / Large 3종은 App Group `widget-snapshot.json` 기반, 15분 쿨다운(5% 급변 예외)으로 40~70회/일 예산 준수.
- **인수 조건**:
  - `Given` 2개 터미널에서 각각 세션 A(최근 10분 내 Context Bloat 경고)와 세션 B(방금 1분 전 정상 턴 완료) 실행 `When` 메뉴 막대 팝오버 열기 `Then` 경고 발생 활성 세션인 세션 A가 1줄 효율 배지에 우선 노출됨.
  - `Given` Claude Code 기준 한도가 미입력된 기본 상태 `When` 팝오버 조회 `Then` `%` 대신 카운트다운·누적 비용·번레이트·피크 대비 바가 `추정 (CLI 기준)` 배지와 함께 표시됨.

### FR-03. 질문(`User Prompt Turn`)당 토큰 효율 분석 대시보드 & 컨텍스트 위생 코칭 (v1.0)
- **목적**: 질문 단위 비용과 효율 저하 원인을 정량 분해하고 즉각 실행 가능한 CLI 개선 액션 제시.
- **상세 기능**:
  - [[concepts/prompt-efficiency-scoring]]의 6대 지표 및 4개 서브스코어 선형 정규화(0–100점) 산출.
  - **통일 임계치 및 `/compact` 자동 감지**: `ContextHygieneScore = min(Score_B, Score_T)` (100점 `B≤3x & T≤10`, 경고 `B≥6x | T≥15`, 0점 `B≥8x | T≥25`). 직전 턴 대비 입력 토큰 **`40%` 이상 급감** 시 압축 이벤트로 자동 감지해 $T=0$ 리셋.
  - **예외 처리**: Cold Start(1~2턴 `Warming Up` 보류), Gemini 4 `< 4,096` 프리픽스 캐시 제외, Thinking 미지원/미기록 모델 가중치 재정규화(`43.75% / 31.25% / 25.00%`).
  - **Zero-Storage On-Demand 미리보기**: DB 원문 저장 없이 로컬 원본 파일 잔존 시 **메모리 상에서만 첫 80자 온디맨드 미리보기** 제공.
- **인수 조건**:
  - `Given` 세션 16턴째(`T=16`)에서 사용자가 `/compact`를 실행해 다음 턴 입력 토큰이 180K → 42K(`76.7%` 감소)로 줄어듦 `When` 효율 점수 재계산 `Then` 압축 이벤트가 자동 감지되어 $T=0$으로 리셋되고 `ContextHygieneScore`가 즉시 회복됨.
  - `Given` 로컬 원본 JSONL 파일이 존재하는 세션의 특정 턴 클릭 `When` 메모리 온디맨드 미리보기 실행 `Then` UI에 첫 80자만 표시되며 SQLite DB 및 `widget-snapshot.json`에는 원문 텍스트 0바이트 유지.

### FR-04. 다중 에이전트·프로젝트·모델 통합 소비 대시보드 (v1.0)
- **목적**: 여러 코딩 에이전트·프로젝트·과금 모드에 분산된 소비량·비용·효율을 단일 화면에서 비교.
- **기능 명세**:
  - 기간 필터(오늘 / 현재 결제 주기 / 7일 / 30일 / 90일 / 전체), 에이전트·모델·프로젝트·Auth 모드 필터, **구독 결제일(Billing Cycle Day, 기본 매월 1일)** 설정.
  - 프로젝트별 소비 Top-N(Gemini `<project_hash>` 3단계 매핑 이름 반영), 구독 환산 가치($V_{\text{api}}$) vs 실제 API 현금 지출($C_{\text{cash}}$) 분리, USD(`$`) 기본 + 선택적 KRW(`₩`) 수동 환율 참고 표기.
- **인수 조건**:
  - `Given` KRW 환율 병기 옵션(`1,380원/$`)이 켜진 상태 `When` Overview 탭 조회 `Then` 기본 USD 금액 옆에 보조 괄호(`₩XX,XXX`)가 함께 표기되며, 내부 ROI 연산은 USD 기준으로 일관 유지됨.

### FR-05. 버전 관리 외부 단가·요금제 카탈로그 및 로컬 오버라이드 (v1.0)
- **목적**: AI 벤더 단가·프로모션 종료일·모델명 변경 시 앱 재빌드 없이 정확한 비용 계산 유지.
- **기능 명세**: 번들 `pricing-catalog.json`(`effective_from`/`effective_until`, 캐시 배수, 장문 `>272K` 할증) + Sparkle 갱신 + 사용자 `pricing-overrides.json` + **Opt-in 정적 단가 JSON 동기화**(기본 비활성, 사용자 데이터 송신 0건) + 미등록 모델 패밀리 폴백.
- **인수 조건**:
  - `Given` 단가 자동 동기화 비활성(기본값) 상태 `When` 앱 실행 중 `Then` 외부 단가 서버 요청 0건 보장.

### FR-06. 요금제 ROI 분석기 & 플랜 변경 추천 엔진 (v2.0)
- **목적**: 결제 주기(최소 14~30일) 누적 데이터를 바탕으로 구독 플랜 적정성 평가 및 변경 추천.
- **기능 명세**: [[concepts/plan-roi-recommendation]] 산식($V_{\text{api}}$, $R$, 한도 마찰 횟수, 반사실적 $V_{\text{adj}} = \max(0.25\, V_{\text{api}}, V_{\text{api}} - (\Delta C_{\text{bloat}} + \Delta C_{\text{cache}} + \Delta C_{\text{reasoning}}))$) 및 의사결정 매트릭스 구현.
- **인수 조건**:
  - `Given` $V_{\text{adj}} < P_{\text{plan}} < V_{\text{api}}$ `When` ROI 분석 실행 `Then` 업그레이드 대신 "플랜 유지 + `/compact`·모델 라우팅 효율 개선 우선"이 1순위 권고로 제시됨.

### FR-07. 선택적 Tier 2 ToS 준수 공식·로컬 브리지 동기화 (v2.0)
- **목적**: Opt-in 사용자에 한해 약관(ToS)을 100% 준수하는 합법 경로로 서버 잔여 한도(%) 동기화([[sources/oauth-tos-constraints]]).
- **벤더별 연동 경로**:
  1. **Claude Code Statusline Bridge**: 사용자 Opt-in 시 `~/.claude/settings.json` statusline에 경량 헬퍼 연결 → Claude Code가 `stdin`으로 넘기는 공식 **`rate_limits.five_hour` / `seven_day`** 수신(OAuth 토큰 접근 0건).
  2. **Gemini CLI / GCP**: 개인 소비자 OAuth 무임승차 금지. Vertex AI / GCP API 사용자에 한해 공식 Google OAuth + Cloud Service Usage API 선택 연동.
  3. **OpenAI Codex CLI**: 비공식 `wham/usage` 엔드포인트 배제. CLI `/status`·`/usage` 로컬 실행 출력 파싱 또는 OpenAI API Org 공식 Usage API 연동.
- **보안**: API Org / GCP 자격 증명은 `macOS Keychain` (`kSecAttrAccessibleAfterFirstUnlock`)에만 저장, 조회 성공 시 UI 배지를 `공식 동기화`로 승격.
- **인수 조건**:
  - `Given` 사용자가 v2 Claude Code Statusline Bridge만 Opt-in 활성화 `When` Claude Code 실행 중 턴 완료 `Then` 외부 Anthropic OAuth API 호출이나 `~/.claude/.credentials.json` 열람 없이 `stdin` 전달값만으로 5h/7d 잔여 %가 `공식 동기화` 배지로 반영됨.

---

## 6. 정보 구조 (IA), UX 플로우 및 상주 앱 생명주기

### 6.1 메뉴 막대 & 팝오버 구조
- **메뉴 막대 상단 바 (기본 초컴팩트)**: `[아이콘] 412/1.5k` (1순위 에이전트) 또는 미니 3-바 인디케이터 (확장 다중 텍스트는 옵션).
- **팝오버 레이아웃 (폭 ~360pt)**: 헤더(동기화 시각·새로고침·대시보드) → 에이전트 카드 3단(Gemini `실측` / Claude `추정` / Codex `추정`) → 멀티 세션 우선순위 기반 활성 세션 효율 배너.

### 6.2 WidgetKit 위젯 3종 규격
- **Small (2x2)**: 1순위 에이전트 핵심 게이지 + 오늘 소비량 + 리셋 카운트다운.
- **Medium (4x2)**: CLI 3종 요약 바 + 오늘 총 API 환산 비용 + 평균 효율 배지.
- **Large (4x4)**: CLI 3종 상세 상태 + 7일 소비 스파크라인 + 최근 세션 효율 경고 최대 2건.

### 6.3 대시보드 윈도우 IA (4개 메인 탭, `EN / KO`)
1. **Overview**: 결제 주기·기간 필터, KPI 카드($V_{\text{api}}$·$C_{\text{cash}}$ 분리, USD + 선택 KRW), 에이전트/모델별 추이 차트, 프로젝트 Top-N.
2. **Prompt Efficiency**: 6대 지표 & 정규화 점수 카드(Cold-Start 보류 표시), 턴 워터폴 차트, 80자 메모리 미리보기, `/compact` 자동 감지 마커 및 개선 시뮬레이션.
3. **Sessions & Projects**: 세션/프로젝트 탐색기, Auth 모드(`Subscription`/`API Key`) 태그, Gemini 프로젝트 별칭 편집기, `Low-Confidence` 필터, 개별 프로젝트/세션 삭제.
4. **Plan ROI & Settings**:
   - v1: 구독 플랜·결제일(Billing Cycle Day), 리셋 시간대(PT/로컬) 토글, 5시간 기준선 수동 입력, CLI 경로 & 워크스페이스 루트, 단가 오버라이드 & Opt-in 단가 동기화, 통화/KRW 환율, 로그인 자동 실행(`SMAppService`), JSON/CSV 내보내기 & 전체 DB 초기화.
   - v2: Plan ROI 추천 카드, Tier 2 Opt-in 연동(Claude Statusline Bridge / Codex Probe·Org API / GCP OAuth).

### 6.4 최초 실행 온보딩 플로우
1. 자동 경로 스캔(`~/.gemini`, `~/.claude`, `~/.codex`, `$CODEX_HOME`) + 워크스페이스 루트 폴더 지정(Gemini `<project_hash>` 매핑용).
2. 프로바이더별 현재 요금제·결제일·기본 Auth 모드 선택.
3. 메뉴 막대 초컴팩트/확장 모드 및 WidgetKit 추가 안내 → 백그라운드 초기 적재 시작.

### 6.5 앱 생명주기 및 빈 상태 / 오류 복구 UX
- **상주 생명주기**: `SMAppService.mainApp` 로그인 자동 실행 지원. 메뉴 막대 전용 시 `NSApp.setActivationPolicy(.accessory)`, 대시보드 열림 시 `.regular` 전환 후 창 닫기 시 앱 종료 없이 `.accessory` 복귀.
- **빈 상태(Empty State)**: CLI 미설치 또는 로그 0건 시 에이전트별 설치/실행 가이드 카드 표시.
- **손상 로그 격리 UX**: 파싱 오류 파일 발생 시 `ingest_errors` 테이블에 격리하고 Settings 내 "손상 로그 목록 확인·무시/재시도" 제공.

---

## 7. 비기능 요구사항 (NFR) 및 macOS 아키텍처 기준

### 7.1 지원 OS 정책, 라이선스 및 기술 스택
- **최소 OS 버전 정책**: 완성·배포 시점 기준 Apple 공식 보안 지원 3개 메이저 버전 정책 적용([[concepts/macos-menubar-widget-architecture]]).
  - **2026년 말 ~ 2027년 상반기 출시 기준**: **`macOS 15.0 Sequoia`** 최소 지원 (`macOS 15`, `macOS 26`, `macOS 27` 커버).
  - **2027년 가을 macOS 28 출시 이후**: **`macOS 26.0 Tahoe`**로 최소 버전 상향.
- **라이선스 & 현지화**: **MIT License**, SwiftUI String Catalog(`.xcstrings`) 기반 **영어(기본) + 한국어** 지원.
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
  1. **`실측 (Local)`**: 로컬 로그 파싱 토큰, Gemini 일일 `API Request` 수, API 환산 비용($V_{\text{api}}$ / $C_{\text{cash}}$).
  2. **`추정 (CLI 기준)`**: Claude/Codex 5시간 블록 Adaptive Baseline 및 조건부 `%` 추정치.
  3. **`Low-Confidence`**: Claude Code 스트리밍 플레이스홀더(`0`/`1`/`2`) 감지 턴.
  4. **`공식 동기화` (v2)**: Opt-in ToS 준수 브리지/API 동기화 완료 값.

### 8.2 프라이버시·보안 가드레일
- **Zero-Storage On-Demand 원칙**: SQLite DB와 `widget-snapshot.json`에 프롬프트 원문·코드 스니펫·도구 출력 원문 저장 금지(80자 미리보기는 원본 파일 잔존 시 메모리에서만 온디맨드 추출).
- **시크릿·소비자 자격 증명 미열람**: `.env`, `.zshrc`, `~/.claude/.credentials.json`, `~/.codex/auth.json` 내 비밀 토큰·API 키 값 열람/수집/전송 금지.
- **네트워크 격리 (v1)**: 기본 오프라인 100%(Sparkle 체크 및 Opt-in 정적 단가 JSON 동기화 제외).
- **자격 증명 보호 (v2)**: Opt-in GCP/Org API 토큰은 `Keychain Services` (`kSecAttrAccessibleAfterFirstUnlock`)에만 저장.

![[visuals/telemetry-pipeline-and-accuracy.excalidraw]]

---

## 9. 기술 리스크·가정 및 완화 전략 (Risk Matrix)

| 리스크 ID | 리스크 내용 | 발생 가능성 | 영향도 | 완화 전략 |
| :--- | :--- | :---: | :---: | :--- |
| **R-01** | **Claude Code JSONL 플레이스홀더/중복**: `input/output_tokens` `1`/`2` 또는 `requestId` 중복 | 높음 | 높음 | `requestId` 최종 청크 병합, `Low-Confidence` 플래그 부여, 실제 로컬 픽스처 사전 검증 |
| **R-02** | **Gemini CLI `sessionRetention` 삭제 & 덮어쓰기**: `30d`/`50개` 경과 삭제 및 단일 JSON 덮어쓰기 | 높음 | 높음 | FSEvents 즉시 `mtime` 델타 파싱 + SQLite 영구 적재 + Sleep/Wake Catch-up 스캔 |
| **R-03** | **WidgetKit 리로드 예산(40~70회/일) 고갈**: 잦은 CLI 응답 시 위젯 갱신 멈춤 | 중간 | 중간 | 1~2초 디바운스 + 15분 쿨다운(5% 급변 예외) + `TimelineProvider` 미래 엔트리 생성 |
| **R-04** | **CLI 업데이트에 따른 로그/Statusline 스키마 변경**: 키·`tool_result` 구조 변경 | 높음 | 중간 | 버전별 Parser Strategy 어댑터, 손상 파일 `ingest_errors` 격리·경고 배지 노출 |
| **R-05** | **벤더 단가·구독 한도 급변**: 2026-12-01 Gemini 단가 인상 등 빈번한 가격 변동 | 높음 | 중간 | 외부 `pricing-catalog.json`(`effective_from`) + 사용자 오버라이드 + Opt-in 정적 동기화 |
| **R-06** | **공유 풀 오차 및 비공개 5h 분모**: 웹/모바일 혼용 및 미공개 한도로 `%` 괴리 | 높음 | 중간 | Adaptive Baseline 기본 표기, 기준선 수동/429 학습, v2 Claude Statusline Bridge 등 합법 연동 |
| **R-07** | **Auth 모드 혼용 및 `<project_hash>` 미매핑**: 구독/API 키 혼재 및 Gemini 해시 식별 불가 | 중간 | 중간 | 비민감 CLI 설정 메타데이터 + 수동 태깅(시크릿 미열람), 3단계 프로젝트명 폴백 매핑 |

---

## 10. 설계 착수 전 검증 체크리스트 (Pre-Design Validation Gate)

구체적인 Swift 아키텍처 설계(TDD/ADR, `agents-usage-mac-wiki#3`) 착수 전, 로컬 머신의 실제 로그를 대상으로 아래 6개 항목 검증 필수:

1. **실제 Gemini CLI 로그 픽스처 검증**: `~/.gemini/tmp/*/chats/*.json` 샘플 3건 이상에서 파일 덮어쓰기 패턴 여부, `input`/`output`/`cached`/`thoughts` 키 경로, 모델명·프로젝트 메타데이터·`selectedAuthType` 등 비민감 설정 필드 확인.
2. **Gemini `<project_hash>` 해시 알고리즘 역추적**: 프로젝트 루트 경로 문자열과 `<project_hash>`(SHA-256 등) 일치 여부 실증.
3. **실제 Claude Code 로그 픽스처 검증**: `~/.claude/projects/*/*.jsonl` 샘플에서 `requestId` 중복 빈도, `input_tokens`/`output_tokens` 플레이스홀더, `tool_result`(`role: "user"`)·`/compact`·서브에이전트 레코드 구분 키 확인.
4. **실제 Codex CLI 로그 픽스처 검증**: `~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl` (및 `$CODEX_HOME`) 샘플에서 `cache_read`/`cache_write`/모델명/`cwd` 및 비민감 Auth 구분 단서 확인.
5. **CLI 버전별 차이 기록**: 설치된 `gemini`, `claude`, `codex` CLI 현재 버전 확인 및 파서 어댑터 v1 타깃 버전 고정.
6. **Apple Developer Team / App Group ID 예약 확인**: WidgetKit + App Group 프로비저닝에 사용할 Bundle Identifier 및 App Group 문자열 확정.
