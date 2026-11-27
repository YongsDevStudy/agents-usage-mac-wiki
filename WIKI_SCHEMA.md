# Agents Usage Mac — Planning & Knowledge Wiki Schema

**도메인**: macOS AI Agent Usage, Token Efficiency & Plan ROI Analytics  
**생성일**: 2026-11-27  

---

## 1. 계층 및 수정 규칙

1. **`raw/` (불변 원본 계층)**:
   - 외부 문서·노트·이미지(`raw/assets/`) 저장.
   - LLM은 읽기 전용으로만 접근하며 원본 파일 내용 수정·삭제 금지.
2. **`wiki/` (LLM 유지보수 지식 계층)**:
   - LLM이 요약(`sources/`), 엔티티(`entities/`), 개념(`concepts/`), 종합(`syntheses/`), 시각화(`visuals/*.excalidraw.md`), 색인(`index.md`), 로그(`log.md`)를 직접 작성·갱신.
   - 기술 문서는 서술형 종결어미와 1인칭을 배제한 간결한 명사형·개조식 문장 유지.
3. **`WIKI_SCHEMA.md` (운영 규약)**:
   - 도메인 특성에 맞춰 사용자·LLM이 함께 개정.

---

## 2. 파일·링크·시각화 규칙

- 파일명은 소문자 kebab-case 사용 (`예: three-vendor-ssot.md`).
- 모든 콘텐츠 페이지 상단에 YAML Frontmatter(`id`, `type`, `aliases`, `tags`, `created`, `updated`, `sources`, `status`) 필수 기재.
- 위키 내부 연결은 `[[wikilink]]` 사용 (동명 파일 충돌 시 `[[concepts/name]]` 형태 경로 표기).
- 구조·흐름·비교 시각화 시 `wiki/visuals/<slug>.excalidraw.md` 생성 후 본문에 `![[<slug>.excalidraw]]` 임베드.
- `.excalidraw.md`는 `excalidraw_gen.py`로 생성·검증하며 Prettier 등 범용 포매터 적용 금지.

---

## 3. 표준 작업 절차

- **Ingest**: `raw/` 원본 정독 → `wiki/sources/<slug>.md` 생성 → `entities/`·`concepts/` 갱신 및 모순 기록 → 필요 시 Excalidraw 시각화 추가 → `wiki/index.md` 갱신 → `wiki/log.md` 기록.
- **Query**: `wiki/index.md` 탐색 → 관련 페이지 정독 → 출처 포함 답변 → 재사용 가치 높은 종합/비교 결과는 `wiki/syntheses/`로 역축적.
- **Lint**: `wiki_tool.py lint` 정적 검사(깨진 링크·고아 페이지·프론트매터·Excalidraw 스키마) + 의미적 모순·구식 주장·개념 페이지 누락 점검 → `wiki/log.md` 기록.

---

## 4. `agents-usage-mac` 도메인 특화 규약

- **대상 범위 (v1 MVP)**: 로컬 코딩 에이전트 CLI 3종 (`Gemini CLI` 1순위, `Claude Code`, `OpenAI Codex CLI`) 및 향후 확장 대상(AI IDE·API 키·웹 구독 챗봇).
- **데이터 정확도 2계층 표기 원칙**:
  - `Tier 1 (Exact Local)`: 로컬 세션 로그(`~/.gemini`, `~/.claude`, `~/.codex`) 파싱을 통한 실측 토큰·턴·API 환산 비용.
  - `Tier 1 (Estimated)`: 로컬 소비량과 공개 요금제 규칙으로 추정한 5시간·주간·일일 잔여 한도(웹·모바일 공유 풀 변동 포함).
  - `Tier 2 (Verified, v2 Opt-in)`: 사용자 명시 동의 후 공식 API/OAuth 또는 CLI 상태 프로브로 조회한 서버 잔여 쿼터.
  - 모든 위키·기획서·UI 명세에서 실측치와 추정치를 엄격히 구분 표기.
- **단가·요금제 카탈로그 외부화 원칙**:
  - 벤더 모델 단가와 구독 플랜 한도는 수시 변경 대상이므로 기준일(`YYYY-MM-DD`) 명시 및 외부 버전 관리 JSON 카탈로그 + 로컬 오버라이드 구조 전제.
- **문서 본문 링크 규약**:
  - YAML Frontmatter(`sources:`) 외 본문 내 `[[...]]` 상호 참조 필수 기재(고아 페이지 방지 및 지식 그래프 연결 유지).
