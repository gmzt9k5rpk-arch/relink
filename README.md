# 청년 사회참여·일경험 매칭 플랫폼 — 개발 킥오프 브리프

> 이 문서는 「모두의 창업: 사회혁신 소셜벤처 리그」 지원용 솔루션제안서(과제② 일자리 및 사회참여 확대)의
> 사업 내용을 바탕으로, 실제 서비스를 AI(코덱스/코파일럿 등)로 개발·완성할 수 있도록
> 기술 스펙 형태로 재구성한 문서입니다.
> Visual Studio에서 Codex/Copilot에게 이 문서를 붙여넣고 개발을 시작하시면 됩니다.

---

## 0. 서비스 한 줄 정의

고립·은둔 청년이 온라인 커뮤니티에서 정서적 안정과 소속감을 먼저 얻고, 이후 단계적으로 초단기(2~4시간) 사회참여·근로 활동에 참여하도록 돕는 플랫폼.

**핵심 구조**: 회복(커뮤니티) → 사회참여(소규모 프로그램) → 초단기 근로(소상공인 매칭) → 자립

**중요 원칙**: AI는 참여자를 "평가·진단"하지 않는다. 참여 이력/관심분야/만족도 기반으로 활동을 "추천"하는 보조 도구로만 쓴다.

---

## 1. 핵심 사용자 3그룹

| 사용자 | 목표 | 니즈 |
|---|---|---|
| 청년 참여자 (고립·은둔 청년) | 부담 없이 사회와 다시 연결되기 | 낮은 진입장벽, 단계적 참여, 익명성/안전감 |
| 소상공인 (카페·식당 등) | 피크타임(점심/저녁/주말) 짧은 인력 확보 | 신속한 매칭, 신뢰 가능한 인력, 간단한 관리 |
| 운영자/코디네이터 | 참여자 온보딩과 매칭 품질 관리 | 참여 이력 모니터링, 프로그램 운영, 매칭 승인 |

---

## 2. 서비스 흐름 (단계형 구조)

```
[가입/온보딩] 
   → [커뮤니티 참여] (게시판, 소모임, 온라인 활동으로 정서적 안정)
   → [소규모 사회참여 프로그램] (오프라인 소모임, 워크숍 등 저부담 활동)
   → [초단기 근로 매칭] (2~4시간 단위, 소상공인과 연결)
   → [활동 이력 축적 → 데이터 기반 추천 → 반복 참여/확장]
```

---

## 3. 핵심 기능 (MVP 기준)

### 3.1 커뮤니티 모듈
- 게시판/피드 (익명 또는 닉네임 기반)
- 소모임 개설/참여 (관심사 기반)
- 정서 지원 콘텐츠 (체크인, 감정 기록 등 — 진단이 아닌 자기기록 목적)

### 3.2 단계형 참여 관리
- 참여자별 현재 단계 추적: `커뮤니티만 참여` → `프로그램 참여 가능` → `근로 매칭 가능`
- 단계 전환 조건 설정 (예: 커뮤니티 활동 N회 이상 → 프로그램 추천)
- 코디네이터의 수동 승인/조정 기능

### 3.3 초단기 근로 매칭
- 소상공인의 구인 등록 (시간대, 업무 유형, 필요 인원, 시급)
- 참여자의 신청/매칭
- 매칭 확정 후 출근 체크인/체크아웃, 후기·만족도 기록

### 3.4 데이터 기반 추천 (AI 보조, 진단 아님)
- 입력: 참여 이력, 관심분야 태그, 활동 만족도, 근로 매칭 이력
- 출력: "다음에 참여하면 좋을 프로그램/근로 추천 리스트"
- 방식: 초기에는 규칙 기반(태그 매칭 + 최근 활동 유사도) → 데이터 누적 후 협업 필터링/추천 모델로 고도화 가능
- **주의**: 참여자의 심리 상태를 "평가"하거나 "등급화"하는 기능은 만들지 않는다 (제안서의 핵심 방어 논리).

