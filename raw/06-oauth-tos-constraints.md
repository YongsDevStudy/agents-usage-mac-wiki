# 06. 3사 OAuth 정책, ToS 제약 및 합법적 쿼터 연동 조사 (OAuth & ToS Constraints)

- **기준일**: 2026-11-27
- **조사 목적**: 서드파티 macOS 앱이 웹 브라우저 OAuth 로그인을 통해 Google·Anthropic·OpenAI 소비자 구독 쿼터를 직접 조회할 수 있는지 기술·약관(ToS) 검증
- **주요 출처 도메인**: `geminicli.com`, `github.com`, `reddit.com`, `autonomee.ai`, `medium.com`, `pi.dev`, `onecli.sh`, `google.com`

---

## 1. 핵심 결론 요약

- 3사 모두 **서드파티 앱이 자체 웹 브라우저 OAuth 로그인으로 개인/소비자 구독(`AI Pro/Ultra`, `Claude Pro/Max`, `ChatGPT Plus/Pro`) 잔여 한도를 공식 조회하는 경로를 제공하지 않거나 약관상 금지**.
- 따라서 v1은 **100% 로컬 로그·비민감 설정 메타데이터 기반 수집**으로 고정하고, v2 Tier 2는 각 벤더별 **ToS 준수 공식/로컬 브리지 경로**로 이원화 필요.

---

## 2. 벤더별 OAuth·ToS 및 쿼터 조회 현황

### 2.1 Google Gemini / Gemini CLI
- **정책 제한**: 서드파티 소프트웨어·비공식 데스크톱 래퍼가 Gemini CLI 또는 소비자 OAuth 클라이언트 ID에 무임승차(piggybacking)해 백엔드 서비스에 접근하는 행위 엄격 금지(계정 정지 및 Client ID 차단 위험).
- **소비자 쿼터 API 부재**: 개인 Google 계정 / AI Pro·Ultra 구독의 잔여 토큰·일일 요청 수를 조회하는 공개 API 엔드포인트 미제공.
- **합법적 공식 대안**:
  - **개인 구독(CLI)**: 로컬 세션 로그의 `API Request` 총합 카운트 + Pacific Time 자정 리셋 기준 추적.
  - **Google Cloud / Vertex AI 사용자**: 표준 Google Cloud OAuth + Google Cloud Service Usage API / Cloud Monitoring API로 프로젝트 단위 쿼터 조회 가능(v2 선택 확장).

### 2.2 Anthropic Claude / Claude Code
- **Consumer ToS 제한**: 소비자 구독(Free, Pro, Max) OAuth 로그인은 Claude Code 및 Anthropic 공식 앱 일반 사용 전용. 서드파티 개발자가 `claude.ai` 자격 증명·세션 토큰을 수집·저장·중개하는 행위 금지.
- **비공식 경로 리스크**: `~/.claude/.credentials.json` 또는 Keychain 토큰을 읽어 `https://api.anthropic.com/api/oauth/usage`를 호출하는 패턴은 ToS 회색지대/위반 소지.
- **공식 합법 대안 (Claude Code Statusline Bridge)**:
  - Claude Code는 로컬 **Statusline 설정의 `stdin`으로 `rate_limits.five_hour`, `rate_limits.seven_day` 데이터를 공식 전달**.
  - 서드파티 앱이 raw OAuth 토큰에 접근하거나 외부 API를 핑하지 않고도 로컬 실행 결과만으로 정확한 서버 잔여 % 확보 가능(v2.0 핵심 연동 경로).
  - *(참고: Claude Platform OAuth + Linked Billing은 조직 API 과금 앱 연동 전용이며 소비자 구독 한도 조회용 아님).*

### 2.3 OpenAI ChatGPT / Codex CLI
- **CLI OAuth 구조**: `codex login` 자체가 `auth.openai.com/oauth/authorize` → `http://localhost:1455/auth/callback` 흐름으로 `${CODEX_HOME:-~/.codex}/auth.json` 생성.
- **비공식 쿼터 엔드포인트**: 일부 서드파티 도구가 로컬 `auth.json` 토큰으로 `https://chatgpt.com/backend-api/wham/usage`를 호출하나, 이는 **문서화되지 않은 내부 백엔드 경로**로 예고 없는 변경·차단 위험 존재.
- **합법적 대안**:
  - **v1**: 비공식 내부 API 호출·토큰 탈취 배제, 로컬 JSONL 기반 Adaptive Baseline 유지.
  - **v2 (Opt-in)**: 사용자 동의 시 CLI 자체 `/status`·`/usage` 로컬 실행 출력 파싱 또는 OpenAI Platform Organization 공식 Usage API 연동.

---

## 3. macOS GUI 앱 Auth 모드 감지 제약 (P0-A 근거)

- macOS `launchd`에서 실행되는 GUI 앱은 사용자 인터랙티브 셸(`zsh`, `direnv`, 탭별 `export`)의 환경변수(`GEMINI_API_KEY` 등)를 상속받지 못함.
- `.zshrc`나 `.env` 파일을 스캔해 API 키를 검사하는 방식은 시크릿 파일 접근 보안 오해를 유발하므로 배제.
- **정정 방향**: CLI 설정 파일의 **비민감 인증 타입 필드**(예: `selectedAuthType` 등, 시크릿 키·토큰 값 열람 금지) + 프로바이더 기본 과금 모드 + 프로젝트/세션 수동 태깅으로 구분.
