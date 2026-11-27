---
id: source-oauth-tos-constraints
type: source
raw_source: "raw/06-oauth-tos-constraints.md"
aliases:
  - "OAuth 정책 및 ToS 제약 조사"
tags:
  - source
  - oauth
  - tos
  - security
  - tier2
created: 2026-11-27
updated: 2026-11-27
sources: []
status: active
---

# Source: 3사 OAuth 정책, ToS 제약 및 합법적 쿼터 연동 조사

- **원본**: [[raw/06-oauth-tos-constraints.md]] (기준일: 2026-11-27)
- **핵심 요약**: 서드파티 macOS 앱이 자체 브라우저 OAuth로 소비자 구독 한도를 조회하는 방식은 Google·Anthropic·OpenAI 3사 모두 약관 금지 또는 비공식 내부 경로에 해당하므로 배제하고, 벤더별 합법 경로(Claude Statusline Bridge, Gemini 로컬 요청 수 + GCP OAuth, Codex CLI `/status` 프로브)를 채택해야 함.

---

## 주요 추출 팩트

1. **소비자 OAuth 무임승차·스크래핑 금지**:
   - **[[entities/gemini-cli|Gemini CLI]]**: 서드파티 앱의 소비자 OAuth 무임승차(piggybacking) 금지 및 개인 구독 쿼터 API 부재. (GCP/Vertex AI 프로젝트는 공식 Google OAuth + Service Usage API 가능).
   - **[[entities/claude-code|Claude Code]]**: Consumer ToS가 `claude.ai` 자격 증명 수집·중개 금지. 대신 **공식 Statusline `stdin` (`rate_limits.five_hour` / `seven_day`)** 경로 제공.
   - **[[entities/codex-cli|OpenAI Codex CLI]]**: `chatgpt.com/backend-api/wham/usage`는 비공식 내부 경로 → CLI `/status`·`/usage` 로컬 프로브 또는 API Org 공식 Usage API가 안전 대안.
2. **macOS GUI 앱 환경변수 제약**:
   - GUI 앱은 터미널 환경변수 미상속 + 시크릿 파일(`.env`/`.zshrc`) 스캔 금지 → 비민감 CLI 설정 메타데이터 및 수동 태깅 방식 채택.

---

## 관련 위키 문서

- [[concepts/two-tier-telemetry-accuracy|2계층 텔레메트리 정확도 모델]]
- [[entities/claude-code|Claude Code]]
- [[entities/gemini-cli|Gemini CLI]]
- [[entities/codex-cli|OpenAI Codex CLI]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD v1.2)]]
