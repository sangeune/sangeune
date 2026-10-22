# 유상은 · Sangeun You

산업공학 기반의 데이터 분석과 Spring 백엔드 개발 경험을 가진 신입 개발자입니다.<br>
실사용자 서비스 운영, 금융 API 개발, 데이터 정합성 검증을 경험했습니다.

`Java` `Spring Boot` `Spring MVC` `MyBatis` `MySQL` `PostgreSQL` `Python` `Vue 3` `React`

[Tech Blog](https://yseee.tistory.com/) · [GitHub](https://github.com/sangeune)

<br>

## YORR — 휴대폰이 컨트롤러가 되는 실시간 웹 파티게임

SSAFY 15기 공통 프로젝트 · 6인 팀 · AI · Backend 담당 · **발표 1위**

<img src="https://raw.githubusercontent.com/team-yorr/yorr/main/docs/play-game.png" alt="YORR 요트 다이스, 탁구, 석양이 진다 플레이 화면" width="100%" />

<table>
  <tr>
    <td align="center" width="33%"><h3>80명</h3><sub>최고 동시 접속</sub></td>
    <td align="center" width="33%"><h3>635건</h3><sub>누적 게임 참여</sub></td>
    <td align="center" width="33%"><h3>487판</h3><sub>플레이한 게임</sub></td>
  </tr>
</table>

<sub>2026-08-09 기준 실제 서비스 운영 지표 (팀 성과)</sub>

- **Expectimax 요트 AI 봇** — 동일 시드 10,000판 평균 점수 107.49 → **179.93 (+67.39%)**
- **요트 규칙·점수 API** — 12족보 판정, 점수·보너스 계산, 승패·순위 판정
- **탁구 게임 이식** — 서버 경기 규칙, Three.js 3D 화면, 휴대폰 컨트롤러 연결

<sub>팀 스택: Spring Boot · React · MySQL · Redis · WebSocket · WebRTC — WebSocket·Redis·인프라는 다른 팀원 담당</sub>

**[Repository](https://github.com/team-yorr/yorr) · [Live Demo](https://yorr.site) · [Case Study](projects/yorr.md)**

<br>

## Projects

### 가게채움 · 금융 IT Backend
소상공인 대출·정책 지원 통합 플랫폼 · KB IT's Your Life 해커톤 **장려상**

- 지역·업종 기반 정책·대출 추천, 신청 서류 현황, 월별 현금 흐름(상환 방식별 상환액) API 개발 · PR 23건 머지
- 응답 정합성 수정: `UNION ALL` 컬럼 불일치, 필요 서류 중복 삽입(UNIQUE + `NOT EXISTS`)

`Java` `Spring MVC` `MyBatis` `MySQL` · [Repository](https://github.com/Gagechaeum/backend) · [상세](projects/gagaechaeum.md)

### WikiPulse · Data Pipeline
위키백과 편집·조회수 급증 문서를 사건 단위로 묶어 관련 종목과 보여주는 서비스 · SSAFY 특화 프로젝트

- 과거 덤프 적재, 실시간·과거 결과 출처 분리(DB 제약), 클러스터링 v2 프로덕션 반영
- 상위 문서 선택 적용 시 20개+ 거대 클러스터 59 → 0, 무손실 사전필터 1,960조합·20,000구간 불일치 0

<sub>급증 판정 수식·기준선, Kafka 인프라, 뉴스·LLM 종목 매칭은 다른 팀원 담당</sub>

`Python` `PostgreSQL` `PySpark` `pytest` · [Repository](https://github.com/team-926/WikiPulse) · [상세](projects/wikipulse.md)

### K-Bulkup · 금융 교육 웹 서비스
금융 트레이너와 회원을 연결하는 금융 교육 플랫폼 · KB IT's Your Life 프로젝트

- 트레이닝 4단계 등록 화면·API, 리뷰 평점 집계·트레이닝 검색 API, 마이페이지 UI 개선
- 팀 내 Frontend 커밋 최다(134건) · AI 채점, 로그인, 인프라는 다른 팀원 담당

`Vue 3` `Spring MVC` `MyBatis` `MySQL` · [Repository](https://github.com/K-Bulkup/Frontend) · [상세](projects/k-bulkup.md)

<br>

## Data Analysis

### 온마을 · 식품 사막 지역 이동식 식료품 서비스 기획
3인 팀 · 2024 전북 공공데이터 활용 창업경진대회 **우수상**

2SFCA로 식료품 접근성을 분석하고, MCLP로 이동식 상점 정차 거점 25곳을 선정했습니다.
분석 모델 기준 접근성 **31.89% 개선** <sub>(실제 운영 성과가 아닌 분석 모델 결과 · 공개 소스코드 없음)</sub>

<br>

## Contact

[yseee.tistory.com](https://yseee.tistory.com/) · [github.com/sangeune](https://github.com/sangeune)

<sub>각 프로젝트는 팀 원본 저장소로 연결되며, 상세 문서에 팀 기술 스택과 개인 구현 범위를 구분해 정리했습니다.</sub>
