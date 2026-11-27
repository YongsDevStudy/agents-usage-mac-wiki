---
id: prd-agents-usage-mac
type: synthesis
aliases:
  - "Agents Usage Mac PRD"
  - "제품 기획서 v1.0"
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

- **문서 버전**: v1.0 (기획 확정 기준, 기준일 `2026-11-27`)
- **제품 형태**: macOS 네이티브 메뉴 막대 앱 + WidgetKit 위젯 + 대시보드 윈도우 (오픈소스 / 무료, DMG & Homebrew Cask 배포)
- **근거 기반**: [[sources/market-competitors]], [[sources/local-cli-telemetry]], [[sources/pricing-quotas-2026]], [[sources/prompt-efficiency-metrics]], [[sources/macos-architecture-constraints]]

---

## 1. Executive Summary & 제품 비전

### 1.1 배경 및 핵심 문제 정의
- 터미널 코딩 에이전트([[entities/gemini-cli|Gemini CLI]], [[entities/claude-code|Claude Code]], [[entities/codex-cli|Codex CLI]]) 병행 사용이 일반화됨에 따라 다음 4가지 실무 페인포인트 심화:
  1. **한도 예측 불능**: Claude Pro/Max·ChatGPT Plus/Pro의 5시간·주간 롤링 한도와 Gemini CLI 일일 요청 한도(1,000~2,000 req/day) 소진 시점 사전 예측 불가.
  2. **질문(턴)당 토큰 누수 미가시화**: 무상태(Stateless) LLM의 컨텍스트 재전송 구조로 인해 세션 장기화 시 비용·토큰 소비가 이차적으로 폭증하나 턴 단위 비효율 원인 식별 불가.
  3. **요금제 ROI 근거 부재**: 현재 월 구독 플랜($20~$500) 대비 실제 소비 가치 및 한도 마찰 빈도를 비교할 정량 데이터 부족.
  4. **다중 에이전트 분산**: 에이전트별 로그 포맷·저장 경로·과금 체계가 상이해 통합 소비량·모델별 효율 단일 뷰 부재.

### 1.2 제품 포지셔닝
- **한 줄 정의**: *"로컬 코딩 에이전트의 실시간 한도 감시부터 질문당 토큰 효율 코칭, 요금제 ROI 최적화까지 완결하는 프라이버시 우선 네이티브 macOS 컴패니언"*
- **핵심 가치 순환**: **Monitor (메뉴 막대·위젯 실시간 감시) → Diagnose (질문당 효율·컨텍스트 위생 진단) → Optimize (행동 코칭 및 요금제 ROI 최적화)**

### 1.3 북극성 지표(NSM) 및 검증 KPI

| 구분 | 지표명 | 정의 및 산출 기준 | 목표치 (v1/v2 검증 기준) |
| :--- | :--- | :--- | :--- |
| **NSM** | **Weekly Active Efficient Sessions** | 주간 1회 이상 효율 코칭(`/compact`/`/clear`/모델 전환) 반영 또는 한도 임계치 회피 완료 세션 비율 | 활성 세션 중 효율 점수 ≥ 75점 비중 지속 상승 |
| **실시간성** | 로그-UI 반영 지연 | CLI 턴 종료 후 메뉴 막대·대시보드 반영까지 소요 시간 | P95 < `500ms` (FSEvents 증분 파싱) |
| **정확도** | 로컬 파싱 정합성 | `requestId` 중복 제거 및 단가표 대조 기준 `ccusage` 또는 실제 청구서 대비 오차 | 로컬 토큰 집계 오차 < `1%` (플레이스홀더 플래그 제외) |
| **효율 개선** | 평균 Cache Hit Rate & 낭비 절감 | 2주 이상 연속 사용자의 세션 평균 캐시 적중률 및 컨텍스트 비대화 감소율 | Cache Hit Rate ≥ `75%`, 비대화 경고 세션 비율 감소 |
| **자원 효율** | 상주 리소스 점유 | 메뉴 막대 상시 실행 시 유휴 CPU 및 메모리 사용량 | 유휴 CPU < `1%`, RAM < `80MB` |

---

## 2. 타깃 페르소나 & 핵심 사용자 여정 (JTBD)

