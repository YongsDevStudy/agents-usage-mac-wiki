# 02. 로컬 CLI 텔레메트리 및 로그 스키마 조사 (Local CLI Telemetry)

- **기준일**: 2026-11-27
- **대상**: Gemini CLI (`~/.gemini`), Claude Code (`~/.claude`), OpenAI Codex CLI (`~/.codex`)
- **주요 출처 도메인**: `geminicli.com`, `googleblog.com`, `ccusage.com`, `gille.ai`, `claudelogs.com`, `tokenforest.com.au`, `prismmd.app`, `github.com`

---

## 1. Gemini CLI (`~/.gemini`)

### 1.1 로컬 세션 파일 경로 및 구조
- **경로**: `~/.gemini/tmp/<project_hash>/chats/session-<timestamp>-<uuid>.json` (JSON 또는 JSONL 형식).
- **프로젝트 매핑**: 프로젝트 루트 경로의 해시값(`<project_hash>`) 기준으로 디렉터리 분리 저장.
- **저장 내용**:
  - 사용자 프롬프트 및 모델 응답 이력.
  - 도구 호출 내역(입력, 출력, 실행 결과, 에러 상태).
  - 토큰 사용 통계: `input` 토큰, `output` 토큰, `cached` 토큰(컨텍스트 캐시 히트).
  - 모델 내부 추론(`thoughts` / reasoning tokens) 별도 기록.

### 1.2 데이터 보존 한계 (핵심 주의점)
- `~/.gemini/settings.json` 내 `general.sessionRetention` 설정에 의해 오래된 세션 파일이 자동 정리됨.
  - 대표 기본/권장 설정 예: `enabled: true`, `maxAge: "30d"`, `maxCount: 50`.
- 결과적으로 원본 JSON 파일만 매번 재스캔할 경우 30일 이전 또는 50개 초과 이력이 유실되어 월간·분기별 요금제 ROI 분석 불가능.
- **대응 요건**: 앱 자체 로컬 DB(SQLite)에 세션·턴 메트릭을 증분 영구 적재 필수.

### 1.3 OpenTelemetry(OTel) 연동 옵션
- 설정 파일(`.gemini/settings.json` 또는 환경변수 `GEMINI_TELEMETRY_*`)로 로컬 OTel 수집기 연동 가능:
  - `telemetry.enabled: true`, `target: "local"`, `otlpEndpoint: "http://localhost:4317"`, `otlpProtocol: "grpc"`, `logPrompts: true`.
  - 메트릭: `gemini_cli.token.usage` (Int Counter), 파일 작업 카운터 등.
- **주의**: `--telemetry-outfile` 플래그로 단일 파일 직접 출력 시 대용량 도구 출력으로 인해 Node.js `EFBIG` 파일 크기 초과 오류 위험 보고 → v1 기본 수집 경로로 비권장(세션 파일 직접 파싱 우선, 로컬 OTLP 리시버는 향후 선택 확장).

---

## 2. Claude Code (`~/.claude`)

### 2.1 로컬 세션 파일 경로 및 스키마
- **경로**: `~/.claude/projects/<project-path>/<session-id>.jsonl`
- **형식**: 줄 단위 독립 JSON 객체(JSONL).
- **핵심 레코드**: `type == "assistant"` 항목 내부 `message.usage` 객체:
  ```json
  {
    "type": "assistant",
    "message": {
      "id": "msg_01X...",
      "role": "assistant",
      "model": "claude-...",
      "stop_reason": "end_turn",
      "usage": {
        "input_tokens": 1,
        "output_tokens": 1,
        "cache_creation_input_tokens": 4678,
        "cache_read_input_tokens": 17997
      }
    },
    "requestId": "req_01H...",
    "timestamp": "2026-03-30T..."
  }
  ```

### 2.2 알려진 파싱 함정 (Critical Caveats)
1. **스트리밍 청크 중복(Streaming Duplicates)**:
   - 단일 응답이 스트리밍되는 동안 동일 `requestId` 및 `message.id`를 가진 여러 줄이 순차 기록됨.
   - 단순 행 합산 시 토큰 수 배 이상 과대 집계 발생 → **`requestId` / `message.id` 기준 최종 레코드 병합·중복 제거 필수**.
2. **스트리밍 플레이스홀더 왜곡(Placeholder Skew)**:
   - `cache_read_input_tokens`와 `cache_creation_input_tokens`는 초기 응답 패킷부터 정확히 기록되는 반면, 일부 버전/스트리밍 기록 시점에서 `input_tokens`와 `output_tokens`가 `0`, `1`, `2` 등 플레이스홀더 값으로 남는 현상 보고.
   - 일부 JSONL 레코드에서 extended thinking 토큰이 `output_tokens`에 반영되지 않거나 누락되는 사례 존재.
   - **대응 요건**: 파서 레벨에서 동일 `requestId` 중 `stop_reason` 존재 여부·최대 토큰 값을 우선 취하고, `output_tokens <= 2`이면서 본문 텍스트 길이가 긴 경우 "추정/플레이스홀더 감지(Low-Confidence)" 플래그 부여 및 실제 로컬 샘플 픽스처 검증 필수.
3. **5시간 롤링 빌링 블록**:
   - Claude 구독 한도는 5시간 롤링 윈도우(`ccusage blocks` 모델) 및 주간 캡 기준 동작.

---

## 3. OpenAI Codex CLI (`~/.codex`)

### 3.1 로컬 세션 파일 경로 및 스키마
- **경로**: `~/.codex/sessions/YYYY/MM/DD/rollout-<timestamp>-<id>.jsonl`
- **환경변수**: `CODEX_HOME` 설정 시 해당 경로 하위의 `sessions/` 사용.
- **형식**: 날짜별 디렉터리 계층 구조 + 세션당 1개 JSONL 파일.
- **이벤트 구성**: 사용자 프롬프트, 어시스턴트 응답, 쉘 실행, 패치 제안, 도구 실행 이벤트.
- **토큰 필드**: 어시스턴트 응답 페이로드에 `input`, `output`, `cache read`, `cache write` 토큰 통계 포함.
- **CLI 보조 커맨드**: 인터랙티브 CLI 내 `/usage`, `/status` 지원.

---

## 4. 로컬 로그 기반 계측의 구조적 경계

- **로컬 로그로 정확히 파악 가능한 영역 (Tier 1 Exact)**:
  - CLI별·모델별·프로젝트별·일별/월별 사용 토큰(Input, Output, Cache Write, Cache Read, Thoughts).
  - 질문(턴) 단위 컨텍스트 팽창, 캐시 히트율, 도구 호출 루프, API 환산 비용.
- **로컬 로그만으로 알 수 없는 영역 (Tier 1 Structural Blind Spot)**:
  - 웹(`claude.ai`, `chatgpt.com`)·모바일·타 기기에서 동일 구독 계정으로 소비한 사용량(Anthropic·OpenAI는 웹과 CLI가 공유 풀 사용).
  - 서버 측 동적 레이트 리밋 현재 잔여 %(구독 한도의 정확한 서버 잔량은 공식 API/OAuth/세션 프로브 없이는 5시간 블록 및 일일 요청 수 기반 추정치로만 산출 가능).
