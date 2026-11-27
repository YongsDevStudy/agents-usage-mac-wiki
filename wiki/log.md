# Agents Usage Mac — Planning & Knowledge Wiki Log

시간순 append-only 작업 기록. 최근 이력 조회: `grep "^## \[" wiki/log.md | tail -10`

---

## [2026-11-27] init | Agents Usage Mac — Planning & Knowledge Wiki 초기화
- 도메인: macOS AI Agent Usage, Token Efficiency & Plan ROI Analytics
- 3계층 구조(`raw/`, `wiki/`, `WIKI_SCHEMA.md`) 생성 완료

## [2026-11-27] ingest | 원본 5건 인제스트 및 엔티티·개념 8건 구축
- 경쟁환경, CLI 텔레메트리(Gemini/Claude/Codex), 2026-11 단가·쿼터, 효율 지표, macOS 제약 반영

## [2026-11-27] visualize | Excalidraw 다이어그램 3종 생성·검증
- system-architecture, telemetry-pipeline-and-accuracy, competitive-positioning

## [2026-11-27] update | 실무 기획서(PRD v1.0) 및 허브·카탈로그 갱신
- wiki/syntheses/prd-agents-usage-mac.md 작성 완료

## [2026-11-27] lint | 정적 린트 및 의미 일관성 점검 통과 (20 pages, 0 errors, 0 warnings)

## [2026-11-27] update | GitHub 저장소·Org Project(#3)·Issue/PR 템플릿·초기 이슈(#1~#11) 셋업
- YongsDevStudy/agents-usage-mac 연결 및 로드맵 보드 구성

## [2026-11-27] update | 2-저장소 역할 분리 및 main Branch Protection · wiki-check CI 구성
- agents-usage-mac-wiki(기획/문서) <-> agents-usage-mac(구현) 분리 및 PR-Only 보호 규칙 적용

## [2026-11-27] update | PRD v1.1 및 핵심 개념 문서 4건 개정
- 리뷰 P0(4)·P1(7)·P2(3) 결정 반영: Adaptive Baseline, 턴 3분류, 정규화/예외, V_adj, 노치 UX

## [2026-11-27] ingest | 3사 OAuth 정책 및 ToS 제약 근거(raw/06-oauth-tos-constraints.md) 인제스트
- 소비자 OAuth 스크래핑 금지, v2.0 Claude Statusline Bridge·GCP OAuth·Codex CLI 프로브 확정

## [2026-11-27] visualize | Excalidraw 다이어그램 2종 v1.2 아키텍처 동기화
- system-architecture, telemetry-pipeline-and-accuracy 재생성 및 검증

## [2026-11-27] update | PRD v1.2 (Design-Ready Freeze) 확정
- 2차 리뷰 P0(3)·P1(4) 및 OAuth/ToS 반영 완료

## [2026-11-27] ingest | 로컬 CLI 환경·업스트림 스키마 검증(raw/07-cli-schema-validation.md) 인제스트
- claude 2.1.316, codex-cli 0.166.0, ~/.gemini/settings.json selectedType 실측 및 3사 스키마 계약 확정

## [2026-11-27] visualize | SwiftPM 모듈 및 SQLite 9-Table ERD 다이어그램 생성
- wiki/visuals/sqlite-erd-and-modules.excalidraw.md

## [2026-11-27] update | ADR-001~005 및 기술 설계서(TDD v1.0) 작성, PRD 3차 미세 정합성 반영
- 이슈 #3 사전 설계 게이트 및 TDD DDL/파서 계약 완성