### 2.1 주 타깃 페르소나
- **페르소나 A — 다중 CLI 에이전트 파워유저 (1차 핵심 타깃)**
  - **프로필**: macOS 환경에서 [[entities/gemini-cli|Gemini CLI]]를 주력(1순위)으로 사용하며, 코드 리뷰·아키텍처 설계·복잡 디버깅에 [[entities/claude-code|Claude Code]]와 [[entities/codex-cli|Codex CLI]]를 병행하는 소프트웨어 엔지니어.
  - **구독 조합**: Google AI Pro/Ultra, Claude Pro 또는 Max, ChatGPT Plus/Pro 중 1~3종 복수 구독 또는 API 키 혼용.
  - **JTBD (Jobs-to-be-Done)**:
    - *"작업 도중 한도 차단을 맞기 30분 전에 메뉴 막대나 위젯으로 번레이트를 확인하고 에이전트를 전환하고 싶다."*
    - *"방금 보낸 질문 1건이 왜 그렇게 많은 토큰을 썼는지, 지금 `/compact`를 쳐야 하는지 대시보드에서 즉시 보고 싶다."*
    - *"지난달 실제 사용량을 기준으로 내 $100 구독을 유지할지, $20 + API 종량제로 내릴지 숫자로 검증받고 싶다."*

### 2.2 일일 워크플로우 접점 매핑

| 시점 | 사용 접점 | 사용자 행동 및 시스템 제공 가치 |
| :--- | :--- | :--- |
| **상시 코딩 중** | **메뉴 막대 아이콘 & WidgetKit 위젯** | 주력 에이전트(Gemini CLI 일일 요청 수) 및 활성 블록(Claude/Codex 5시간 추정 소진율) 상시 확인 |
| **한도 80% 근접 시** | **메뉴 막대 팝오버 & macOS 알림** | 예상 소진 시각("약 24분 후 한도 도달 예상") 확인 및 대체 CLI 에이전트로 작업 전환 |
| **세션 지연·비용 체감 시** | **대시보드 (Prompt Efficiency 탭)** | 최근 질문별 토큰 분해, 캐시 적중률 급락 지점, 컨텍스트 팽창 곡선 확인 후 `/compact`·`/clear` 실행 |
| **월말 / 구독 갱신 전** | **대시보드 (Plan ROI 탭, v2)** | 30일 실측 API 환산 가치($V_{\text{api}}$) 대비 구독료 비교 및 업/다운그레이드·API 병행 권고 검토 |

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
| **질문당 토큰 효율 진단·코칭** | ❌ | ❌ | ❌ | ❌ | **✅ (6대 지표 + 트리거)** |
| **요금제 ROI 비교·변경 추천** | ❌ | ❌ | ❌ | ❌ | **✅ (v2 핵심 엔진)** |

### 3.2 차별화 쐐기 (Differentiation Wedge)
1. **모니터링을 넘어선 행동 변화(Actionable Coaching)**: 경쟁 제품이 "현재 78% 소진"만 보여줄 때, 본 앱은 *"최근 10턴 동안 미압축 컨텍스트 재전송으로 질문당 비용이 3.4배 증가 — `/compact` 실행 권장"*을 제시.
2. **Gemini CLI 보존 공백 해결**: `general.sessionRetention`(`maxAge: 30d`, `maxCount: 50`) 자동 삭제 이전에 로컬 SQLite에 영구 적재해 월간·분기 ROI 데이터 보존.
3. **데이터 정직성(Honest Telemetry)**: 로컬 로그 실측치와 공유 풀 추정치를 UI 배지로 명확히 구분해 신뢰 확보.

![[visuals/competitive-positioning.excalidraw]]

---

## 4. 제품 범위 & 단계별 로드맵 (v1 / v2 / v3) 및 Non-Goals

### 4.1 MoSCoW 범위 정의

