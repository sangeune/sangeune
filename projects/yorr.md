# YORR — 휴대폰이 컨트롤러가 되는 실시간 웹 파티게임

> SSAFY 15기 공통 프로젝트 · 6인 팀 · 2026-07-06 ~ 2026-08-13 · **공통 프로젝트 발표 1위**
>
> [원본 저장소](https://github.com/team-yorr/yorr) · [서비스](https://yorr.site) · [← 프로필로 돌아가기](../README.md)

<img src="https://raw.githubusercontent.com/team-yorr/yorr/main/docs/play-game.png" alt="요트 다이스, 탁구, 석양이 진다 플레이 화면" width="100%" />

## 프로젝트 개요

여러 사람이 한 공간에서 게임을 하려면 보통 전용 기기나 앱 설치가 필요합니다.
YORR는 이미 가지고 있는 휴대폰의 모션 센서를 컨트롤러로 사용해, 링크나 QR만으로 바로 참여하는 웹 게임 플랫폼입니다.
센서를 쓸 수 없는 환경에서는 화면 탭으로 끝까지 플레이할 수 있습니다.

- 게임: 요트 다이스(1–6명), 탁구(1–2명), 석양이 진다(2명)
- 배포: 2026-07-30 요트 다이스 1차 배포 → 사용자 피드백 반영 → 2026-08-05 탁구·석양이 진다 추가 배포

## 운영 결과 (팀 성과, 실제 서비스 지표)

2026-08-09 기준 실제 배포 환경에서 집계한 값입니다.

| 최고 동시 접속 | 누적 게임 참여 | 플레이한 판 수 |
|:---:|:---:|:---:|
| **80명** | **635건** | **487판** |

> 누적 게임 참여(635건)는 참가 횟수이고, 487판은 진행된 게임 수입니다. 사용자 수와 판 수는 다른 지표입니다.

## 팀 기술 스택과 내 구현 범위

| 구분 | 팀 전체 기술 스택 | 내가 직접 구현한 범위 |
|---|---|---|
| Backend | Java 21, Spring Boot 4.1, Spring WebSocket, JPA, Flyway | 요트 규칙 도메인, 점수 API, AI 봇 정책, 탁구 서버 규칙 |
| Frontend | React 19, TypeScript, Three.js, Rapier | 탁구 3D 화면과 휴대폰 컨트롤러 화면 |
| Data / Realtime | MySQL, Redis, WebSocket, WebRTC | — (팀원 담당) |
| Infra | Docker, Nginx, Jenkins, Prometheus, Grafana | — (팀원 담당) |

WebSocket 이벤트 프로토콜, Redis 방·세션 상태 관리, 재접속, 소셜 로그인, 인프라는 다른 팀원이 담당했습니다.
제 작업은 팀이 만든 실시간 구조 위에서 게임 규칙과 AI, 탁구 게임을 구현하는 부분이었습니다.

## 내가 담당한 기능

### 1. 요트 다이스 규칙 도메인과 점수 API

- 12개 족보 판정, 점수 계산, 상단 보너스, 승패·순위 판정 로직을 작성하고 단위 테스트를 함께 작성했습니다.
- 점수 후보 조회 API `POST /api/v1/games/{gameId}/score-candidates`
- 점수판·최종 결과 조회 API `GET /api/v1/rooms/{roomId}/scores`, `GET /api/v1/rooms/{roomId}/results`
- 점수 확정과 라운드 제출 흐름을 연결했습니다.

### 2. Expectimax 기반 요트 AI 봇

혼자 접속한 사용자도 게임을 즐길 수 있도록 AI 봇 대전을 만들었습니다.

- **문제**: 처음의 규칙 기반 봇은 높은 눈과 최빈값만 유지했고, 현재 점수만 비교해 남은 칸의 가치를 고려하지 못했습니다.
- **접근**: 가능한 모든 HOLD 조합과 점수 기록 행동을 비교하는 Expectimax 탐색을 구현했습니다.
  - 현재 턴의 남은 최대 2번 재굴림만 정확한 확률로 탐색하고, 이후 가치는 점수판 평가 함수로 추정했습니다.
  - 눈별 개수가 같은 주사위 결과를 하나의 상태로 묶고, 같은 상태의 계산값을 메모이제이션했습니다.
  - 남은 점수 칸의 가치와 상단 보너스 달성 가능성을 평가 함수에 반영했습니다.
- **검증**: 정책별 비교를 위한 결정적 시뮬레이터를 만들고, 동일한 test 시드로 각 정책을 10,000판씩 평가했습니다.

| 정책 | 평균 점수 (10,000판, 동일 시드) |
|---|---:|
| 규칙 기반 봇 | 107.49 |
| 증류 MLP (Expectimax 판단을 학습한 회귀 모델) | 153.07 |
| **Expectimax (서비스 적용)** | **179.93** (+67.39%) |

- 첫 굴림 대표 상태에서 2회 재굴림 탐색이 1초 안에 끝나는지 단위 테스트로 확인했습니다.
- 실제 플레이에서 발견한 스몰 스트레이트 선택 우선순위 오류, 같은 눈 주사위 KEEP 식별자 문제를 수정하고 회귀 테스트를 추가했습니다.

### 3. 탁구 게임 이식과 휴대폰 컨트롤러

팀원이 개인 프로젝트로 만든 탁구 게임을 YORR 서비스 구조로 옮기는 작업을 맡았습니다.

- 서버: 팀의 게임 모듈 구조에 맞춰 탁구 규칙(득점, 듀스, 매치포인트)과 게임 상태를 구현했습니다.
- 프론트엔드: Three.js 기반 탁구 3D 화면과 스윙 입력을 구현했습니다.
- 파티 모드: 큰 화면 하나와 휴대폰 컨트롤러를 짝지어 휴대폰을 탁구채처럼 휘두르는 컨트롤러 화면과 연결 흐름을 만들었습니다.
- 실제 플레이에서 나온 모션 스윙 활성화, 플레이어 구분, 피드백 가독성 문제를 반복해서 수정했습니다.

## 코드 바로가기

- [ExpectimaxYachtBotPolicy.java](https://github.com/team-yorr/yorr/blob/main/backend/src/main/java/com/ssafy/yorr/game/yacht/ExpectimaxYachtBotPolicy.java) — Expectimax 정책
- [ScorecardValueEvaluator.java](https://github.com/team-yorr/yorr/blob/main/backend/src/main/java/com/ssafy/yorr/game/yacht/ScorecardValueEvaluator.java) — 점수판 가치 평가 함수
- [ExpectimaxYachtBotPolicyTest.java](https://github.com/team-yorr/yorr/blob/main/backend/src/test/java/com/ssafy/yorr/game/yacht/ExpectimaxYachtBotPolicyTest.java) — 판단 회귀 테스트와 1초 응답 테스트
- [AI 시뮬레이터·증류 파이프라인](https://github.com/team-yorr/yorr/tree/feature/217-yacht-ai-simulator/backend/src/main/java/com/ssafy/yorr/game/yacht/simulation) — `feature/217-yacht-ai-simulator` 브랜치
- [PingPongRules.java](https://github.com/team-yorr/yorr/blob/main/backend/src/main/java/com/ssafy/yorr/game/pingpong/PingPongRules.java) — 탁구 서버 규칙
- [내 커밋 목록](https://github.com/team-yorr/yorr/commits/main/?author=sangeune)

## 배운 점

로컬에서는 정상이던 기능도 실제 사용자가 여러 기기로 접속하자 다른 문제가 드러났습니다.
AI 봇의 판단 오류도 사용자 플레이에서 발견했고, 발견한 사례를 회귀 테스트로 남겨 같은 문제가 다시 생기지 않도록 했습니다.
