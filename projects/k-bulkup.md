# K-Bulkup — 금융 교육 웹 플랫폼

> KB IT's Your Life 프로젝트 · 6인 팀 · Frontend 중심 Full-stack
>
> [Organization](https://github.com/K-Bulkup) · [Frontend 저장소](https://github.com/K-Bulkup/Frontend) · [Backend 저장소](https://github.com/K-Bulkup/Backend) · [← 프로필로 돌아가기](../README.md)

## 프로젝트 개요

헬스장에서 운동하듯 금융을 훈련하는 플랫폼입니다.
금융 트레이너가 트레이닝(커리큘럼과 루틴)을 개설하면, 회원이 트레이닝을 구매해 루틴(퀴즈)을 수행하고 1:1 PT와 자산 관리 기능으로 금융 습관을 기릅니다.

| 구분 | 팀 기술 스택 |
|---|---|
| Frontend | Vue 3, Vite, Pinia, Vue Router, Tailwind CSS, Chart.js |
| Backend | Java 17, Spring MVC 5.3, Spring Security(JWT), MyBatis, MySQL, MongoDB, Redis, AWS S3 |

제 커밋은 Frontend 134건(팀 내 최다), Backend 18건입니다(develop 브랜치, 머지 커밋 제외).

**다른 팀원 담당**: 루틴 AI 채점과 결과 처리, 로그인·회원가입·OAuth, CI/CD·Docker, 1:1 PT 채팅, 결제, 관리자 기능

## 내가 담당한 기능

### 1. 트레이닝 등록 (주 담당)

트레이너가 트레이닝을 개설하는 흐름 전체를 화면과 API 양쪽에서 구현했습니다.

- **화면**: 카테고리 선택 → 커리큘럼·루틴 작성 → 썸네일 등록 → 완료로 이어지는 4단계 등록 페이지와 루틴 추가 모달
- **API**: 트레이닝 등록 API와 썸네일 S3 업로드 호출, 프론트 연동을 위한 요청 DTO 정리
- **도메인 정리**: `Training`, `Routine` 도메인을 setter 대신 정적 팩토리 메서드로 생성하도록 리팩터링
- **규칙 반영**: 난이도에 따라 포인트와 가격을 고정값으로 처리, 루틴 답안이 저장되지 않던 오류 수정

### 2. 루틴 수행 화면 (초기 버전)

- 회원 루틴 수행 페이지, 답안 제출 바, 정답·오답 모달, 회원 트레이닝 상세 화면의 첫 버전을 만들었습니다.
- 이후 팀원이 페이지 대부분을 다시 작성했습니다. 루틴 수행·채점 백엔드는 다른 팀원 담당입니다.

### 3. 리뷰·검색 API 개선

- 트레이닝 리뷰 응답에 평균 평점과 리뷰 수를 함께 주는 요약 DTO와 평균 평점 쿼리를 추가했습니다.
- 리뷰 조회 경로를 트레이너 전용에서 공통 경로로 옮겨 회원 화면에서도 쓸 수 있게 했습니다.
- 트레이너 본인의 트레이닝 목록·검색 API `GET /trainer/trainings`를 추가했습니다. 제목·트레이너 이름 검색에서 공백을 무시하고(`REPLACE(… ,' ','') LIKE`), 승인 상태를 함께 반환합니다.
- 프론트엔드에서 리뷰 목록, 반 별점을 지원하는 별점 컴포넌트, 리뷰 작성 화면을 연동했습니다.

### 4. UI/UX 수정

- 회원 마이페이지(레벨, 성장 카드, 수집 카드), 트레이닝 등록·상세·루틴 수행 화면 디자인 수정
- 공통 컴포넌트(`BaseHeader`, `BaseBadge`, `BaseTag` 등)와 트레이닝 승인 상태 뱃지

## 코드 바로가기

- [TrainerTrainingInputPage.vue](https://github.com/K-Bulkup/Frontend/blob/develop/src/views/trainer/TrainerTrainingInputPage.vue) — 트레이닝 4단계 등록 페이지
- [TrainerRoutineAddModal.vue](https://github.com/K-Bulkup/Frontend/blob/develop/src/components/trainer/training/TrainerRoutineAddModal.vue) — 루틴 추가 모달
- [TrainingServiceImpl.java](https://github.com/K-Bulkup/Backend/blob/develop/src/main/java/com/kbulkup/training/service/TrainingServiceImpl.java) — 트레이닝 등록 서비스
- [Backend PR #144 트레이너 트레이닝 목록 조회 및 검색 API](https://github.com/K-Bulkup/Backend/pull/144)
- [Frontend PR 목록](https://github.com/K-Bulkup/Frontend/pulls?q=is%3Apr+author%3Asangeune) · [Backend PR 목록](https://github.com/K-Bulkup/Backend/pulls?q=is%3Apr+author%3Asangeune)