| 우선순위 | 범위 항목 | 대상 버전 |
| :--- | :--- | :---: |
| **Must Have** | Gemini CLI(1순위)·Claude Code·Codex CLI 로컬 로그 증분 파싱 및 SQLite 영구 적재 | **v1.0** |
| **Must Have** | 메뉴 막대(`MenuBarExtra`) 실시간 상태·팝오버 및 WidgetKit(Small/Medium/Large) 위젯 | **v1.0** |
| **Must Have** | 질문(턴)당 토큰 효율 분석 대시보드(6대 지표, 컨텍스트 팽창 차트, `/compact`·`/clear` 코칭) | **v1.0** |
| **Must Have** | 다중 에이전트·모델·프로젝트 통합 소비 뷰 및 외부 JSON 단가 카탈로그 | **v1.0** |
| **Should Have** | 요금제 ROI 비교기 & 업그레이드/다운그레이드/API 하이브리드 추천 엔진 (30일+ 누적 기반) | **v2.0** |
| **Should Have** | 사용자 Opt-in 기반 Tier 2 공식 API/OAuth 및 CLI `/usage` 상태 동기화 | **v2.0** |
| **Could Have** | Gemini CLI 로컬 OTel(`4317`) 수신기, 선택적 로컬 LLM(Apple Intelligence/Ollama) 프롬프트 리뷰 | **v2.1+** |
| **Won't Have (v1/v2)** | AI IDE(Cursor·Antigravity·Copilot·Windsurf), API 종량제 키 대시보드, 웹 챗봇 통합 | **v3.0+** |

### 4.2 마일스톤 로드맵
- **v1.0 (MVP — Local Monitor & Efficiency Coach)**: 100% 로컬 처리, CLI 3종 파서, SQLite 영구 적재, 메뉴 막대 + WidgetKit + 대시보드 3개 메인 탭(`Overview`, `Prompt Efficiency`, `Sessions & Projects`) + 단가 카탈로그 설정.
- **v1.1 (Hardening & UX Polish)**: 임계치 도달 네이티브 알림 규칙 고도화, 프로젝트별 예산 알림, CSV/JSON 리포트 내보내기, Sparkle 자동 업데이트 안정화.
- **v2.0 (Plan ROI & Opt-in Official Sync)**: `Plan ROI` 추천 탭 정식 탑재, Opt-in 공식 API/OAuth·CLI 상태 동기화(Tier 2), API 크레딧 잔액 추적.
- **v3.0+ (Ecosystem Expansion)**: AI IDE 확장(Cursor, Antigravity, Copilot, Windsurf), 종량제 API 키(Anthropic/OpenAI/Gemini), 웹 챗봇 사용량 통합.

### 4.3 명시적 비목표 (Non-Goals)
1. **프롬프트·소스코드 원문 외부 전송 금지**: v1에서 어떠한 텔레메트리·사용량 분석·코칭 목적의 네트워크 송신도 수행하지 않음(Sparkle 업데이트 체크 제외).
2. **비공식 브라우저 쿠키 탈취 기본 비활성**: 브라우저 쿠키·세션 토큰을 무단 추출하는 불안정한 스크래핑을 기본 수집 방식으로 채택하지 않음.
3. **LLM 기반 실시간 프롬프트 채점(v1 제외)**: v1 효율 점수는 100% 결정적 수학/규칙 모델만 사용(추론 지연 및 추가 토큰 비용 발생 차단).
4. **팀/서버 동기화 SaaS(제외)**: 개인 로컬 macOS 워크스테이션 단일 사용자 경험에 집중.

---

## 5. 상세 기능 요구사항 (FR-01 ~ FR-07)

### FR-01. 다중 CLI 로컬 로그 증분 수집·정규화 엔진 (v1.0)
- **목적**: 로컬 코딩 에이전트 3종의 로그를 비침습적으로 감지·파싱해 통합 데이터 모델로 영구 저장.
- **입력 경로 및 기본 감시 대상**:
  - **Gemini CLI (1순위)**: `~/.gemini/tmp/<project_hash>/chats/session-*.json` (및 JSONL 변형).
  - **Claude Code**: `~/.claude/projects/<project-path>/<session-id>.jsonl`.
  - **OpenAI Codex CLI**: `~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl` (`$CODEX_HOME` 환경변수 우선).
  - 사용자 설정 화면에서 커스텀 루트 경로 추가/비활성화 지원.
- **처리 로직**:
  - `FSEvents` 파일 변경 감지 + 1~2초 디바운싱 후 마지막 파싱 오프셋(`byte_offset` / `mtime` / `file_hash`) 이후 구간만 증분 파싱.
  - **중복 제거**: Claude Code `requestId`/`message.id` 기준 최종 레코드 병합.
  - **이상치 가드**: Claude Code 스트리밍 플레이스홀더(`input_tokens <= 2` 또는 `output_tokens <= 2`이면서 대량 응답) 감지 시 `confidence = low` 플래그 부여.
  - **영구 보존**: Gemini CLI 원본 파일이 `sessionRetention` 정책으로 삭제되어도 이미 적재된 `sessions`, `turns`, `api_requests` 테이블 레코드 유지.
