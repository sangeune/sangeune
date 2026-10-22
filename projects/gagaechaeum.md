# 가게채움 — 소상공인 대출·정책 지원 통합 플랫폼

> KB IT's Your Life 해커톤 · 6인 팀 · Backend 담당 · **장려상**
>
> [Backend 저장소](https://github.com/Gagechaeum/backend) · [Frontend 저장소](https://github.com/Gagechaeum/frontend) · [← 프로필로 돌아가기](../README.md)

## 프로젝트 개요

소상공인이 받을 수 있는 정책 지원금과 대출 상품을 지역·업종에 맞게 추천하고,
신청에 필요한 서류 준비 현황과 월별 현금 흐름(지원금 수령, 대출 상환)을 한 화면에서 관리하는 서비스입니다.

| 구분 | 기술 |
|---|---|
| Backend (팀) | Java 17, Spring MVC 5.3 (Gradle WAR), MyBatis, MySQL, Spring Security, JWT |
| Frontend (팀) | Vue 3, Vite, Pinia, axios |
| 내가 주로 다룬 부분 | Spring MVC Controller·Service, MyBatis Mapper XML(SQL), MySQL 스키마 일부, Vue API 연동 |

기획서의 역할 분담에서 제 담당은 대출·지역 정책 추천, 서류 확인, 납부 금액 대시보드였습니다.
로그인·JWT·OAuth2, 마이데이터 연동, 채팅·알림, 서류 업로드·암호화·S3 저장은 다른 팀원이 담당했습니다.

## 내가 개발한 API

### 1. 정책·대출 추천과 검색

| API | 내용 |
|---|---|
| `GET /api/policies/recommendation` | 사업장 지역·업종이 맞는 정책을 북마크 수(인기순)로 정렬해 추천 |
| `GET /api/loans/recommendation` | 지역과 업종이 **동시에** 맞는 대출 상품 추천 |
| `POST /api/policies/sync` | 정부24 API에서 정책 데이터를 받아 동기화. 텍스트로 된 신청 기간을 시작일·종료일로 파싱 |
| `GET /api/search` | 홈 화면 정책·대출 통합 검색 |

### 2. 신청 서류 조회와 신청 상태 관리

| API | 내용 |
|---|---|
| `GET /api/me/documents` | 내 서류 목록 |
| `GET /api/me/bookmarks/progress` | 관심 상품별 서류 준비 현황 |
| `GET /api/products/{productId}/documents` | 상품별 필요 서류 체크리스트 |
| `GET /api/documents/{documentId}/products` | 서류 하나로 신청 가능한 상품 목록 |
| `PATCH /api/me/policies/{id}/status` 외 | 정책·대출 신청 상태 변경 |
| `POST`·`DELETE /api/me/{policies\|loans}/{id}/bookmark` | 관심 상품 등록·삭제 |

### 3. 현금 흐름과 납부 정보 대시보드

- `GET /api/reports/dashboard`: 요약, 2주 일정, 월별 현금 흐름(정책 지원금 수령과 대출 상환)을 한 번에 반환합니다.
- 대출 상환 방식(원금균등분할상환, 만기일시상환)에 따라 월별 상환 금액과 이자를 계산하도록 대시보드를 DB 연동 방식으로 바꿨습니다.
- `GET /api/reports/items`의 페이징을 애플리케이션 단에서 DB 쿼리 단으로 옮겼습니다.
- 리포트 API가 요청 파라미터의 userId 대신 인증된 사용자 정보를 쓰도록 바꿨습니다.

## 데이터 정합성 문제 해결

| 문제 | 해결 |
|---|---|
| 신청 진행 상태(요건확인 → 서류 수집/업로드 → 제출 준비 → 제출 완료/결과)를 관심 상품 단위로 관리해야 함 | 상태 컬럼을 사용자 정책·대출 테이블에서 관심 상품(북마크) 테이블로 옮기고, 상태 변경 API를 구현 |
| 정책·대출 북마크를 합친 `UNION ALL` 조회의 컬럼 불일치 | 두 쿼리의 컬럼을 맞추고 응답에 `policyId`·`loanId`를 포함 |
| 정책별 필요 서류 행이 중복 생성 | UNIQUE 제약과 `INSERT … WHERE NOT EXISTS`로 중복 삽입 방지 |
| 성공 응답 코드가 200·201로 나뉘어 있었음 | 공통 응답 래퍼 `CustomResponse`를 작성하고 성공 응답 코드를 하나로 통일 |

## 프론트엔드 연동

- 리포트 페이지의 API 연동, 페이지네이션·정렬, 상세 페이지 이동과 상환일 팝업을 연결했습니다.
- 관심 상품 등록·삭제, 일정 페이지의 정책·대출 데이터 연동 오류를 수정하고 백엔드의 중복 호출을 제거했습니다.
- 화면 디자인과 대부분의 UI 컴포넌트는 프론트엔드 팀원이 만들었습니다.

## 코드 바로가기

- [ReportServiceImpl.java](https://github.com/Gagechaeum/backend/blob/develop/src/main/java/com/gagechaeum/backend/report/service/ReportServiceImpl.java) — 대시보드·월별 상환 금액 계산
- [BookmarkMapper.xml](https://github.com/Gagechaeum/backend/blob/develop/src/main/resources/com/gagechaeum/backend/bookmark/mapper/BookmarkMapper.xml) — 서류 진행 현황·상태 SQL
- [PR #34 리포트 대시보드 DB 연동 및 월별 상환 금액 계산](https://github.com/Gagechaeum/backend/pull/34)
- [PR #13 대출 추천 API 및 정책 데이터 동기화](https://github.com/Gagechaeum/backend/pull/13)
- [Backend PR 목록 (23건 머지)](https://github.com/Gagechaeum/backend/pulls?q=is%3Apr+author%3Asangeune) · [Frontend PR 목록](https://github.com/Gagechaeum/frontend/pulls?q=is%3Apr+author%3Asangeune)
