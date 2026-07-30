# 📝 3주차 실습 정리 — InSooBeen

📅 **07.20(월) ~ 07.26(일) 과제 주차** — 실제 착수는 07.26부터. Part 2. 실전 프로젝트 · 기획 & 착수

## 프로젝트: Kinly — 가족 건강 동반자

> 📎 기획서 전체: [Notion](https://cookie-moonstone-7ce.notion.site/369a44f2e1e3807282cbc0b6c657396e)

떨어져 사는 부모님을 챙기는 20~40대 자녀를 위한, **평소엔 안부 확인 앱·위기엔 비상 정보 금고**로 작동하는 가족 단위 건강 관리 서비스. 핵심 3기능(비상 정보 금고 / 안전 체크인 / 컨디션 추세)의 6주 계획 중 3주차는 **①비상 정보 금고**에 해당.

저장소는 실제 의료·비상 정보를 다루는 프로젝트라 **개인 private 레포**(`InSooBeen/kinly`)로 운영 중입니다. 코드 열람이 필요하시면 별도로 초대해 드릴 수 있고, 아래 각 일차 항목엔 **그날의 결정·트러블슈팅을 자세히 기록한 Notion 페이지**를 링크했습니다.

## ✅ 체크리스트

- [x] 프로젝트 주제 확정 — 가족 건강 동반자(Kinly)
- [x] 요구사항/기능 목록 문서화 — ADR 14건 (`docs/decisions/`)
- [x] 프로젝트 레포 셋업 — 모노레포(`server` Spring Boot + `app` Expo), Flyway 마이그레이션 9테이블
- [x] 프로젝트용 `CLAUDE.md` 작성
- [x] 첫 커밋 & 개발 착수 — 가족 그룹 / 구성원 프로필 / 비상 정보 금고 API까지 구현·테스트 완료

## 일별 진행 상황

### 1일차 · 2026-07-26

- 기술 스택 전체 확정(Android 네이티브 + Expo, Spring Boot 4.1 + PostgreSQL, AWS Lightsail 등) — 처음엔 웹 PWA를 검토했으나 **"데모냐 실사용이냐"**를 다시 따져 네이티브 앱으로 전환
- 코어 도메인 설계 — 계정(User)과 사람(Person) 분리, 비상정보는 그룹이 아닌 **Person에 귀속**시켜 다중 가족 그룹 중복 문제 해결
- `kinly` 모노레포 생성, ADR 11건 + `V1__init.sql`(9테이블) 작성
- WSL 개발 환경 구축, `./gradlew compileJava` **BUILD SUCCESSFUL**

📎 상세: [Notion — 3주차 1일차](https://cookie-moonstone-7ce.notion.site/3-1-2026-07-26-3aaa44f2e1e3817b8242e94ce1997be7?source=copy_link)

### 2일차 · 2026-07-27

- Flyway 마이그레이션 실행, 필드 암호화 계층(`AES-256-GCM`) 구현 — 단위 테스트 8건 통과
- Expo 앱 스캐폴딩, **GitHub private 레포 생성 + 첫 푸시**, 브랜치 전략(`main` 단독) 확정
- 카카오 로그인 리다이렉트 후보가 전부 막혀(커스텀 스킴·포트 제한·Expo 인증 프록시 폐지) **서버 콜백 방식**으로 인증 설계 전환
- 가족 그룹 API → 구성원 프로필 API → 비상 정보 금고 API를 세션 10회에 걸쳐 순차 설계·구현
- 전체 테스트 **47건 통과**로 마감

📎 상세: [Notion — 3주차 2일차](https://cookie-moonstone-7ce.notion.site/3-2-2026-07-27-3aaa44f2e1e3811db307fc4762f3f4d5?source=copy_link)

### 3일차 · 2026-07-28

- worklog·HANDOFF 날짜 체계를 실제 달력 기준으로 정합화
- 비상 정보 금고 API 커밋(`b819563`)
- **우선순위 재조정** — "핵심 기능부터"라는 피드백에 따라 PIN·감사로그 등 부가 보안 계층 대신 **앱 화면 구현**을 다음 착수 대상으로 결정
- `superpowers:brainstorming` 스킬로 앱 화면(로그인 → 그룹 생성/합류) 설계 스펙 5개 섹션 전부 승인받아 완료·커밋(`a61d37a`)
- `superpowers:writing-plans` 스킬로 구현 계획(Task 1~10) 작성 — 서버 소스 재확인 + Expo Router v57 공식 문서·타입 선언 직접 검증(스펙의 `<Redirect>`를 v57 권장 패턴인 `<Stack.Protected guard>`로 교체 결정 포함), 실행 방식은 **Subagent-Driven** 선택 → 계획 문서 커밋(`563081b`)
- `superpowers:subagent-driven-development`로 SDD 착수 — Task 1(API 클라이언트 `client.ts` + Jest 인프라) 구현·리뷰·커밋(`05f384f`), Task 2(`auth.ts`/`groups.ts` 엔드포인트 함수) 구현(스테이징까지)

📎 상세: [Notion — 3주차 3일차](https://cookie-moonstone-7ce.notion.site/3-3-2026-07-28-3aba44f2e1e381c78e35de1f3c201f30?source=copy_link)

### 4일차 · 2026-07-30

- 백엔드 전용 세션 — PostgreSQL 기동 후 서버 전체 테스트 재검증(**BUILD SUCCESSFUL**, 실패 0건), 이력 조회·되돌리기(revert) 엔드포인트 커밋(`0b00f1d`), ADR 0015 커밋(`9f4b62e`)

📎 상세: [Notion — 3주차 4일차](https://cookie-moonstone-7ce.notion.site/3-4-2026-07-30-3ada44f2e1e38193bd56dfb38d2d9594?source=copy_link)

## 다음 할 것

- SDD Task 2 리뷰 → 커밋 승인 → Task 3부터 계획 문서 순서대로 Task 10까지 진행(Task 5부터 실기기/에뮬레이터 확인 필요)
- PIN 설정·검증, vault 토큰, 접근 감사 로그 — 앱 화면 조각 이후 순서 재확인