- **출력**: 정규화된 SQLite 레코드(`agent`, `session_id`, `project_hash_or_path`, `user_turn_index`, `model`, `input_tokens`, `output_tokens`, `cache_write_tokens`, `cache_read_tokens`, `thinking_tokens`, `tool_calls_count`, `estimated_cost_usd`, `confidence_flag`, `timestamp`).
- **인수 조건 (Acceptance Criteria)**:
  - `Given` Gemini CLI 세션 파일에 새 턴이 append 될 때 `When` FSEvents 이벤트 발생 후 500ms 이내 `Then` 증분 파서만 동작해 신규 턴이 SQLite에 1건만 적재됨(전체 재스캔 금지).
  - `Given` 동일 `requestId`가 5줄 중복 기록된 Claude Code JSONL `When` 파싱 수행 `Then` DB에는 최종 1건만 반영되며 토큰 수 중복 합산 0건.
  - `Given` Gemini CLI 원본 채팅 JSON 파일이 CLI retention으로 삭제됨 `When` 앱 재시작 또는 리프레시 `Then` 대시보드의 과거 일간/월간 집계 수치가 유실 없이 동일하게 유지됨.

### FR-02. 메뉴 막대 팝오버 & WidgetKit 실시간 모니터링 (v1.0)
- **목적**: 작업 컨텍스트 전환 없이 현재 에이전트별 사용량·남은 한도·번레이트를 1초 내 파악.
- **메뉴 막대 아이콘 상태**:
  - 통합 게이지 또는 1순위 에이전트(사용자 지정, 기본 Gemini CLI) 요약 표시 + 선택적 멀티 아이콘 모드.
  - 임계치 색상/상태: 정상(`< 70%`), 주의(`70–89%`), 임박(`≥ 90%`), 데이터 오류/경로 미감지(`warning`).
- **팝오버 구성**:
  - 에이전트 카드 3종(Gemini CLI / Claude Code / Codex CLI):
    - Gemini CLI: 오늘 요청 수(`예: 412 / 1,500 req`, `실측`) + 토큰/환산 비용.
    - Claude Code: 현재 5시간 블록 경과·추정 소진율(`추정`) + 리셋 카운트다운(`2h 14m 남음`) + 주간 누적.
    - Codex CLI: 현재 5시간 블록/일일 토큰 + 추정 소진율(`추정`) + 리셋 카운트다운.
  - 최근 활성 세션 효율 배지 1줄 요약(예: `현재 세션 Cache Hit 82% · /compact 양호`).
  - "대시보드 열기(`Cmd+D`)" 및 "수동 새로고침" 버튼.
- **WidgetKit 위젯**:
  - App Group 공유 스냅샷(`widget-snapshot.json`) 기반 Small / Medium / Large 3종 지원.
  - 일일 40~70회 리로드 예산 보호를 위해 15분 쿨다운 또는 사용률 5% 이상 급변 시에만 `WidgetCenter` 리로드 요청.
- **인수 조건**:
  - `Given` 백그라운드에서 10초 내 파일 수정 이벤트 20회 연속 발생 `When` WidgetKit 갱신 판단 `Then` 실제 `reloadTimelines` 호출은 디바운스·쿨다운 규칙에 따라 최대 1회로 제한됨.
  - `Given` Tier 1 로컬 추정만 활성화된 상태 `When` Claude 5시간 한도 게이지 표시 `Then` 게이지 인접 영역에 `추정` 배지 및 툴팁("웹/모바일 사용량 미포함")이 항상 노출됨.

### FR-03. 질문(턴)당 토큰 효율 분석 대시보드 & 컨텍스트 위생 코칭 (v1.0)
- **목적**: 질문 단위 비용과 효율 저하 원인을 정량 분해하고 즉각 실행 가능한 CLI 개선 액션 제시.
- **상세 기능**:
  - [[concepts/prompt-efficiency-scoring]]의 6대 지표 산출 및 세션 종합 효율 점수(0–100점) 계산.
  - **세션 턴 워터폴(Turn Waterfall) 차트**: 턴 $1 \dots N$ 진행에 따른 입력 토큰(Uncached vs Cache Read vs Cache Write), 출력+Thinking 토큰, API 환산 비용 누적 곡선 시각화.
  - **비용 급등(Spike) / 낭비 탐지기**:
    - 컨텍스트 팽창 경고(예: `150K~400K` 초과 또는 `≥ 6x` 팽창 + 15턴 이상 미압축 → `/compact` 권고).
    - 신규 주제 전환/장기 노이즈 누적 시 `/clear` 권고.
    - 단일 도구 출력 스파이크(`> 40K` 토큰 급증) 하이라이트.
    - Thinking 토큰 과다 비율(`≥ 75%`) 턴 식별 및 경량 모델(Flash/Sonnet/Luna) 전환 시뮬레이션 절감액 표시.
    - Base Prompt / MCP 고정 세금(`≥ 25K` 토큰) 진단.
