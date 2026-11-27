---
id: concept-two-tier-telemetry-accuracy
type: concept
aliases:
  - "2계층 텔레메트리 정확도 모델"
tags:
  - concept
  - accuracy
  - privacy
  - telemetry
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[sources/local-cli-telemetry]]"
  - "[[sources/macos-architecture-constraints]]"
status: active
---

# 2계층 텔레메트리 정확도 모델 (Two-Tier Accuracy & Privacy)

- **배경**: 100% 로컬 처리 원칙(v1)과 웹·CLI 공유 풀을 가진 벤더 구독 한도의 정확한 실시간 잔여량 파악 요구 사이의 간극 해결.

---

## 1. 2계층 데이터 신뢰도 분류

| 계층 | 수집 방식 | 적용 시점 | 산출 가능 데이터 | UI 표기 배지 |
| :--- | :--- | :---: | :--- | :--- |
| **Tier 1-A (Exact Local)** | `~/.gemini`, `~/.claude`, `~/.codex` 로컬 파일 파싱 | **v1.0** | CLI 토큰(Input/Output/Cache/Thoughts), 턴당 효율, API 환산 비용, 일일 CLI 요청 수 | `실측 (Local)` |
| **Tier 1-B (Estimated)** | 로컬 사용량 + 5시간 블록/요금제 규칙 모델링 | **v1.0** | 구독 플랜 5시간·주간 잔여 한도 추정 % (웹/모바일 사용량 미반영 한계 존재) | `추정 (CLI 기준)` |
| **Tier 2 (Verified, Opt-in)** | 사용자 명시 동의 기반 공식 API/OAuth 또는 CLI `/usage` 상태 프로브 | **v2.0** | 서버 기준 실제 잔여 쿼터·API 크레딧 잔액·조직 한도 | `공식 동기화` |

---

## 2. 프라이버시 및 보안 원칙

1. **원문 미전송·최소 보관**:
   - v1은 네트워크 송신 0건(스파크 업데이트 체크 제외).
   - 로컬 SQLite DB에도 기본값으로 프롬프트/코드 원문을 복제 저장하지 않고 **토큰 수치·타임스탬프·모델명·도구 호출 메타데이터(도구명·길이·성공 여부)**만 적재(세션 제목 등 최소 식별자만 해시/로컬 보관).
2. **비공식 스크래핑 기본 배제**:
   - 브라우저 쿠키 탈취 등 비공식·불안정한 세션 스크래핑을 기본 동작으로 채택하지 않음(v2 Tier 2는 공식 API/OAuth 또는 명시적 CLI 프로브 중심 검토).
3. **시각화**: ![[visuals/telemetry-pipeline-and-accuracy.excalidraw]]

---

## 관련 문서

- [[sources/local-cli-telemetry|로컬 CLI 텔레메트리 요약]]
- [[entities/gemini-cli|Gemini CLI]]
- [[entities/claude-code|Claude Code]]
- [[entities/codex-cli|OpenAI Codex CLI]]
- [[concepts/macos-menubar-widget-architecture|macOS 메뉴 막대·위젯 아키텍처]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
