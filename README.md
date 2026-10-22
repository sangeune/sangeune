# 유상은 · Sangeun You

**데이터로 문제를 정의하고, 서비스로 구현하는 백엔드 개발자**

산업정보시스템공학을 전공하며 데이터 분석과 최적화로 문제를 구조화하는 방법을 배웠고,
SSAFY와 KB IT's Your Life에서 Spring 기반 백엔드와 데이터 파이프라인을 개발했습니다.

- 실제 사용자에게 공개한 웹 게임 서비스에서 게임 규칙 API와 AI 봇을 개발하고, 사용자 피드백으로 기능을 고쳤습니다.
- 금융 서비스 백엔드에서 대출·정책 추천, 신청 서류, 현금 흐름 API를 개발했습니다.
- 데이터 파이프라인에서 DB 제약과 회귀 테스트로 결과 정합성을 검증했습니다.

[Tech Blog](https://yseee.tistory.com/) · [GitHub](https://github.com/sangeune)

<br>

## Featured Projects

| | 프로젝트 | 성격 | 내 역할 | 핵심 결과 |
|:-:|---|---|---|---|
| 1 | [**YORR**](#yorr--휴대폰이-컨트롤러가-되는-실시간-웹-파티게임) | 실서비스 개발·운영 | AI · Backend | 발표 1위, 최고 동시 접속 80명 |
| 2 | [**가게채움**](#가게채움--소상공인-대출정책-지원-통합-플랫폼) | 금융 IT 백엔드 | Backend API | KB 해커톤 장려상, PR 23건 머지 |
| 3 | [**WikiPulse**](#wikipulse--위키백과-급증-데이터-기반-이슈-탐지) | 데이터 파이프라인 | Data Pipeline | 거대 클러스터 59 → 0 |
| 4 | [**K-Bulkup**](#k-bulkup--금융-교육-웹-플랫폼) | 금융 교육 웹 서비스 | Frontend 중심 Full-stack | 팀 내 Frontend 커밋 최다, 트레이닝 등록 기능 |

<br>

### YORR — 휴대폰이 컨트롤러가 되는 실시간 웹 파티게임

> SSAFY 15기 공통 프로젝트 · 6인 팀 · 2026-07-06 ~ 2026-08-13 · **공통 프로젝트 발표 1위**

<img src="https://raw.githubusercontent.com/team-yorr/yorr/main/docs/play-game.png" alt="YORR 요트 다이스, 탁구, 석양이 진다 플레이 화면" width="100%" />

앱 설치 없이 링크나 QR로 접속하고, 휴대폰을 흔들거나 휘둘러 함께 플레이하는 웹 게임 플랫폼입니다.
요트 다이스·탁구·석양이 진다를 제공하며, 실제 사용자에게 두 차례 배포하고 피드백을 반영했습니다.

| 최고 동시 접속 | 누적 게임 참여 | 플레이한 판 수 |
|:---:|:---:|:---:|
| **80명** | **635건** | **487판** |

<sub>2026-08-09 기준 실제 서비스 운영 지표 (팀 성과)</sub>

**내가 담당한 기능** — AI · Backend

- **Expectimax 요트 AI 봇**: 모든 HOLD 조합과 점수 기록 행동의 기대 점수를 비교하는 탐색 AI를 구현했습니다.
  동일 시드 10,000판 시뮬레이션에서 평균 점수 **107.49 → 179.93 (+67.39%)**, 1초 이내 응답을 테스트로 확인했습니다.
- **요트 규칙 도메인과 점수 API**: 12개 족보 판정, 점수·상단 보너스 계산, 승패·순위 판정, 점수 후보·점수판·결과 조회 API
- **탁구 게임 이식**: 서버 경기 규칙, Three.js 3D 화면, 휴대폰을 탁구채로 쓰는 컨트롤러 연결 화면

**Tech** — 팀: Java 21, Spring Boot, React, TypeScript, MySQL, Redis, WebSocket, WebRTC, Docker, Jenkins
<br><sub>WebSocket 프로토콜, Redis 상태 관리, 인프라·모니터링은 다른 팀원 담당</sub>

[원본 저장소](https://github.com/team-yorr/yorr) · [서비스](https://yorr.site) · [상세 소개](projects/yorr.md)

<br>

### 가게채움 — 소상공인 대출·정책 지원 통합 플랫폼

> KB IT's Your Life 해커톤 · 6인 팀 · Backend · **장려상**

소상공인에게 지역·업종에 맞는 정책 지원금과 대출 상품을 추천하고, 신청 서류와 월별 현금 흐름을 관리하는 서비스입니다.

- **추천 API**: 지역·업종 기반 정책·대출 추천, 정부24 API 정책 데이터 동기화(신청 기간 텍스트 파싱)
- **서류·신청 상태 API**: 상품별 필요 서류 체크리스트, 관심 상품의 서류 준비 현황, 신청 상태 변경
- **현금 흐름 대시보드 API**: 정책 지원금 수령과 대출 상환을 월별로 집계, 상환 방식(원금균등·만기일시)별 상환액 계산
- **정합성 수정**: `UNION ALL` 조회 컬럼 불일치, 필요 서류 중복 삽입(UNIQUE + `NOT EXISTS`), 응답 코드 통일

**Tech** — Java 17, Spring MVC, MyBatis, MySQL, Vue 3 (API 연동)

[Backend](https://github.com/Gagechaeum/backend) · [Frontend](https://github.com/Gagechaeum/frontend) · [상세 소개](projects/gagaechaeum.md)

<br>

### WikiPulse — 위키백과 급증 데이터 기반 이슈 탐지

> SSAFY 15기 특화 프로젝트 · 6인 팀 · 데이터 파이프라인 담당

위키백과 편집·조회수가 급증한 문서를 사건 단위로 묶고, 관련 미국 상장 종목 정보를 함께 보여주는 서비스입니다.

- **과거 데이터 적재**: 위키백과 편집 덤프를 실시간 이벤트와 같은 형식으로 정규화하는 적재 CLI
- **출처 분리**: 실시간(`live`)·과거 재생(`replay`) 결과를 DB 고유 키와 CHECK 제약으로 분리
- **클러스터링 프로덕션 반영**: 시점별 상위 문서 선택 + 판정 시점 링크 기반 묶음.
  같은 2개월 재생에서 상위 문서 선택 없이 묶을 때 20개 이상 거대 클러스터 **59개 → 적용 후 0개**
- **무손실 사전필터 검증**: 경계값 1,960개 조합과 난수 20,000개 구간에서 판정 결과 불일치 **0건**

**Tech** — Python, PostgreSQL, PySpark, pytest
<br><sub>급증 판정 수식·기준선, Kafka·Spark 인프라, 뉴스·LLM 종목 매칭은 다른 팀원 담당</sub>

[원본 저장소](https://github.com/team-926/WikiPulse) · [상세 소개](projects/wikipulse.md)

<br>

### K-Bulkup — 금융 교육 웹 플랫폼

> KB IT's Your Life 프로젝트 · 6인 팀

금융 트레이너가 교육 트레이닝을 개설하고, 회원이 루틴(퀴즈)을 수행하며 금융 습관을 기르는 웹 플랫폼입니다.

- **트레이닝 등록 기능**: 카테고리 → 커리큘럼·루틴 → 썸네일 → 완료 4단계 등록 화면(Vue)과 등록 API(Spring), 난이도별 포인트·가격 고정 처리
- **루틴 수행 화면**: 회원 루틴 수행 페이지와 정답·오답 모달의 초기 버전 개발
- **리뷰·검색 API 개선**: 트레이닝 평균 평점·리뷰 수 집계 응답 추가, 트레이너 트레이닝 목록 검색 API(공백 무시 키워드 검색, 승인 상태 포함)
- **UI/UX**: 회원 마이페이지와 트레이닝 상세 화면 디자인 수정, 리뷰·별점 공통 컴포넌트

**Tech** — Vue 3, Pinia, Java 17, Spring MVC, MyBatis, MySQL
<br><sub>AI 채점, 로그인·회원가입, CI/CD·인프라는 다른 팀원 담당</sub>

[Frontend](https://github.com/K-Bulkup/Frontend) · [Backend](https://github.com/K-Bulkup/Backend) · [상세 소개](projects/k-bulkup.md)

<br>

## Data Analysis & Optimization

### 온마을 — 식품 사막 지역을 위한 이동식 식료품 판매 서비스 기획

> 3인 팀 · **2024 전북 공공데이터 활용 창업경진대회 우수상** · 숭실공학상 동상 · 전북도의회 정책 토론회 발표 초청

전북 지역의 식료품 접근성이 낮은 곳을 공공데이터로 찾고, 이동식 상점의 정차 거점과 운행 경로를 설계했습니다.
제 역할은 수요 분석, 입지 선정, 경로 설계였습니다.

1. **접근성 분석** — 2SFCA(공급과 수요 인구를 함께 고려한 접근성 지표)로 전북 14개 시·군을 비교해 대상 지역 선정
2. **입지 선정** — MCLP(정해진 거점 수로 최대 수요를 커버하는 최적화 모델)로 25개 정차 거점 선정
3. **경로 설계** — K-means로 거점을 3개 권역으로 나누고 권역별 운행 경로와 정차 시간표 설계

결과: 분석 모델 기준 식료품 접근성 **31.89% 개선**
<br><sub>실제 운영 성과가 아닌 분석·최적화 모델상의 결과입니다. 공개 가능한 소스코드는 없습니다.</sub>

<br>

## Tech Stack

실제 프로젝트에서 직접 작성한 코드 기준입니다.

| 분야 | 기술 | 사용한 곳 |
|---|---|---|
| Backend | Java, Spring Boot, Spring MVC, MyBatis | YORR, 가게채움, K-Bulkup |
| Database | MySQL, PostgreSQL | 가게채움, K-Bulkup, WikiPulse |
| Data | Python, SQL, PySpark, pytest | WikiPulse, 온마을 |
| Frontend | Vue 3, React, TypeScript, Three.js | K-Bulkup, 가게채움, YORR |
| Collaboration | Git, GitHub, GitLab, Jira | 전 프로젝트 |

<br>

## Contact

- Tech Blog: [yseee.tistory.com](https://yseee.tistory.com/)
- GitHub: [github.com/sangeune](https://github.com/sangeune)