- **인수 조건**:
  - `Given` 누적 입력 토큰이 200K를 넘고 최근 15턴 연속 `/compact`가 없는 세션 `When` 대시보드 또는 팝오버 조회 `Then` `Context Bloat` 경고 카드와 함께 재전송 누적 낭비 추정 금액($) 및 `/compact` 권장 안내가 노출됨.
  - `Given` 프롬프트 원문 저장 비활성(기본값) 상태 `When` 효율 분석 실행 `Then` 원문 텍스트 접근·저장 없이 토큰 수·시간·도구 호출 메타데이터만으로 6대 지표와 점수가 100% 산출됨.

### FR-04. 다중 에이전트·프로젝트·모델 통합 소비 대시보드 (v1.0)
- **목적**: 여러 코딩 에이전트와 프로젝트에 분산된 소비량·비용·효율을 단일 화면에서 비교.
- **기능 명세**:
  - 기간 필터(오늘 / 7일 / 30일 / 90일 / 전체) 및 에이전트·모델·프로젝트별 다중 필터.
  - 에이전트별 일별 토큰/환산 비용 스택 바 차트 (`Swift Charts`).
  - 프로젝트별 소비 Top-N 및 평균 질문당 비용 비교 표.
  - 모델별 효율 비교(예: `Gemini 4 Argon` vs `Gemini 3.8 Flash`, `Sonnet 5.5` vs `Opus 5.5` 사용 비중 및 턴당 단가).
- **인수 조건**:
  - `Given` 동일 기간 내 Gemini CLI와 Claude Code를 모두 사용한 기록 존재 `When` Overview 탭에서 30일 기준 조회 `Then` 두 에이전트의 통합 API 환산 비용, 총 토큰, Cache Hit Rate 비교 차트가 1초 이내 렌더링됨.

### FR-05. 버전 관리 외부 단가·요금제 카탈로그 및 로컬 오버라이드 (v1.0)
- **목적**: AI 벤더 단가·프로모션 종료일·모델명 변경 시 앱 바이너리 재빌드 없이 정확한 비용 계산 유지.
- **기능 명세**:
  - 번들 내 기본 `pricing-catalog.json` 탑재(기준일 및 `effective_from` / `effective_until` 지원 — 예: `Gemini 4 Carbon` 2026-12-01 인상, `Gemini 3.8 Flash` 2026-12-31 프로모션 종료 반영).
  - 사용자 로컬 오버라이드 파일(`Application Support/.../pricing-overrides.json`) 및 UI 단가·플랜(현재 구독 중인 요금제 선택) 편집 기능.
  - 미등록 신규 모델명 발견 시 패밀리 기반 폴백 단가 적용 + UI `미등록 모델(추정 단가)` 알림.
- **인수 조건**:
  - `Given` `2026-12-01` 이후 타임스탬프를 가진 `Gemini 4 Carbon` 로그 `When` 비용 환산 실행 `Then` 카탈로그의 `effective_from` 규칙에 따라 인상 후 단가($4.00/$20.00)가 자동 적용됨.

### FR-06. 요금제 ROI 분석기 & 플랜 변경 추천 엔진 (v2.0)
- **목적**: 30일+ 실측 데이터를 바탕으로 월 구독료 대비 손익분기·과부족 판단 및 플랜 변경 추천.
- **기능 명세**: [[concepts/plan-roi-recommendation]] 산식($V_{\text{api}}$, 구독 ROI 배수 $R$, 한도 마찰 횟수, 비효율 제거 조정 소비액 $V_{\text{adj}}$) 및 의사결정 매트릭스 구현.
- **추천 시나리오**: 다운그레이드, API 종량제 하이브리드 전환, 업그레이드(Max/Ultra/Pro 100·200·500/AI Pro·Ultra), 플랜 유지 + 효율 개선 선행.
- **인수 조건**:
  - `Given` 데이터 수집 기간이 14일 미만인 신규 설치 상태 `When` Plan ROI 탭 진입 `Then` 성급한 플랜 변경 추천 대신 "데이터 수집 중 (X/14일)" 상태와 현재까지 누적 API 환산액만 표시됨.
  - `Given` $V_{\text{adj}} < P_{\text{plan}} < V_{\text{api}}$ 조건(낭비 제거 시 현재 플랜으로 충분) `When` ROI 분석 실행 `Then` 상위 플랜 업그레이드 대신 "플랜 유지 및 `/compact`·모델 라우팅 효율 개선"이 1순위 권고로 제시됨.

