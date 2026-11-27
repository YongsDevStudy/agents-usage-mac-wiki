# 01. 시장·경쟁 도구 조사 (Market & Competitors)

- **기준일**: 2026-11-27
- **조사 범위**: macOS 메뉴 막대 AI 사용량 트래커 및 로컬 CLI 토큰 분석 오픈소스 생태계
- **주요 출처 도메인**: `github.com`, `ccusage.com`, `openusage.ai`, `claudeusagebar.com`, `ai-deck.app`, `macmenubar.app`

---

## 1. 시장 배경 및 공통 페인포인트

- 터미널 기반 코딩 에이전트(Claude Code, OpenAI Codex CLI, Gemini CLI 등) 보급 확대로 작업 도중 예기치 못한 레이트 리밋·롤링 윈도우 한도 도달("Token Anxiety") 빈발.
- 주요 AI 벤더(Anthropic, OpenAI, Google)가 소비자 구독 플랜의 실시간 잔여 한도를 조회하는 단일 표준 공개 API를 제공하지 않는 한계 존재.
- 기존 유틸리티들은 비침습적 로컬 방식(CLI 인증 토큰/RPC, 로컬 Keychain/브라우저 세션 캐시, 로컬 JSONL/JSON 로그 파싱)에 의존해 실시간 한도·리셋 카운트다운을 추적.

---

## 2. 주요 경쟁·유사 도구 분석

### 2.1 CodexBar (`codexbar`)
- **개발·배포**: Peter Steinberger 제작 오픈소스 macOS 메뉴 막대 앱 (`brew install --cask codexbar`, GitHub 배포).
- **지원 범위**: Claude, Codex, Gemini, Cursor, Copilot, DeepSeek, OpenRouter 등 다수 프로바이더.
- **핵심 기능**:
  - 실시간 세션·주간 한도 리셋 카운트다운.
  - 프로바이더별 개별 아이콘 또는 통합 "Merge Icons" 모드 지원.
  - 로컬 비용 추적 및 크레딧 잔액 스캔.
  - 비밀번호 저장 없이 로컬 CLI 크리덴셜·API 키·브라우저 세션을 재사용하는 보안 모델.

### 2.2 `ccusage` (CLI 분석기)
- **개발·배포**: 오픈소스 Node/TS CLI (`npx ccusage daily`, `npx ccusage blocks`, GitHub/ccusage.com).
- **지원 범위**: Claude Code, OpenAI Codex CLI, Gemini CLI, OpenCode, Cline, GitHub Copilot CLI 등.
- **핵심 메커니즘**:
  - `~/.claude/projects/` 등 로컬 JSONL/JSON 파일 파싱.
  - Input / Output / Cache Creation / Cache Read 토큰 분리 집계 및 `ccusage.json` 기반 커스텀 모델 단가 오버라이드.
  - Claude 5시간 롤링 빌링 블록(`ccusage blocks`) 매핑, 번레이트(burn rate) 및 예상 소진량 산출.
  - `--json` 출력으로 스크립팅·외부 대시보드 파이프라인 연동 지원.
- **한계**: CLI 전용 도구로 상시 가시성(메뉴 막대·위젯), 인터랙티브 대시보드, 프롬프트 효율 코칭, 요금제 변경 추천 기능 부재.

### 2.3 OpenUsage (`openusage.ai`)
- **특징**: 다중 계정(개인·업무) 워크플로우에 특화된 오픈소스 macOS 메뉴 막대 트래커.
- **지원·기능**: Claude, ChatGPT/Codex, Cursor, Copilot 지원, 소진 임박 한도 우선 정렬, 스크립팅용 `openusage` CLI 헬퍼(JSON 파싱) 제공.

### 2.4 ClaudeBar / ClaudeUsageBar / ccusage-menubar
- **특징**: Claude 및 Claude Code 단일 생태계 특화 메뉴 막대 도구.
- **기능**: `~/.claude` 로컬 파일 또는 CLI 출력 기반 실시간 비용·토큰 통계, 5시간·주간 한도 배터리형 프로그레스 바, 임계치 도달 알림.

### 2.5 기타 인디 앱 (AI Usage Barometer, PromptJuice, TokenBar 등)
- **특징**: Swift/SwiftUI 기반 네이티브 macOS 메뉴 막대 게이지, 데스크톱 위젯, 임계치 알림 제공.

---

## 3. 비교 요약 및 시장 공백 (Differentiation Gap)

| 도구명 | 형태 | 다중 CLI (Gemini·Claude·Codex) | 메뉴 막대 / 위젯 | 5h·일일 쿼터 추적 | 질문(턴)당 토큰 효율 진단 | 요금제 ROI·변경 추천 |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| **CodexBar** | macOS 앱 | ✅ | 메뉴 막대 | ✅ | ❌ | ❌ |
| **ccusage** | CLI | ✅ | ❌ | ✅ (blocks) | ❌ (일/세션 집계 위주) | ❌ |
| **OpenUsage** | macOS 앱 | 일부 (Gemini 미주력) | 메뉴 막대 + CLI | ✅ (멀티계정 강점) | ❌ | ❌ |
| **ClaudeBar 류** | macOS 앱 | ❌ (Claude 전용) | 메뉴 막대 | ✅ | ❌ | ❌ |

- **시장 공백**:
  1. **사후 모니터링 편중**: 경쟁 앱 대부분이 "남은 한도와 누적 비용" 표시에 집중하며, **질문(턴) 단위로 왜 토큰이 낭비되는지(캐시 히트율 저하, 컨텍스트 비대화, 도구 출력 폭주, 과도한 사고 토큰)** 진단·코칭하는 기능 부재.
  2. **요금제 의사결정 부재**: 다중 에이전트 월간 실제 API 환산 소비량과 한도 도달 빈도를 근거로 **구독 업그레이드·다운그레이드·API 종량제 전환**을 정량 추천하는 기능 부재.
  3. **Gemini CLI 1순위 네이티브 지원 약세**: 기존 메뉴 막대 생태계가 Claude Code/Codex 중심이며, Gemini CLI의 `~/.gemini/tmp/<hash>/chats/` 보존 주기(`30d`/`50개`)·Thoughts 토큰·Implicit Caching 특성을 반영한 네이티브 UX 미흡.
