# 03. 2026년 11월 기준 요금제·쿼터·API 단가 조사 (Pricing & Quotas)

- **기준일**: 2026-11-27
- **주의사항**: AI 벤더 모델 라인업·프로모션 단가·구독 한도는 수시 변경되므로 앱 내 하드코딩 금지, 외부 버전 관리 카탈로그 적용 필수
- **주요 출처 도메인**: `anthropic.com`, `openai.com`, `ai.google.dev`, `geminicli.com`, `benchlm.ai`, `infro.io`, `datastudios.org`, `olumia.dev`

---

## 1. Google Gemini (Gemini CLI & Gemini API)

### 1.1 Gemini CLI 일일 요청 쿼터 (Requests per User per Day)
| 인증 / 구독 플랜 | 일일 요청 한도 | 비고 |
| :--- | :--- | :--- |
| **Google 계정 / Code Assist (Individual)** | 1,000 req/day | 개인용 Code Assist 단독 플랜은 Google AI 구독·Antigravity 중심으로 재편 |
| **Google AI Pro** | 1,500 req/day | 개인 유료 구독 티어 |
| **Google AI Ultra** | 2,000 req/day | 개인 최상위 구독 티어 |
| **Gemini API Key (Free Tier)** | 250 req/day | 무료 키 제한 |
| **Pay-As-You-Go / Vertex AI** | 계층별 RPM 적용 | 종량제 과금, 일일 고정 캡 대신 분당 요청(RPM) 제한 |

- *참고*: 레거시 Gemini Code Assist Standard(~$19–$23/월) 및 Enterprise(~$45–$54/월)는 신규 판매 종료 및 2027년까지 단계적 일몰 진행.

### 1.2 Gemini API 대표 단가 (Per 1M Tokens, 2026-11-27 기준)
| 모델 티어 | Standard Input | Cached Input Read | Standard Output | 단가 변동 예고 |
| :--- | :--- | :--- | :--- | :--- |
| **Gemini Flash-Lite** | $0.10 | — | $0.40 | — |
| **Gemini 3.8 Flash** | $0.75 (intro) | — | $3.75 (intro) | 2026-12-31 이후 $1.50 / $7.50 |
| **Gemini 4 Hydrogen / Neon** | $1.50 – $3.00 | ~$0.15 (Neon) | $7.50 – $15.00 | — |
| **Gemini 4 Carbon / Argon** | $2.00 | ~$0.10 – $0.20 | $10.00 | 2026-12-01부터 $4.00 / $20.00 |

- **Batch API**: 비동기 24시간 처리 시 입출력 50% 할인.
- **Context Caching**:
  - **Implicit Caching (자동)**: Gemini 2.5+ 및 Gemini 4 기본 활성화, 최소 프리픽스 **4,096 토큰**(Gemini 4 / 3.8 Flash 기준) 이상 일치 시 최대 **95% 할인**.
  - **Explicit Caching**: Gemini 2.5/3 일부만 지원(`cachedContents` + 시간당 스토리지 과금), **Gemini 4 모델은 Implicit Caching 전용**.

---

## 2. Anthropic Claude (Pro, Max, Ultra & Claude API)

### 2.1 구독 플랜 및 레이트 리밋
| 플랜 | 월 구독료 | 사용량 한도 특성 | API 크레딧 포함 | 비고 |
| :--- | :--- | :--- | :---: | :--- |
| **Free** | $0 | 5시간 단위 제한적 윈도우 | ❌ | 웹 중심 |
| **Pro** | $20/월 (연 $17/월) | 기준 5시간·주간 롤링 한도, Claude Code 포함 | ❌ | 웹·데스크톱·CLI 풀 공유 |
| **Max (5x)** | $100/월 | Pro 대비 약 5배 5시간·주간 한도 | $100/월 | 피크타임 우선 접근, Fable 주간 별도 캡 |
| **Ultra** | $200/월 | 주간 10배(일부 20x 상당), 5시간 제한 완화(인증 계정) | $200/월 | 한도 소진 시 종량제 API 크레딧 폴백 가능 |

### 2.2 Claude API 대표 단가 및 프롬프트 캐싱 (Per 1M Tokens)
- **캐시 배수 규칙**:
  - Cache Write (5분 TTL): 기본 입력 단가의 `1.25x`
  - Cache Write (1시간 TTL): 기본 입력 단가의 `2.00x`
  - Cache Read (Hit): 기본 입력 단가의 `0.10x` (플래그십 일부 모델은 `0.025x`~`0.05x`까지 인하)
  - *참고*: 캐시 히트 토큰은 ITPM(Input Tokens Per Minute) 레이트 리밋 산정에서 제외.

| 모델 티어 | Standard Input | Standard Output | Cache Write (5m) | Cache Read |
| :--- | :--- | :--- | :--- | :--- |
| **Claude Haiku 5.5** (≤100K) | $0.10 | $0.50 | $0.125 | $0.01 |
| **Claude Sonnet 5.5** | $2.00 | $10.00 | $2.50 | $0.10 |
| **Claude Opus 5.5** | $4.00 (일부 티어 상이) | $20.00 | $5.00 (`1.25x`) | $0.20 |
| **Claude Fable 5.1** | $10.00 | $50.00 | $12.50 | $0.25 |

---

## 3. OpenAI ChatGPT & Codex (Plus, Pro & OpenAI API)

### 3.1 구독 플랜 및 공유 풀 구조
| 플랜 | 월 구독료 | 한도 특성 | API 크레딧 환급 | 비고 |
| :--- | :--- | :--- | :---: | :--- |
| **Free / Go** | $0 / $8(US) | 경량 웹/모바일 | ❌ | — |
| **ChatGPT Plus** | $20/월 | 기준 5시간·주간 롤링 한도, Codex 포함 | ❌ | ChatGPT + Codex 공유 풀 |
| **Pro 100 / 200 / 500** | $100 / $200 / $500 | Plus 대비 약 10x / 20x / 50x 풀 확장, 5시간 제한 제거 | 구독료 전액 상당 API 크레딧 | 400K 추론 윈도우, $500 티어 Ultrafast 지원 |

- **Instant 모드 예외**: 빠른 채팅(`Instant`) 모드는 ChatGPT/Codex 공유 토큰 할당량에서 차감되지 않음.

### 3.2 OpenAI API 대표 단가 (Per 1M Tokens)
| 모델 티어 | Standard Input | Cached Input | Standard Output | 비고 |
| :--- | :--- | :--- | :--- | :--- |
| **GPT-6 Luna** | $0.10 | $0.01 – $0.025 | $0.50 | 경량·대용량 |
| **GPT-6.1 Terra** | $1.00 | $0.05 | $5.00 | 중간 티어 |
| **GPT-6.1 / 6.2 Sol** | $2.00 | $0.10 | $10.00 | 주력 코딩 모델 |
| **GPT-6 / 6.2 Astra** | $8.00 – $10.00 | $0.40 – $1.00 | $40.00 – $50.00 | 플래그십 추론 |

- **장문 컨텍스트 할증**: 입력이 임계치(대표적으로 272K 토큰)를 초과하면 입력 단가 2배 배율 적용. Cache Write는 TTL(30분~12시간)에 따라 `1.25x`~`2.5x`.