### FR-07. 선택적 Tier 2 공식 API/OAuth 및 CLI 상태 동기화 (v2.0)
- **목적**: 사용자의 명시적 Opt-in 후 공유 풀을 쓰는 구독 계정의 실제 서버 잔여 한도를 정확히 동기화.
- **기능 명세**:
  - 사용자가 설정에서 특정 프로바이더에 대해 공식 API 키 / OAuth 또는 CLI 상태 조회(`/usage`) 프로브를 명시적으로 활성화한 경우에만 동작.
  - 자격 증명은 `macOS Keychain`에만 저장.
  - 조회 성공 시 UI 배지를 `추정 (CLI 기준)` → `공식 동기화`로 승격.
- **인수 조건**:
  - `Given` 사용자가 Tier 2 연동을 비활성화(기본값)한 상태 `When` 앱 실행 중 `Then` 외부 벤더 API 엔드포인트로의 네트워크 요청 0건 보장.

---

## 6. 정보 구조 (IA) 및 UX 플로우

### 6.1 메뉴 막대 & 팝오버 구조
- **메뉴 막대 상단 바**: `[아이콘] Gem 27% | Cld 64%` (컴팩트 모드 / 단일 주력 모드 / 멀티 아이콘 모드 설정 지원).
- **팝오버 레이아웃 (폭 ~360pt)**:
  1. **헤더**: 앱 상태 요약, 최종 동기화 시각, 수동 새로고침, 대시보드 열기 버튼.
  2. **에이전트 쿼터 카드 리스트 (3단)**:
     - Gemini CLI (`실측` · 일일 `req/day` 게이지 + 오늘 환산 비용)
     - Claude Code (`추정` · 5h 블록 진행률 + 리셋 카운트다운 + 주간 누적)
     - Codex CLI (`추정` · 5h 블록/일일 토큰 + 리셋 카운트다운)
  3. **활성 세션 효율 배너**: 현재 진행 중인 세션의 Efficiency Score, Cache Hit %, 경고 시 1줄 액션 배지(`/compact 추천`).

### 6.2 WidgetKit 위젯 3종 규격
- **Small (2x2)**: 1순위 에이전트(Gemini CLI 기본) 원형 게이지 + 오늘 소비량 + 리셋/한도 상태.
- **Medium (4x2)**: CLI 3종 수평 프로그레스 바 + 오늘 총 API 환산 비용 + 효율 점수 요약.
- **Large (4x4)**: CLI 3종 상세 게이지 + 7일 소비 스파크라인 + 최근 세션 효율 경고 최대 2건.

### 6.3 대시보드 윈도우 IA (4개 메인 탭)
1. **Overview (통합 현황)**: 기간별 KPI 카드(총 환산 비용, 총 토큰, 평균 Cache Hit Rate, 일일 한도 근접 횟수), 에이전트/모델별 추이 차트, 프로젝트 Top 소비 목록.
2. **Prompt Efficiency (질문당 효율 코칭)**: 6대 지표 스코어카드, 세션 선택기, 턴별 컨텍스트 팽창·비용 워터폴 차트, 비효율 턴 리스트 및 `/compact`·`/clear`·모델 전환 시뮬레이션 카드.
3. **Sessions & Projects (세션·프로젝트 탐색기)**: 에이전트별 세션 히스토리, 프로젝트별 ROI/평균 턴 비용, 신뢰도 배지(`실측` / `Low-Confidence`).
4. **Plan ROI & Settings (요금제 분석 & 설정)**:
   - v1: 현재 구독 플랜 설정, CLI 로그 경로 탐색·커스텀 지정, 단가표 오버라이드, 알림 임계치, 프라이버시(원문 미저장 확인)·DB 백업/내보내기.
   - v2: Plan ROI 추천 카드, Tier 2 Opt-in 연동 관리.

