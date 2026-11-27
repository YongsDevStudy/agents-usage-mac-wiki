# Agents Usage Mac

macOS 네이티브 메뉴 막대(`MenuBarExtra`)·`WidgetKit` 위젯·대시보드로 로컬 코딩 에이전트 CLI의 실시간 사용량, 질문(턴)당 토큰 효율, 구독 요금제 ROI를 분석하는 컴패니언 앱.

---

## 1. 핵심 가치 및 문제 해결

- **Monitor**: 에이전트별 일일 요청 한도·5시간 롤링 블록·리셋 카운트다운 및 번레이트 상시 추적
- **Diagnose**: 질문(턴) 단위 토큰 분해(Input/Output/Cache/Thoughts), 컨텍스트 팽창 지수, `/compact`·`/clear`·모델 전환 코칭
- **Optimize (v2.0)**: 월 API 환산 소비액($V_{\text{api}}$) 및 낭비 보정 소비액($V_{\text{adj}}$) 기반 요금제 업·다운그레이드·API 병행 추천

---

## 2. 지원 대상 및 로드맵

| 버전 | 핵심 범위 | 대상 에이전트 / 데이터 원칙 |
| :--- | :--- | :--- |
| **v1.0 (MVP)** | 메뉴 막대 팝오버, WidgetKit(S/M/L), 질문당 효율 대시보드, 다중 에이전트 통합 뷰, 외부 JSON 단가표 | **Gemini CLI** (`~/.gemini`, 1순위), **Claude Code** (`~/.claude`), **Codex CLI** (`~/.codex`) · 100% 로컬 처리 |
| **v1.1** | 임계치 알림 규칙, 프로젝트별 예산 알림, Sparkle 2 자동 업데이트, Homebrew Cask | Developer ID 서명 + 공증(Notarization) |
| **v2.0** | 요금제 ROI 비교·변경 추천 엔진, Opt-in Tier 2 공식 API/OAuth 및 CLI 상태 동기화 | Keychain 자격 증명 보호, `실측`/`추정`/`공식 동기화` 배지 구분 |
| **v3.0+** | AI IDE(Cursor·Antigravity·Copilot·Windsurf), 종량제 API 키, 웹 구독 챗봇 확장 | 멀티 소스 어댑터 확장 |

---

## 3. 기획 문서 및 지식 베이스 (LLM Wiki)

본 저장소는 `/llm-wiki` 3계층 구조(`raw/`, `wiki/`, `WIKI_SCHEMA.md`)로 기획 근거와 아키텍처 지식을 관리.

- **진입 허브**: [wiki/overview.md](wiki/overview.md)
- **전체 색인**: [wiki/index.md](wiki/index.md)
- **제품 기획서 (PRD v1.0)**: [wiki/syntheses/prd-agents-usage-mac.md](wiki/syntheses/prd-agents-usage-mac.md)
- **아키텍처 다이어그램**: [system-architecture.excalidraw.md](wiki/visuals/system-architecture.excalidraw.md)

---

## 4. 개발 및 협업 워크플로우

- **브랜치 전략**: GitHub Flow (`main` 직접 푸시 금지, 이슈 기반 작업 브랜치 `feat/<issue>-<desc>`, `fix/<issue>-<desc>`, `docs/<issue>-<desc>` 분기 후 PR 머지)
- **커밋 컨벤션**: `[#<issue>] <type>(<scope>): <Imperative description>` (예: `[#4] feat(telemetry): Add Gemini CLI session parser`)
- **이슈·PR 관리**: `.github/ISSUE_TEMPLATE/` 및 `.github/PULL_REQUEST_TEMPLATE.md` 준수, `YongsDevStudy` Organization Project 보드 연동
