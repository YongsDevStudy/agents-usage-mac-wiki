---
id: entity-codex-cli
type: entity
aliases:
  - "OpenAI Codex CLI"
  - "~/.codex"
tags:
  - entity
  - cli-agent
  - openai-codex
  - required
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/local-cli-telemetry]]"
  - "[[sources/pricing-quotas-2026]]"
  - "[[sources/oauth-tos-constraints]]"
status: active
---

# OpenAI Codex CLI (`~/.codex`)

- **역할**: 본 프로젝트 v1 MVP **필수 지원** 로컬 코딩 에이전트 CLI.

---

## 1. 로컬 로그 경로 및 구조

- **기본 경로**: `~/.codex/sessions/YYYY/MM/DD/rollout-<timestamp>-<id>.jsonl`.
- **경로 오버라이드**: 환경변수 `CODEX_HOME` 설정 시 `$CODEX_HOME/sessions/` 우선 탐색 필수.
- **이벤트 모델**: 날짜별 디렉터리 내 세션당 1개 JSONL 파일. 사용자 프롬프트, 어시스턴트 응답, 쉘 실행, 패치 제안, 도구 실행 이벤트 기록 및 응답 페이로드 내 `input`, `output`, `cache read`, `cache write` 토큰 통계 포함.
- **보조 확인 수단 및 OAuth 제약 ([[sources/oauth-tos-constraints]])**: CLI 자체 `/usage`, `/status` 커맨드 지원. 비공식 내부 경로(`chatgpt.com/backend-api/wham/usage`) 호출은 배제하고 v1은 Adaptive Baseline 유지, v2 Opt-in에서 CLI `/status` 로컬 프로브 또는 API Org 공식 Usage API 연동.

---

## 2. 요금제 및 비용 산정 특성

- **공유 풀 구조**: ChatGPT Plus($20, 5시간·주간 롤링), Pro 100/200/500($100/$200/$500, 약 10x/20x/50x 풀 확장 및 5시간 제한 완화, 전액 API 크레딧 환급) 모두 ChatGPT 웹과 Codex CLI가 동일 토큰 할당량 공유. Instant 모드는 미차감.
- **장문 컨텍스트 할증**: 입력이 임계치(예: 272K 토큰) 초과 시 API 환산 비용 계산에서 입력 단가 2배 배율 반영 필수([[concepts/plan-roi-recommendation]] 참조).

---

## 관련 문서

- [[sources/local-cli-telemetry|로컬 CLI 텔레메트리 요약]]
- [[sources/pricing-quotas-2026|2026년 11월 요금제·단가 요약]]
- [[entities/gemini-cli|Gemini CLI]]
- [[entities/claude-code|Claude Code]]
- [[concepts/two-tier-telemetry-accuracy|2계층 텔레메트리 정확도 모델]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
