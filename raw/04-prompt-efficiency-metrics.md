# 04. 프롬프트 토큰 효율 및 컨텍스트 엔지니어링 지표 (Prompt Efficiency Metrics)

- **기준일**: 2026-11-27
- **목적**: LLM 코딩 에이전트의 턴/세션 로그만으로 100% 로컬에서 산출 가능한 정량 효율 지표 및 비용 절감 레버 정의
- **주요 출처 도메인**: `strake.dev`, `claude.com`, `brainy.ink`, `betterclaw.io`, `systemprompt.io`, `redis.io`, `uber.com`

---

## 1. 코딩 에이전트 토큰 경제학의 핵심 원리

- **무상태 재전송 비용(Stateless Context Re-read)**: LLM은 턴마다 누적된 시스템 프롬프트 + MCP/도구 스키마 + 대화 이력 + 도구 출력을 매 요청 전체 재입력.
- **이차적 비용 증가(Quadratic Session Cost)**: 총 턴 수 $N$, 턴당 평균 추가 컨텍스트 $\Delta C$일 때 세션 누적 입력 토큰은 $O(N^2 \cdot \Delta C)$ 규모로 급증.
- **방어 기제**: 프롬프트 캐싱(Cache Read는 일반 입력 대비 90~97.5% 저렴) 유지와 적시 컨텍스트 압축(`/compact`)·초기화(`/clear`)가 비용 효율의 핵심.

---

## 2. 로컬 로그 기반 6대 핵심 효율 지표

### 2.1 Cache Hit Rate (캐시 적중률)
- **산식**: `cache_read / (cache_read + cache_creation + uncached_input)`
- **의미**: 프리픽스 캐시가 얼마나 안정적으로 재사용되었는지 측정.
- **비효율 원인**:
  - 세션 도중 시스템 프롬프트·전역 규칙(`CLAUDE.md`, `GEMINI.md`)·MCP 도구 목록 변경 시 프리픽스 캐시 전체 무효화.
  - 5분 TTL 만료 후 재질의 시 `cache_creation` 재과금(`1.25x`).
  - Gemini 4 모델 기준 최소 4,096 토큰 프리픽스 미달 시 Implicit Caching 미발동.

### 2.2 Context Bloat Index (컨텍스트 팽창 지수)
- **산식**: 세션 내 최신 턴 총 입력 토큰 $\div$ 첫 턴 기본 입력 토큰, 또는 최근 5턴 평균 입력 토큰 증가 기울기($\Delta \text{input}/\text{turn}$).
- **비효율 패턴**:
  - 기본 auto-compact 임계치(예: 1M 컨텍스트 모델 기준 ~967K)까지 압축 없이 방치 시 수십 턴 동안 매 요청 수백만 토큰 재전송.
  - 권장: 탐색 작업 완료 후 코드 수정 전 수동 `/compact`, 무관한 신규 작업 시작 전 `/clear` 실행(또는 `autoCompactWindow: 400000` 등 하향 튜닝).

### 2.3 Effective Cost per User Prompt (질문당 실질 비용 및 루프 배수)
- **산식**: 단일 사용자 질의(User Turn)가 완료될 때까지 발생한 모든 하위 API 요청(Tool Call + 재시도 포함)의 합산 API 환산 비용 및 요청 배수(`API Requests / User Prompt`).
- **비효율 패턴**:
  - 모호한 프롬프트나 넓은 `glob`/`grep` 탐색 지시로 에이전트가 10~30회 불필요한 파일 열람 루프 수행.

### 2.4 Tool Output Bloat (도구 출력 비대화)
- **산식**: 단일 턴에서 직전 턴 대비 입력 토큰 급증분($\Delta \text{Context}_t$) 중 도구 응답(빌드·테스트·린트 전체 로그)이 차지하는 비중.
- **완화 행동**: `--quiet` 플래그 사용, 실패 로그만 필터링 전달, 대규모 조사는 서브에이전트로 격리.

### 2.5 Thinking / Output Overhead Ratio (추론 토큰 비중)
- **산식**: `thinking_tokens / (output_tokens + thinking_tokens)` (Gemini CLI `thoughts` 및 추론 모델 대상).
- **비효율 패턴**: 단순 오타 수정이나 보일러플레이트 질의에 최상위 추론 모델·무제한 Thinking Budget 사용 → Flash/Sonnet/Luna 급 경량 모델 라우팅 권고.

### 2.6 Base Prompt / MCP Tax (고정 컨텍스트 세금)
- **산식**: 세션 첫 번째 API 요청의 초기 입력 토큰 크기.
- **의미**: 미사용 MCP 서버·장황한 전역 지침이 매 턴 고정 비용으로 누적되는 규모 파악.
