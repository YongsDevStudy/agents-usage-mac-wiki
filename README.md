# Agents Usage Mac — Planning & Knowledge Wiki

macOS 네이티브 AI 코딩 에이전트 사용량·질문당 토큰 효율·요금제 ROI 분석 애플리케이션의 **기획·리서치·아키텍처(ADR/TDD)·검증 문서 통합 관리 저장소**.

---

## 1. 2-저장소 역할 분리 구조

| 저장소 | 역할 | 링크 |
| :--- | :--- | :--- |
| **`agents-usage-mac-wiki` (현재 저장소)** | 시장/단가 리서치(`raw/`), 지식 베이스(`wiki/`), PRD, ADR/TDD, 로컬 로그 픽스처 검증 | [YongsDevStudy/agents-usage-mac-wiki](https://github.com/YongsDevStudy/agents-usage-mac-wiki) |
| **`agents-usage-mac`** | Swift/SwiftUI macOS 메뉴 막대·WidgetKit·대시보드 실제 애플리케이션 구현 *(기획 고도화 후 구현 착수)* | [YongsDevStudy/agents-usage-mac](https://github.com/YongsDevStudy/agents-usage-mac) |
| **Organization Project** | 기획·설계·구현 전체 로드맵 통합 보드 | [Agents Usage Mac Roadmap (#3)](https://github.com/orgs/YongsDevStudy/projects/3) |

---

## 2. 핵심 문서 진입점 (LLM Wiki)

본 저장소는 `/llm-wiki` 3계층 구조(`raw/`, `wiki/`, `WIKI_SCHEMA.md`)로 운영.

- **진입 허브**: [wiki/overview.md](wiki/overview.md)
- **전체 색인**: [wiki/index.md](wiki/index.md)
- **제품 기획서 (PRD v1.0)**: [wiki/syntheses/prd-agents-usage-mac.md](wiki/syntheses/prd-agents-usage-mac.md)
- **아키텍처 다이어그램**: [system-architecture.excalidraw.md](wiki/visuals/system-architecture.excalidraw.md)
- **위키 운영 규약**: [WIKI_SCHEMA.md](WIKI_SCHEMA.md)

---

## 3. GitHub Branch Strategy & 협업 규약

- **`main` 브랜치 보호 (GitHub Ruleset `active`)**:
  - `main` 직접 푸시·강제 푸시(`non_fast_forward`)·브랜치 삭제 차단
  - 모든 변경은 이슈 기반 작업 브랜치(`docs/<issue>-<desc>`, `chore/<issue>-<desc>`, `refactor/<issue>-<desc>`)에서 PR로만 머지
  - 미해결 리뷰 스레드 해결(`required_review_thread_resolution`) 및 CI 상태 체크(`wiki-check`) 통과 필수
- **커밋 컨벤션**: `[#<issue>] <type>(<scope>): <Imperative description>` (예: `[#12] chore: Align wiki repo templates and add wiki CI`)
- **이슈 범위**: 본 저장소는 기획·설계(ADR/TDD)·로그 픽스처 검증 이슈만 관리하며, 실제 구현 이슈는 기획 확정 후 `YongsDevStudy/agents-usage-mac`에 등록