### 3.5 소상공인 관리
- 매장 등록, 구인 공고 등록
- 매칭 이력/후기 관리
- 구독형 요금제 또는 매칭 수수료 결제 (MVP는 무료/베타로 시작 가능)

### 3.6 운영자(코디네이터) 대시보드
- 전체 참여자 현황, 단계별 분포
- 매칭 현황, 노쇼/이슈 관리
- 프로그램 개설/일정 관리

---

## 4. 추천 기술 스택 (MVP 규모에 맞춘 실용적 선택)

| 영역 | 기술 | 비고 |
|---|---|---|
| 프론트엔드 (웹) | Next.js / React | 커뮤니티+매칭 웹앱 |
| 모바일이 필요하면 | Flutter 또는 반응형 웹으로 대체 | 예산/인력에 따라 결정 |
| 백엔드 | FastAPI 또는 Node.js(Express/NestJS) | 팀 역량에 맞춰 택1 |
| DB | PostgreSQL 또는 MySQL | 관계형 데이터 위주라 RDB 적합 |
| 인증 | JWT 기반 자체 인증 또는 소셜 로그인(카카오) | 청년 대상이라 카카오 로그인 권장 |
| 추천 로직 | Python 규칙 기반 스코어링 → 추후 scikit-learn 협업필터링 | 초기엔 AI 모델 불필요, 규칙으로 충분 |
| 알림 | 카카오 알림톡 또는 앱 푸시(FCM) | 참여자 특성상 문자/알림톡이 접근성 높음 |
| 배포 | 우선 단일 서버(예: Railway, Render, 또는 국내 클라우드) | MVP 단계에서 과한 인프라 불필요 |

---

## 5. DB 스키마 초안

```sql
CREATE TABLE users (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  role ENUM('YOUTH','BUSINESS','COORDINATOR') NOT NULL,
  nickname VARCHAR(50),
  phone VARCHAR(20),
  kakao_id VARCHAR(100),
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE youth_profiles (
  user_id BIGINT PRIMARY KEY,
  stage ENUM('COMMUNITY_ONLY','PROGRAM_READY','WORK_READY') DEFAULT 'COMMUNITY_ONLY',
  interest_tags JSON,
  community_activity_count INT DEFAULT 0,
  program_participation_count INT DEFAULT 0,
  work_participation_count INT DEFAULT 0,
  last_active_at DATETIME
);

CREATE TABLE businesses (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  owner_user_id BIGINT NOT NULL,
  business_name VARCHAR(100),
  business_type VARCHAR(50),
  address VARCHAR(200),
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE community_posts (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  user_id BIGINT NOT NULL,
  content TEXT,
  is_anonymous BOOLEAN DEFAULT TRUE,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE programs (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(100),
  description TEXT,
  tags JSON,
  capacity INT,
  scheduled_at DATETIME
);

CREATE TABLE program_participations (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  program_id BIGINT NOT NULL,
  user_id BIGINT NOT NULL,
  status ENUM('APPLIED','CONFIRMED','ATTENDED','NO_SHOW') DEFAULT 'APPLIED',
  satisfaction_score INT, -- 1~5
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE job_postings (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  business_id BIGINT NOT NULL,
  title VARCHAR(100),
  time_slot VARCHAR(50), -- 예: '평일 11:00-14:00'
  hours DECIMAL(3,1),
  hourly_wage INT,
  required_count INT,
  tags JSON,
  status ENUM('OPEN','CLOSED') DEFAULT 'OPEN',
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE job_matches (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  job_posting_id BIGINT NOT NULL,
  user_id BIGINT NOT NULL,
  status ENUM('MATCHED','CHECKED_IN','CHECKED_OUT','NO_SHOW','CANCELLED') DEFAULT 'MATCHED',
  satisfaction_score INT,
  business_feedback TEXT,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE recommendations_log (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  user_id BIGINT NOT NULL,
  recommended_type ENUM('PROGRAM','JOB'),
  recommended_id BIGINT NOT NULL,
  reason VARCHAR(200),
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```