### 6.4 최초 실행 온보딩 플로우
1. 자동 경로 스캔 (`~/.gemini`, `~/.claude`, `~/.codex`, `$CODEX_HOME`) → 감지된 에이전트 체크 표시.
2. 현재 사용 중인 요금제 선택(예: Gemini — Google AI Pro, Claude — Pro, Codex — 미사용/Plus).
3. 메뉴 막대 표시 모드 및 WidgetKit 추가 가이드 안내 → 백그라운드 초기 적재 시작(UI 블로킹 없음).

---

## 7. 비기능 요구사항 (NFR) 및 macOS 아키텍처 기준

### 7.1 지원 OS 정책 및 기술 스택
- **최소 OS 버전 정책**: 완성·배포 시점 기준 Apple 공식 보안 지원 3개 메이저 버전 정책 적용([[concepts/macos-menubar-widget-architecture]]).
  - **2026년 말 ~ 2027년 상반기 출시 기준**: **`macOS 15.0 Sequoia`** 최소 지원 (`macOS 15 Sequoia`, `macOS 26 Tahoe`, `macOS 27 Golden Gate` 동시 지원).
  - **2027년 가을 macOS 28 출시 이후**: **`macOS 26.0 Tahoe`**로 최소 버전 상향(Liquid Glass 등 최신 macOS 26/27 API 적극 채택).
- **스택**: Swift 6 / SwiftUI (`MenuBarExtra`, `Window`, `Swift Charts`), `WidgetKit`, `CoreServices(FSEvents)`, `SQLite` (GRDB.swift 등 Swift 네이티브 래퍼), `Sparkle 2`.

### 7.2 성능 및 안정성 예산

| 항목 | 목표 기준 | 검증 방법 |
| :--- | :--- | :--- |
| **유휴 CPU 점유율** | `< 1.0%` | Activity Monitor / Instruments Time Profiler |
| **상주 메모리(메뉴 막대 전용 시)** | `< 80MB` (대시보드 열림 시 `< 180MB`) | Instruments Allocations / VM Tracker |
| **증분 파싱 지연** | P95 `< 500ms` (일반 턴 추가 시) | 10MB JSONL 파일 기준 벤치마크 |
| **초기 대량 적재(1,000 세션)** | `< 5초` (백그라운드 스레드, UI 프리징 0) | 샘플 픽스처 스위트 부하 테스트 |
| **WidgetKit 리로드 예산** | 일일 `< 48회` (시스템 상한 40~70회 이내 안전 유지) | 리로드 카운터 로깅 검증 |

### 7.3 프로세스·배포 아키텍처
- **비샌드박스 메인 앱 + 샌드박스 WidgetKit**: 메인 앱은 `~/.gemini`, `~/.claude`, `~/.codex` 상시 감시를 위해 비샌드박스 + Hardened Runtime 적용. WidgetKit 익스텐션(`.appex`)은 App Group(`group.<domain>.agents-usage-mac`) 컨테이너의 경량 스냅샷만 읽기 전용 조회.
- **배포 파이프라인**: Apple Developer ID Application 서명 → `xcrun notarytool` 공증 → GitHub Releases `.dmg` + Sparkle 2 `appcast.xml` → Homebrew Cask 배포.

![[visuals/system-architecture.excalidraw]]

---

## 8. 데이터 정확도 거버넌스 & 프라이버시/보안 원칙

### 8.1 정확도 거버넌스 (Two-Tier Accuracy)
- 모든 지표는 [[concepts/two-tier-telemetry-accuracy]]에 따라 UI에서 출처 신뢰도를 명시:
  1. **`실측 (Local)`**: 로컬 로그에서 직접 파싱된 토큰 수, Gemini 일일 요청 수, API 환산 비용.
  2. **`추정 (CLI 기준)`**: 5시간 블록·주간 한도 추정치(웹·모바일 공유 풀 미반영 한계 고지).
  3. **`Low-Confidence`**: Claude Code 스트리밍 플레이스홀더(`0`/`1`/`2`) 감지 턴에 별도 표시.
  4. **`공식 동기화` (v2)**: Opt-in Tier 2 동기화 완료 값.

