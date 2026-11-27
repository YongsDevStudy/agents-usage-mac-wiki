---
id: entity-gemini-cli
type: entity
aliases:
  - "Gemini CLI"
  - "~/.gemini"
tags:
  - entity
  - cli-agent
  - google-gemini
  - priority-1
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/local-cli-telemetry]]"
  - "[[sources/pricing-quotas-2026]]"
  - "[[sources/prompt-efficiency-metrics]]"
  - "[[sources/oauth-tos-constraints]]"
status: active
---

# Gemini CLI (`~/.gemini`)

- **역할**: 본 프로젝트 v1 MVP의 **1순위 주력 지원 대상** 로컬 코딩 에이전트 CLI.

---

## 1. 로컬 텔레메트리 구조 및 보존 제약

- **세션 로그 경로**: `~/.gemini/tmp/<project_hash>/chats/session-<timestamp>-<uuid>.json` (JSON/JSONL).
- **기록 데이터**:
  - 사용자 프롬프트·모델 응답 및 도구 호출 입출력/에러 상태.
  - 토큰 세부 항목: `input`, `output`, `cached` (Context Cache Read), `thoughts` (내부 추론 토큰).
- **치명적 데이터 유실 리스크 (`sessionRetention`)**:
  - `~/.gemini/settings.json`의 `general.sessionRetention` (`maxAge: "30d"`, `maxCount: 50` 등) 설정으로 기한 경과 또는 개수 초과 세션 자동 삭제.
  - **설계 요건**: 앱 백그라운드 워커가 FSEvents 감지 즉시 로컬 SQLite DB에 세션·턴 메트릭을 증분 영구 적재(ingest)해 장기 ROI 분석 데이터 보존.
- **OpenTelemetry 확장 옵션**:
  - `.gemini/settings.json`에서 `telemetry.enabled: true`, `target: "local"`, `otlpEndpoint: "http://localhost:4317"` 설정 시 `gemini_cli.token.usage` 메트릭 수신 가능(향후 선택적 보강 경로).
  - `--telemetry-outfile`은 대용량 도구 출력 시 Node `EFBIG` 위험이 있어 기본 수집 방식에서 배제.

---

## 2. 쿼터·캐싱·OAuth 제약 (2026-11-27 기준)

- **일일 요청 한도(Hard Cap, `API Request` 총합 기준)**:
  - Google 개인 계정 / Code Assist Individual: `1,000 req/day` (Pacific Time 자정 리셋 기본 + 로컬 자정 토글)
  - **Google AI Pro**: `1,500 req/day`
  - **Google AI Ultra**: `2,000 req/day`
  - Gemini API Free 키: `250 req/day` / Pay-As-You-Go는 계층별 RPM.
- **OAuth / ToS 제약 ([[sources/oauth-tos-constraints]])**:
  - 서드파티 앱의 소비자 OAuth 무임승차(piggybacking) 금지 및 개인 구독 잔여 한도 API 원천 부재 → 로컬 `API Request` 카운트 기반이 유일한 안전·정확 경로(GCP/Vertex AI 프로젝트 사용자만 v2 공식 Service Usage API 확장 대상).
- **캐싱 구조**:
  - Gemini 4 계열은 **Implicit Caching 전용**(최소 프리픽스 `4,096` 토큰, 최대 95% 할인). Explicit Caching 미지원.
- **효율 포인트**:
  - `thoughts` 토큰 비율이 높은 모델(Argon/Carbon/Neon)의 경우 단순 질의 시 Thinking Overhead 경보 및 Flash/Flash-Lite 다운시프트 권장([[concepts/prompt-efficiency-scoring]] 참조).

---

## 관련 문서

- [[sources/local-cli-telemetry|로컬 CLI 텔레메트리 요약]]
- [[sources/pricing-quotas-2026|2026년 11월 요금제·단가 요약]]
- [[entities/claude-code|Claude Code]]
- [[entities/codex-cli|OpenAI Codex CLI]]
- [[concepts/two-tier-telemetry-accuracy|2계층 텔레메트리 정확도 모델]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
