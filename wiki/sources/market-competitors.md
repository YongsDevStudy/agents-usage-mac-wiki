---
id: source-market-competitors
type: source
raw_source: "raw/01-market-competitors.md"
aliases:
  - "시장·경쟁 도구 조사"
tags:
  - source
  - market
  - competitors
created: 2026-11-27
updated: 2026-11-27
sources: []
status: active
---

# Source: 시장·경쟁 도구 조사

- **원본**: [[raw/01-market-competitors.md]] (기준일: 2026-11-27)
- **핵심 요약**: macOS 메뉴 막대 AI 사용량 트래커 시장에는 `CodexBar`, `ccusage`, `OpenUsage`, `ClaudeBar` 등이 존재하나, 대부분 사후 쿼터 모니터링에 편중되어 있으며 질문(턴)당 효율 코칭 및 요금제 ROI 추천 영역이 공백 상태.

---

## 주요 추출 팩트

1. **시장 문제("Token Anxiety")**: 코딩 에이전트 작업 중 예기치 않은 5시간·일일 한도 도달이 빈번하나 벤더 공식 실시간 구독 잔여량 API가 부재.
2. **기존 생태계 스펙트럼**:
   - **CodexBar**: 다중 프로바이더 지원, 메뉴 막대 통합/개별 아이콘, 로컬 크리덴셜·쿠키·로그 기반 한도 및 크레딧 추적.
   - **`ccusage`**: CLI 기반 로컬 JSONL 파서(`daily`, `blocks`, `--json`), 캐시 쓰기·읽기 분리 비용 계산 및 5시간 빌링 블록 모델 정립.
   - **OpenUsage**: 다중 계정(개인/업무) 및 소진 임박 한도 우선 정렬 특화.
   - **ClaudeBar 류**: Claude 단일 생태계 5시간·주간 한도 게이지.
3. **차별화 기회**:
   - **질문당 토큰 효율 분석**(캐시 히트율, 컨텍스트 팽창, 도구 출력 비대화, 사고 토큰 비중) 부재.
   - **요금제 ROI 비교 및 변경 추천** 부재.
   - **[[entities/gemini-cli|Gemini CLI]] 1순위 지원** 및 보존 주기(`30d`/`50개`) 대응 영구 적재 미흡.

---

## 관련 위키 문서

- [[entities/competitor-landscape|경쟁 도구 비교 매트릭스]]
- [[concepts/prompt-efficiency-scoring|프롬프트 효율 점수 모델]]
- [[concepts/plan-roi-recommendation|요금제 ROI 추천 엔진]]
- [[syntheses/prd-agents-usage-mac|제품 기획서 (PRD)]]