### 8.2 프라이버시·보안 가드레일
- **최소 데이터 수집**: 기본값으로 프롬프트 본문·코드 스니펫·도구 출력 원문을 SQLite DB에 복사 저장하지 않음(토큰·비용·도구명·바이트 길이·성공 여부·타임스탬프만 저장).
- **네트워크 격리 (v1)**: 로컬 분석 엔진은 오프라인 100% 동작.
- **자격 증명 보호 (v2)**: Opt-in API/OAuth 토큰은 `Keychain Services` (`kSecAttrAccessibleAfterFirstUnlock`) 저장, 평문 파일/UserDefaults 저장 금지.

![[visuals/telemetry-pipeline-and-accuracy.excalidraw]]

---

## 9. 기술 리스크·가정 및 완화 전략 (Risk Matrix)

| 리스크 ID | 리스크 내용 | 발생 가능성 | 영향도 | 완화 전략 |
| :--- | :--- | :---: | :---: | :--- |
| **R-01** | **Claude Code JSONL 플레이스홀더/중복**: `input/output_tokens`가 `1`/`2`로 기록되거나 `requestId` 중복 발생 | 높음 | 높음 | `requestId` 기준 최종 청크 병합, 플레이스홀더 감지 시 `Low-Confidence` 플래그 부여, 실제 로컬 픽스처 사전 검증 |
| **R-02** | **Gemini CLI `sessionRetention` 자동 삭제**: `30d`/`50개` 경과 시 원본 JSON 영구 삭제 | 높음 | 높음 | FSEvents 즉시 로컬 SQLite 영구 적재 + 백그라운드 누락 스캔(초기/주기 실행) |
| **R-03** | **WidgetKit 리로드 예산(40~70회/일) 고갈**: 잦은 CLI 응답 시 위젯 갱신 멈춤 | 중간 | 중간 | 1~2초 디바운스 + 15분 쿨다운(5% 급변 시 예외) + `TimelineProvider` 미래 엔트리 선제 생성 |
| **R-04** | **CLI 업데이트에 따른 로그 스키마 변경**: Gemini/Claude/Codex 업그레이드 후 경로·키 이름 변경 | 높음 | 중간 | 버전별 어댑터 패턴(Parser Strategy), 알 수 없는 필드 안전 무시, 파싱 실패 파일 격리·경고 배지 노출 |
| **R-05** | **벤더 단가·구독 한도 급변**: 2026-12-01 Gemini 단가 인상 등 빈번한 가격 변동 | 높음 | 중간 | 외부 JSON 카탈로그 분리(`effective_from` 날짜 조건 지원) + 사용자 로컬 오버라이드 |
| **R-06** | **공유 풀 오차로 인한 한도 예측 괴리**: 웹/모바일 채팅 사용으로 CLI 기반 추정 한도와 실제 한도 불일치 | 높음 | 중간 | UI에 `추정 (CLI 기준)` 명시 + v2 Opt-in Tier 2 동기화 및 수동 한도 리셋 보정 버튼 제공 |

---

## 10. 설계 착수 전 검증 체크리스트 (Pre-Design Validation Gate)

구체적인 Swift 아키텍처 설계(TDD/ADR) 및 구현 착수 전, 사용자 로컬 머신의 실제 로그를 대상으로 아래 5개 항목 검증 필수:

1. **실제 Gemini CLI 로그 픽스처 검증**: 현재 환경의 `~/.gemini/tmp/*/chats/*.json` 샘플 3건 이상에서 `input`/`output`/`cached`/`thoughts` 정확한 JSON 키 경로 및 모델명 포맷 확인.
2. **실제 Claude Code 로그 픽스처 검증**: `~/.claude/projects/*/*.jsonl` 샘플에서 `requestId` 중복 빈도와 `input_tokens`/`output_tokens` 플레이스홀더 실제 발생 여부 확인.
3. **실제 Codex CLI 로그 픽스처 검증**: `~/.codex/sessions/YYYY/MM/DD/rollout-*.jsonl` (또는 `$CODEX_HOME`) 샘플에서 `cache_read`/`cache_write`/모델명 필드 스키마 확정.
4. **CLI 버전별 차이 기록**: 설치된 `gemini`, `claude`, `codex` CLI 현재 버전 확인 및 파서 어댑터 v1 타깃 버전 고정.
5. **Apple Developer Team / App Group ID 예약 확인**: WidgetKit + App Group 프로비저닝에 사용할 Bundle Identifier 및 App Group 문자열 확정.
