# 작가작업실 JEJU 홈페이지 개발기준서

2026-11-21 KST · v1.8 · 새 채팅에서 사용하는 유지보수 기준 및 확인 기록

## 1. 새 채팅에서 바로 적용할 작업 범위

**기본 수정·배포 대상은 `https://jakgastudio-temp.vercel.app/`의 테스트 환경이다. 운영 변경과 GitHub 원격 저장소 쓰기는 각각 해당 작업에 대한 사용자의 명시적 승인을 받는다.** 이전 대화에서 승인한 배포는 그때 승인한 변경에 적용된다. 이 문서를 확인하는 것만으로 쓰기 권한이 생기지 않는다.

1. 요청에서 바꿀 화면·기능과 검수 조건을 뽑는다. 아래 연결표의 저장소·Vercel 프로젝트·DB ref를 현재 조회값과 대조한다.
2. 읽기 전용으로 현재 화면, 배포 커밋, 작업 파일, 관련 DB 제약과 권한을 확인한다. 이 문서와 현재 구현이 다르면 차이와 영향을 기록한다. 과거 합의 여부는 직접 승인 기록으로 확인한다.
3. 수정 요청을 받으면 테스트용 로컬 작업본에서 필요한 범위를 준비한다. Vercel 배포 또는 GitHub 쓰기가 수반되면 실행 전 대상과 영향을 확인한다. 작업의 실행 범위는 새 채팅에서 사용자가 승인한 내용으로 확인한다.
4. GitHub 승인에는 대상 저장소·브랜치·변경 요약과 자동 배포 여부를 명시한다. push, 원격 브랜치 생성, PR 생성·병합과 저장소 설정 변경에 이 기준을 적용한다. 운영 변경 승인에는 배포할 커밋, 운영 도메인, DB·Storage·Auth·알림 영향, 복구 계획을 명시한다. 두 범위가 한 변경에 포함되면 승인 문구에서 각각 확인한다.
5. 운영 도메인·배포·환경변수·DB·Storage·Auth·Edge Function·실제 수신자 발송 및 운영 복구를 승인 대상으로 다룬다. 프로젝트 또는 DB 대상이 불명확하면 대상 확인까지 조회와 로컬 검증을 계속한다.

| 표기 | 의미 |
|---|---|
| **확정** | 사용자가 명시한 규칙 또는 직접 승인한 변경 |
| **확인** | 기준일에 GitHub·Vercel·Supabase 및 공개 HTML로 확인한 구현 또는 설정 |
| **추정·제안** | 확인된 구조를 바탕으로 작성한 운용 절차 또는 검수 기준 중 추가 확정이 필요한 항목 |
| **미확인** | 조회 권한 부족, 원본 자료 부재, 실제 동작 검증 미실시 등으로 결론을 내릴 수 없는 항목 |

## 2. 환경별 연결표

배포 식별자는 2026-11-17 조회 당시의 기록이다. 2026-11-18 책방 문구 변경 커밋은 테스트 `2f783ff`, 운영 `eb4168a`이다.

| 항목 | 테스트 | 운영 |
|---|---|---|
| 사이트 | [jakgastudio-temp.vercel.app](https://jakgastudio-temp.vercel.app/) | [www.jakgastudio.com](https://www.jakgastudio.com/). Vercel 등록 도메인에 `jakgastudio.com`, `jakgastudio.vercel.app`도 포함 |
| GitHub | [jeong1pumho/jakgastudio-temp](https://github.com/jeong1pumho/jakgastudio-temp) | [jeong1pumho/jakgastudio](https://github.com/jeong1pumho/jakgastudio) |
| 기본 브랜치 | `main` | `main` |
| Vercel 프로젝트 | `jakgastudio-temp` / `prj_8SUBanJRucDwen3xIjvE1LcYWLwb` | `jakgastudio` / `prj_IHfg8sOhCAj7bbKyo6zEbrNqcmmd` |
| Supabase 프로젝트 ref | `qrbavxempotptnmflqoq` | `dolgltddpnxzwlqigtjw` |
| Vercel 실행환경 | Node.js 24.x, 프레임워크 미지정 | Node.js 24.x, 프레임워크 미지정 |
| 최신 READY 배포에 기록된 Git SHA | [4a84238](https://github.com/jeong1pumho/jakgastudio-temp/commit/4a84238d9f5912617fe94fce51c18c1c9ccd3756) | [821789a](https://github.com/jeong1pumho/jakgastudio/commit/821789ab4caec0f8592f64ccf61511f12a5ac9cc) |
| Vercel 배포 기록 | [dpl_2riuzURQ6gfkSUSF3tBDWUiG33yh](https://vercel.com/jeong1pumho/jakgastudio-temp/2riuzURQ6gfkSUSF3tBDWUiG33yh) | [dpl_BuBFY9ZsV8ePgUV3aCPuHhbB7mJM](https://vercel.com/jeong1pumho/jakgastudio/BuBFY9ZsV8ePgUV3aCPuHhbB7mJM) |

**확인.** 두 GitHub 저장소는 공개다. Vercel의 두 프로젝트는 같은 팀 `team_2v57oR0ikHZoysLjevLtCfoI`에 있으며 최근 배포 메타데이터는 각기 다른 저장소를 가리킨다. **두 프로젝트 모두 `main` 배포가 Vercel에서 `production`으로 표시된다.** 작업 전 프로젝트 ID와 연결 저장소를 확인한다. 배포 환경명이 `production`이어도 테스트 프로젝트에 속할 수 있다. 배포의 `READY`와 실제 도메인 응답은 별도로 확인한다. 기준일 두 사이트 홈은 HTTP 200이었고, 각 공개 HTML에서 각자의 Supabase 호스트가 관측됐다.

**격리 기준.** 데이터 쓰기, 로그인, 업로드, 알림에 테스트 ref와 테스트 계정을 사용한다. 테스트 자료에는 가상 인물과 테스트 수신처를 사용한다. 공개 사진 URL은 운영을 참조하는 코드가 남아 있으므로 사진 변경·삭제 전 양쪽 사이트와 DB 참조를 조사한다. 공개 사진 참조는 실제 테스트 데이터 쓰기 경로와 별도로 확인한다.

## 3. 페이지와 디자인

**책방 상세 화면 (2026-11-18 확정).** 테스트·운영 사이트의 `/programs/bookstore`에는 제목 ‘작가가 만든 제주의 작은 책방’ 다음에 책방 소개 본문을 표시한다. `lib/programs-page.js`에서 책방의 `subtitle`은 빈 문자열로 두어 ‘(모든 도서 10% 할인)’ 문구와 해당 문단을 생략한다.

**확인.** 두 사이트 홈의 메뉴 순서는 다음과 같다.

| 순서 | 메뉴 | 경로와 확인 포인트 |
|---|---|---|
| 1 | Home | `/`. Key Visual, 소개, 추천 도서, 모임·행사, 이번 주 일정, 운영 프로그램, 공간 소개, 방문안내 순 |
| 2 | 작가와 공간 소개 | `/space`. 작가와 공간 소개, 관리자가 편집하는 갤러리. 대관 페이지의 `/space#author-gallery` 연결 확인 |
| 3 | 작가 추천 도서 | `/books`, `/books/:id`. 공개 도서, 대표 사진, 작가의 추천문. 상세 진입·직접 접속 확인 |
| 4 | 모임·행사 | `/news`, `/news/:slug`. 진행·종료·대관 분류, 날짜 표시 및 상세 |
| 5 | 월간 캘린더 | `/calendar`, 공개 일정 상세 `/calendar/events/:id`. 월 이동, 운영·휴무·일정 구분 |
| 6 | 운영 프로그램 | `/programs`는 `/programs/bookstore`로 HTTP 308 이동. 하위 경로 `bookstore`, `book-club`, `writing-class`, `space-rental`, `event-planning` |
| 7 | 방문 안내 | `/visit`. 주소, 운영시간, 주차, 지도·연락 동선. 운영시간 변경 시 캘린더와 함께 검증 |

**확정.** Key Visual 카피는 아래 순서의 3줄이다. CI와 구분선, 카피를 한 그룹으로 올린 시안이 2026-11-17 양쪽에 반영됐다. 소스의 PC·모바일 위치는 각각 `calc(clamp(50px,10.5svh,150px) + clamp(16px,2svh,24px))`, `calc(clamp(72px,13svh,140px) + 16px)`다. 현재 기준 파일은 `lib/home-template.html`이다.

> 에세이 작가 유미경이 제주 이호테우에 만든  
> 누구나 책을 읽고 글을 쓸 수 있는  
> 책방이자 모임공간입니다.

**확인.** 홈 Key Visual은 사진 캐러셀이다. 히어로 CI는 `/assets/hero-original-CI.svg`, 메뉴 CI는 `/assets/ci-approved-two-line.svg`를 사용한다. 홈 응답에서 데스크톱용과 모바일 세로용 WebP 첫 사진 preload를 확인했다. 좌우 화살표와 모션 감소 설정이 소스에 있다. 사진이나 크롭을 바꾸면 첫 화면, 세로·가로 크롭, 글자 대비, 로딩 전후 레이아웃, 키보드 조작을 함께 점검한다.

**확정.** 승인된 홈·서브페이지 표시 문구는 `작가작업실 JEJU`를 사용한다. 모임·행사 제목과 본문은 DB 원문을 유지한다. HTML 출력에는 기존 escaping 및 리치 텍스트 정제를 유지한다. 사이트 HTML에는 SEO 제목과 일부 대체텍스트 등 `작가작업실 제주`도 남아 있으므로 전역 치환 전에 해당 문구의 용도를 확인한다. [운영 반영 커밋](https://github.com/jeong1pumho/jakgastudio/commit/d8ff40b91d8bd56b2d96e020d30d762861c6c5e9)

**확인 및 미확인.** README는 임시 KV 사진과 운영 공개 이미지 사용을 설명한다. 이후 커밋·현재 HTML에는 새 캐러셀과 승인 카피가 있다. 현재 HTML과 커밋은 구현 기준으로 삼고, 과거 승인 이미지 원본과 모든 섹션의 시안 일치 여부는 미확인으로 둔다. 화면 수정 전 테스트 사이트에서 PC·모바일 기준 캡처를 확보하고, 변경 후 메뉴·텍스트 줄바꿈·이미지 크롭·CTA·여백을 비교한다. 대관 페이지는 CTA 아래 구분선 여백 조정 이력이 있다. [테스트 변경](https://github.com/jeong1pumho/jakgastudio-temp/commit/7f2405fa9d91b22f0f5318bca8b91f3cde89dea8)

## 4. 구현 위치와 기능 명세

**확인.** Vercel Functions 및 서버 생성 HTML 구조다. `vercel.json`이 화면 경로를 함수로 연결하고 `api/news.js`가 홈·도서 요청을 `lib/handlers/`로 보낸다. `lib/home-template.html`, `lib/home-content.js`는 공통 홈·메뉴 구현에 관여한다. `api/archive-page.js`, `api/news-page.js`, `api/calendar-page.js`는 개별 공개 페이지 함수다. `lib/core.js`는 Supabase 요청과 관리자 권한 검증을 처리한다. `package.json`의 확인된 의존성은 `korean-lunar-calendar@0.4.0`이다. 루트 `index.html`은 삭제 이력이 있어 홈 수정은 rewrite부터 따라간다. `.vercelignore`가 문서·Supabase 개발 디렉터리 등을 배포에서 제외한다. [테스트 경로 설정](https://github.com/jeong1pumho/jakgastudio-temp/blob/main/vercel.json)

### 모임·행사와 NEWS

- **본문 줄 간격.** 사용자 상세 본문과 관리자 등록·수정 입력란, 미리보기는 모두 `line-height: 1.95`를 사용한다. 일반 문단(`p`)의 위아래 여백은 `0`으로 두고, 작성자가 입력한 빈 줄은 보존한다. 관리자 입력란과 미리보기의 기본 글자 크기는 `16px`이다.
- **검수 방법.** ‘모임·일정·인원’을 연속 세 줄로 입력하고 빈 줄 하나 뒤에 맺음말을 쓴다. 편집·미리보기·상세에서 세 항목의 간격과 맺음말 앞 빈 줄을 확인한다. 기존 `div` 형식의 [등대작가 모임](https://www.jakgastudio.com/news/news-1794787399051)과 `p` 형식의 [스케치북 독서모임](https://www.jakgastudio.com/news/news-1795141696424)을 비교한다. [2026-11-20 운영 반영 기록](https://github.com/jeong1pumho/jakgastudio/commit/0d0e8bc15a097af85a98fc600e24a8d7de097c83)

- **확인.** 화면 명칭은 ‘모임·행사’, 코드와 데이터 모델은 NEWS 이름을 사용한다. 관리자 `/admin/news.html`, 공개 목록 `/news`, 상세 `/news/:slug`. 제목·본문·사진·대표 사진·순서·상태·날짜·유형·운영 주체를 편집한다. 유형은 독서·글쓰기·주제모임·특별행사, 주체는 자체 운영·대관이다. 대관은 외부 주최자 필수다. 기존 자료에는 미분류 값이 있을 수 있다.
- 날짜 모드는 표시 없음·하루·기간이다. 행사 날짜가 지나도 `ongoing`에서 `ended`로 자동 전환되지 않는다. 관리자가 모임·행사 편집 화면에서 상태를 변경한다. 캘린더 일정은 `schedule_events`에서 따로 관리한다. NEWS 표시일을 수정할 때 캘린더도 바뀌어야 한다는 전제가 있으면 해당 연결을 먼저 확인한다.
- 진행 중은 `sort_order` 내림차순, 발행일 내림차순(빈 값 뒤), 생성일 내림차순이다. 종료는 발행된 종료 글을 모아 `sort_order`, 유효 종료일(`event_date_end || event_date_start`), 발행일, 생성일, ID 순으로 내림차순 정렬한 뒤 페이지를 나눈다. `/api/news`의 대관 사례는 이 종료 목록 중 `operator_type=rental`이다. 공간 대관 프로그램(`/programs/space-rental`)의 사례는 `operator_type=rental`, 발행일이 있는 `ongoing`·`ended` 글을 모두 조회하고 행사 시작일 최신순(날짜 없는 글은 뒤)으로 표시한다. 공개 API 기본 응답은 진행·종료 각각 최대 6개와 대관 최대 3개다. 대표 사진은 지정 인덱스, 없으면 첫 사진을 쓴다. [정렬 코드](https://github.com/jeong1pumho/jakgastudio-temp/blob/main/lib/news-order.js)
- 검수 시 순서 동점, 날짜 없음·하루·기간, 페이지 경계, 대관 주최자, 기존 미분류 글, 이미지 표시 및 DB 원문 렌더링을 확인한다.

### 캘린더

- **행사 상세와 메모 공개 범위.** 등록 구분이 모임(`meeting`) 또는 특별일정(`custom`)인 일정에는 날짜별 화면에 ‘행사 상세 보기’를 표시한다. 운영 주체는 판정 조건에 포함하지 않는다. 공개 기간 안의 개별 상세에서만 해당 일정의 메모를 읽고 허용된 서식으로 출력한다. 메모가 비어 있으면 본문 영역을 생략한다. 월별 일정 API에는 메모를 포함하지 않는다.
- **상세 하단.** 제목 아래에 일시·구분·장소를 요약하고, 메모와 안내문 아래에 ‘모임·행사 확인’ 링크 하나를 두어 `/news`로 연결한다. 관리자 입력란에는 구분별 공개 여부를 안내한다. [운영 반영 기록](https://github.com/jeong1pumho/jakgastudio/commit/1ad95369ac99ca6cafd9a18ad49709ed978693da)

- **일정 제목 표기.** 월간 캘린더의 날짜별 일정, 개별 일정 상세, 홈 주간 일정 및 예정 일정에서 제목을 표시할 때 `schedule_events.title`의 한글·영문 표기를 그대로 사용한다. `작가작업실 JEJU`와 `작가작업실 제주`를 자동 치환하지 않는다. HTML 출력 시에는 기존 이스케이프 처리를 유지한다.
- **검수 사례.** 운영 캘린더의 2026-12-10 10:00~12:00 일정에서 관리자와 공개 화면의 제목이 모두 `낭독클럽 | 작가작업실 JEJU`인지 확인한다. [운영 일정 상세](https://www.jakgastudio.com/calendar/events/6fde6fb2-3d37-4eaf-a7e4-ca51041aae43) · [운영 반영 기록](https://github.com/jeong1pumho/jakgastudio/commit/8ddbb77d74b0c9efc96b4c7a305b02f48f936cdc)

- **확인.** 영업 기준 09:00–17:00, 점심 제외 12:00–13:00, 일·월 정기휴무. 정기휴무·공휴일을 제외한 화~토 자유방문은 09:00–12:00와 13:00–17:00에서 등록 일정과 겹친 시간을 뺀 결과다. 하루 영업시간 전체가 비면 점심 제외 안내와 함께 09:00–17:00로 묶어 표시한다. 정기휴무·공휴일에는 등록된 특별 일정이 표시되고 자유방문 시간은 생성되지 않는다. 저녁 일정도 등록할 수 있다.
- 캘린더 기준일은 `Asia/Seoul`. 공개 월 조회는 2026-10 개점 월부터 다음 달까지다. 2026-10-22 이전 날짜에는 일정 저장이 금지된다. 개점 표식은 ‘작가작업실 JEJU OPEN’, 2026-10-22 종일이다. 관리자 일반 일정 쓰기 정책에서 개점 표식을 보호한다.
- 일정 유형은 `meeting`, `rental`, `custom`, `closure`, `opening`. `custom`은 표시명을 받는다. 임시 휴무는 00:00–24:00 종일이다. DB는 `[시작, 종료)` 범위가 겹치는 일정을 막으므로 13–15시와 15–17시는 붙여 등록할 수 있다. 14–16시와 15–17시는 충돌한다.
- `/api/schedule?month=YYYY-MM`은 공개 일정 컬럼과 공휴일을 반환한다. 공개 상세 및 구조화 데이터는 모임·특별일정(`meeting,custom`)으로 범위를 제한한다. DB RLS에서 `schedule_events`의 SELECT는 공개이므로 세부 API의 화면 필터를 비밀 유지 수단으로 사용하지 않는다. 민감한 내용은 `schedule_event_memos`에 입력한다.
- 관리자 `/admin/schedule.html` → `/api/schedule-admin` → `save_schedule_event`를 추적한다. 일정과 메모 저장, 같은 시간 재시도, 두 관리자 동시 입력, 종일 휴무 충돌 및 비로그인 상태에서 모임·특별일정 메모 표시와 대관 메모 차단을 확인한다. [일정 API](https://github.com/jeong1pumho/jakgastudio-temp/blob/main/api/schedule.js)

### 관리자와 문자 알림

- **확인.** `/admin/` 메뉴는 Home, Key Visual, 작가 추천 도서, 모임·행사, 월간 캘린더, 공간 소개, 문자 알림이다. Supabase Auth 로그인 후 `news_admins.user_id`에 속하는 사용자를 서버에서 확인한다. 세션과 갱신 토큰은 관리자 탭의 `sessionStorage`를 사용한다. 관리자 권한을 부여할 때 해당 환경의 Auth 사용자 ID와 관리자 매핑을 확인한다. 권한 변경은 환경별 승인 범위에서 진행한다. [권한 코드](https://github.com/jeong1pumho/jakgastudio-temp/blob/main/lib/core.js)
- **확인.** `/admin/sms.html`은 문자 등록자·수신거부, 동의 근거가 있는 현장 수동 등록, 단체 문자, 발송 이력, 신규 신청 Gmail 알림 UI를 제공한다. 양쪽 Supabase에 `studio-sms-news`가 ACTIVE다. 테스트 v15, 운영 v10이며 게이트웨이 `verify_jwt=false`다. 따라서 비공개 동작의 함수 내부 인증·관리자 검증을 변경 시 함께 읽는다. 운영에는 과거 1회성 미디어 이전 함수 `one-time-news-media-import-20261104`도 ACTIVE로 남아 있다.
- 웹 경로는 OTP 인증과 개인정보·마케팅 동의 기록을 사용한다. 현장 경로는 서면 또는 명시적 구두 동의 기록과 등록 관리자를 남긴다. 수신거부와 캠페인 중복 기록을 유지한다. 테스트 발송은 승인된 테스트 수신처로만 확인하고, 실패 과정에서 사업자 발송 이력을 확인한 뒤 재시도한다. 공급자 수락과 실제 도달은 구분한다.
- **미확인.** 관리자 로그인 후 화면, 실제 발송·도달, 080 수신거부 연동, Gmail 재시도 스케줄, 운영·테스트 발신처 분리는 이번 조사에서 실행 검증하지 않았다.

## 5. Supabase 데이터 계약

**확인.** 두 프로젝트는 `ap-northeast-1`, Postgres 17, 조회 당시 ACTIVE_HEALTHY였다. `public` 11개 테이블의 이름·컬럼·RLS 활성화가 일치했다. 마이그레이션 이력은 각각 테스트 26개, 운영 16개다. **동일한 이름·컬럼으로 전체 스키마가 같다고 판단하지 않는다.** 예를 들어 NEWS 날짜 정합성 CHECK는 테스트에만 확인됐다. 운영에서 스키마 변경을 검토할 때는 양쪽 제약, 기본값, 인덱스, 테이블·컬럼 권한, 정책 및 RPC를 별도로 비교한다.

| 테이블 | 키와 핵심 제약 | 데이터 및 읽기 기준 |
|---|---|---|
| `news_posts` | UUID PK, slug 고유, 제목 2~150자, 이미지 최대 20장, `cover_index` 0~19 | ongoing/ended. 유형·주체 nullable. 대관 외부 주최자 1~160자 필수. 테스트 CHECK는 종료일 단독 입력 및 시작일보다 이른 종료일을 제한 |
| `news_admins` | `user_id` PK, `auth.users.id` FK·삭제 연동 | 로그인 사용자의 관리자 매핑 조회에 사용 |
| `schedule_events` | UUID PK, 범위 중복 방지 GiST, 시작보다 종료가 늦어야 함 | 일정 날짜·유형·제목·시간. 제목 1~150자, 사용자 정의 유형 라벨 1~50자 |
| `schedule_event_memos` | `event_id` PK·FK, 일정 삭제 시 함께 삭제 | 최대 30,000자. 원본 테이블은 관리자 전용이며, `public_calendar_event_memo(uuid)`가 공개 기간 내 모임·특별일정의 해당 메모만 반환 |
| `space_gallery` | `id=1`, `version>=1`, 이미지 배열 최대 80 | 공개 갤러리 설정. 순서·파일 참조 유지 |
| `recommended_books` | UUID PK, 제목 1~150자, 추천문 1~5,000자, 이미지 1~20장, `photo_edits` 객체 | `published=true`가 공개, 관리자 편집·정렬 |
| `key_visual` | `id=1`, `revision>0`, 슬라이드 1~10개, 원본 배열 최대 10 | 캐러셀 렌더링 설정. 보관용 원본은 비공개 버킷. 원본 컬럼 공개 여부는 컬럼 권한도 함께 점검 |
| `sms_subscribers` | UUID PK, 휴대전화 번호 고유 | 가입·거부 상태, 웹 OTP/수동 동의 구분 및 이력. 개인정보 |
| `sms_challenges` | UUID PK, 가입/거부용, 시도 횟수 0~6 | OTP 해시·솔트, 만료·소비·발송 시간, IP 해시. 비공개 |
| `sms_campaigns` | UUID PK, `(event_id,message_digest)` 고유, NEWS 삭제 시 FK NULL | preparing/accepted/failed, 공급자 그룹 ID 및 수신 대상 수. 비공개 |
| `sms_signup_email_outbox` | UUID PK, `(subscriber_id,subscribed_at)` 고유, 가입자 삭제 시 함께 삭제 | queued/processing/sent/failed, 시도 횟수 0~20, 공급자 상태. 비공개 |

공개·관리자 RLS 정책을 조회했다. NEWS에는 상태 기반 공개 읽기, 캘린더 일정에는 공개 읽기, 추천 도서에는 발행 조건 읽기, 갤러리·KV에는 공개 읽기 정책이 있다. 절대 공개하면 안 되는 내용은 공개 테이블에 입력하기 전에 열·행 권한과 실제 응답을 확인한다. SMS 4개 테이블에는 조사한 `pg_policies` 결과에서 사용자 정책이 나타나지 않았다. 실제 서버 권한과 함수 동작은 해당 배포 코드로 확인한다.

| Storage 버킷 | 양쪽 공개 상태 | 객체 최대 크기 / 타입 |
|---|---|---|
| `news-media`, `space-media` | 공개 | 10 MiB / JPEG, PNG, WebP, AVIF |
| `key-visual` | 공개 | 50 MiB / WebP, PNG |
| `key-visual-originals` | 비공개 | 30 MiB / JPEG, PNG, WebP, AVIF |
| `book-media` | 공개 | 4 MiB / JPEG, PNG, WebP |
| `book-originals` | 비공개 | 25 MiB / JPEG, PNG, WebP |

버킷 제한과 관리자 UI 업로드 제한은 서로 다를 수 있다. 업로드→DB 참조 저장→공개 화면 확인 순으로 검증한다. 삭제 전 NEWS 본문·대표사진, 도서와 편집 정보, 갤러리, KV 슬라이드 및 양쪽 환경의 외부 URL 참조를 확인한다.

## 6. 환경변수와 외부 서비스

**확인한 이름과 범위만 기록한다.** 값과 비밀은 Vercel 프로젝트 또는 해당 Supabase Edge Function의 비밀 설정에서 다룬다. 키·토큰·연결 문자열은 공개 GitHub, 채팅, 로그, 브라우저, 예시 파일에 기록하지 않는다. Supabase publishable key는 공개 클라이언트에서 쓸 수 있는 유형이며 RLS가 접근을 제한한다. 관리자 config 응답에는 publishable key 또는 legacy anon key만 허용한다.

| 이름 | 용도 | Vercel 조회 범위 |
|---|---|---|
| `SUPABASE_URL` | Supabase 프로젝트 API URL | 테스트 Production·Preview·Development / 운영 Production·Preview |
| `SUPABASE_PUBLISHABLE_KEY` | 공개 데이터 및 사용자 인증에 쓰는 공개 키 | 테스트 Production·Preview·Development / 운영 Production·Preview |
| `SUPABASE_ANON_KEY` | `lib/core.js`의 레거시 대체 변수명 | 실제 등록 여부 미확인 |
| `GA4_TEST_CLIENT_EMAIL`, `GA4_TEST_PRIVATE_KEY`, `GA4_TEST_MEASUREMENT_ID`, `GA4_TEST_PROPERTY_ID` | 테스트 방문 분석 설정 | 테스트 프로젝트 Production |
| `GA4_MEASUREMENT_ID` | 운영 사이트 방문 이벤트 수집용 측정 ID `G-YVEQSZK3DL` | 운영 프로젝트 Production만 (확인) |
| `GA4_PROPERTY_ID` | 운영 GA4 보고서 속성 ID `565255506` | 운영 프로젝트 Production만 (확인) |
| `GA4_CLIENT_EMAIL` | 운영 GA4 Data API 읽기 전용 서비스 계정 | 운영 프로젝트 Production만 (확인) |
| `GA4_PRIVATE_KEY` | 위 서비스 계정의 서명용 비공개 키, Secret/Sensitive로 보관 | 운영 프로젝트 Production만 (등록 상태 확인, 값 검증 불가) |
| Edge Function 비밀 및 서버 전용 변수 | 문자 발송, 가입 메일, 인증·서버 접근 | 전체 설정 여부 별도 확인. 아래 조사 부록의 코드 참조 이름과 대조 |

**중요.** 운영 Vercel에서 위 두 Supabase 변수의 Preview 범위가 확인됐다. Preview 값과 운영 Production 값이 같은 ref인지 확인하기 전에는 **운영 저장소 Preview도 운영 데이터에 연결될 수 있다고 보고** 테스트 쓰기, 관리자 저장, 문자 발송을 하지 않는다. 공개 HTML에서 운영과 테스트 기본 도메인의 연결 ref는 각각 확인했다. GA4 집계 제외 UI는 해당 브라우저·사이트에만 적용된다.

### 운영 GA4 연결 및 관리자 대시보드 계약 (2026-11-20 추가)

**확정된 구분.** 운영 GA4 속성은 '작가작업실 JEJU'이며 운영 웹 스트림 주소는 `https://www.jakgastudio.com`, 측정 ID는 `G-YVEQSZK3DL`, 속성 ID는 `565255506`이다. 기존 테스트 GA4의 속성 ID는 `564317461`, 측정 ID는 `G-8EJM3H9W2K`이다. **두 측정 ID를 교차 사용하지 않는다.** 운영 GA4 측정 ID와 운영 조회 인증정보는 운영 Vercel `jakgastudio` 프로젝트의 Production 환경에만 등록했다. 테스트 Vercel 환경변수는 이번 작업에서 변경하지 않았다.

**운영 배포 확인.** 사용자 요청에 따라 운영 환경변수 4개를 등록하고 동일 소스 커밋 `1ad95369ac99ca6cafd9a18ad49709ed978693da`를 재배포했다. 최종 배포는 `dpl_9jdkppNnSaN2tQEDwMWeRxnxRcnM`로 READY 및 `www.jakgastudio.com`, `jakgastudio.com`, `jakgastudio.vercel.app` 연결을 확인했다. 홈페이지 소스, GitHub 커밋, Supabase DB·Storage는 변경하지 않았다. 환경변수만 바꾸더라도 실행 배포에 반영하려면 새 배포가 필요하다.

**수집 구현 확인.** 운영 `assets/site-analytics.js`가 일반 공개 페이지에서 `/api/admin?action=analytics`로 운영 측정 ID를 조회한 뒤 `gtag.js`를 비동기로 불러온다. `page_view`를 발송하도록 구현되어 있고, 지도·길찾기, 전화, 이메일, 문자 신청 열기·완료, 캘린더 날짜 선택, 행사·프로그램·도서 상세 클릭 이벤트를 추적 대상으로 정의한다. `location.hostname==='www.jakgastudio.com'`일 때만 실행하고 `/admin`, `/api`는 제외하므로 테스트와 운영이 분리된다. `jakgastudio.com`에서 접속 시 www 도메인으로 308 이동하도록 등록돼 있다. `lib/home-template.html`, 모임·행사 목록/상세 및 캘린더 상세의 서버 HTML에 스크립트 포함을 확인했다. **이벤트 핸들러 존재는 Google 서버 수신 성공의 증거가 아니다.** 일부 이벤트(예: 문자 신청 완료)는 호출부·실사용 흐름에서 별도 검증한다.

**관리자 보고서 구현 확인.** `api/admin.js?action=analytics&report=1` → `lib/site-analytics.js`가 관리자 인증 뒤 Google Analytics Data API `batchRunReports`를 호출한다. 환경변수 `GA4_MEASUREMENT_ID`, `GA4_PROPERTY_ID`, `GA4_CLIENT_EMAIL`, `GA4_PRIVATE_KEY`를 사용한다. `hostName`이 `www.jakgastudio.com`인 데이터로 제한한다. `assets/admin-dashboard.js`는 세션, 페이지 조회, 방문당 평균 참여 시간, 주요 유입 경로와 랜딩 페이지, 지정 클릭 이벤트를 표시한다. **조회 기간은 `7daysAgo`~`yesterday`로 오늘 데이터가 제외된다.** 운영 관리자 화면에서 '방문 0회 · 조회 0회'와 세부 0회 항목이 표시돼 이전 '보고서 연결 대기' 상태가 사라진 것을 사용자 캡처로 확인했다. 이는 보고서 처리 경로가 응답했다는 근거이나 실제 방문 이벤트의 수신을 입증하지 않는다.

**계정 및 키 관리.** 사용자가 운영 GA4 속성 액세스 관리 화면에서 `ga4-test-dashboard@spheric-gasket-514705-b3.iam.gserviceaccount.com`의 뷰어 역할을 확인했다. Google Cloud 프로젝트 `spheric-gasket-514705-b3`의 동일 서비스 계정에 Active 키 3개가 표시된 것도 확인했다. 운영 `GA4_PRIVATE_KEY`는 Vercel에서 Sensitive로 등록되었고 값 자체는 채팅, 공개 저장소 및 문서에 기록하지 않았다. **키 내용, 정확한 키-계정 대응, 만료·폐기 계획은 별도 점검 대상**이다. 필요시 비공개 관리 채널에서 키를 교체한다. 이미 등록된 키가 있다면 무분별하게 추가 생성하지 않는다.

**마지막 검증 상태 (미확인).** 공개 운영 홈과 `/news`는 HTTP 200, `/api/admin?action=analytics`는 `G-YVEQSZK3DL` 응답, `/assets/site-analytics.js`도 HTTP 200을 반환했다. 최근 30분 운영 Vercel 오류 로그 조회에는 오류가 관측되지 않았다. 그러나 권한 있는 관리자 토큰으로 API 보고서의 원본 응답을 직접 재조회하지 못했고, 독립적인 브라우저 측 `gtag.js` 요청·Google 수집 엔드포인트 전송 성공·GA4 실시간 `page_view` 수신은 확인하지 않았다. 따라서 **'설치·배포 및 관리자 숫자 렌더링 확인'과 '실제 방문 수집 완료'를 별도 상태로 관리**한다. 실방문이 발생한 뒤 GA4 실시간 또는 DebugView에서 `page_view`와 호스트를 확인하고, 다음 날 관리자 7일 카드와 대조한다. 0회가 지속될 때는 브라우저 추적 차단/동의, 네트워크 요청, GA4 수집 설정, 호스트 필터 및 타임존을 순서대로 점검한다. 검증을 위해 사용자 브라우저의 집계 제외를 해제하지 않는다.

**개인 브라우저 집계 제외.** 현재 운영 관리자 화면의 '이 브라우저: 방문 집계 제외 중'은 해당 브라우저의 `localStorage` 키 `jakgastudio.analytics-excluded.v1`에 저장된 선택이다. 기기·브라우저별로 각각 작동하며 사이트 데이터 삭제 시 설정이 초기화될 수 있다. 서버 관리자 인증 여부와 방문 집계 제외 여부는 별개다.

## 7. 개발·배포 절차

1. **기준선 기록.** 대상 환경, 현재 도메인과 배포 ID, 작업할 SHA, Supabase ref, 영향을 받는 화면·DB·Storage·외부 발송을 기록한다. `main` push의 Vercel 자동 배포 영향을 확인한다. 운영과 테스트는 독립 저장소다. 전체 브랜치 덮어쓰기보다 승인한 변경을 운영 기준 커밋에 적용하고 차이를 검토한다.
2. **로컬 확인.** 대상 저장소의 승인된 기준 커밋을 읽고 Node 24.x와 lockfile을 확인한다. 테스트 자격으로 Vercel 런타임 재현이 필요하면 해당 프로젝트에 연결한 `vercel dev` 또는 승인된 테스트 URL을 쓴다. `vercel.json` rewrite 때문에 단순 HTML 열기로는 홈·API 전체가 재현되지 않는다. `package.json`에 별도 빌드·테스트 스크립트가 정의됐는지 확인한다.
3. **테스트 반영.** 저장소 쓰기 승인을 확보한 범위에서, 또는 이미 승인된 테스트 배포 방식으로 반영한다. GitHub SHA → Vercel READY → 테스트 URL → 테스트 ref를 확인한다. 테스트 DB 변경은 마이그레이션 diff와 적용 순서를 기록한다. 영향을 받는 UI에서 성공·실패·권한·이전 데이터 케이스를 검증한다.
4. **운영 승인 준비.** 테스트 URL, 변경 스크린샷, 반영할 Git diff·SHA, 운영 Vercel/Supabase 대상, DB·Storage 변경 여부, 백업, 복구 후보 배포를 제시한다. 운영 코드와 DB 양쪽이 바뀌면 이전 코드·신규 스키마 및 신규 코드·이전 스키마의 호환 범위를 확인한다. 구조 변경은 가능하면 추가 → 전환 → 기존 구조 정리 순으로 분리한다.
5. **승인된 운영 반영·검증.** 승인 범위 그대로 배포한다. 공개 도메인의 응답, canonical, 관리자 인증 경로, 오류 로그, DB 연결을 확인한다. 운영에 입력이 필요한 검수는 별도 승인된 데이터·수신처로 진행한다. 배포와 실제 공개 상태, DB·알림 확인 결과를 배포 기록에 적는다.
6. **배포·자산 정리.** 검수된 정상 배포 중 복구 후보를 지정하고, 실제 복구 자격과 접근을 확인한 뒤 정리한다. 삭제 후보의 URL·외부 참조와 복구 후보를 기록한다. Git 자산 검색만으로 DB·CMS의 참조를 확인할 수 없으므로 공개 버킷이나 이전 경로도 조사한다.

## 8. 백업과 장애 복구

아래는 **제안 절차**다. 현재 백업 정책, 보관 기간, 백업일과 PITR은 각 Supabase 대시보드에서 확인한다. 과거 마이그레이션의 이름에 backup·snapshot이 있어도 전체 복원이 가능한 백업 사본인지 별도로 검증한다.

| 백업 대상 | 확보·검증 방법 |
|---|---|
| 웹 코드·배포 | Git SHA, Vercel 배포 ID와 정상 동작 확인 시각. 최근 목록의 rollback candidate 여부를 다시 확인 |
| DB | Supabase Dashboard의 Database → Backups에서 복구 시점과 사용 가능 여부 확인. 필요한 경우 해당 환경 전용 연결로 Supabase CLI의 논리 백업을 준비하고 스키마·데이터·역할·Auth 범위를 체크 |
| Storage | DB 복구와 별도로 6개 버킷 객체를 보존. 객체 경로·크기·해시 또는 검증 가능한 목록을 확보하고 공개/비공개 정책을 기록 |
| 연결 설정 | Vercel 도메인·프로젝트 연결·환경변수 이름/범위, Auth redirect 설정, Edge Function 버전·코드·비밀 이름, 알림 스케줄을 기록. 값은 접근 제한된 보관소로 관리 |
| 복원 가능성 | 독립 복구 환경에서 웹 렌더링, 이미지·원본 권한, 일정·메모, 구독/수신거부 상태를 검증. 알림 전송은 격리. 백업 접근 담당자와 목표 복구시간·허용 유실량은 별도 확인 |

- **웹 배포만 문제일 때.** 장애 시작, 현재 배포, 마지막 정상 배포를 기록한다. DB와 화면 간 호환성을 확인한 후 승인된 환경에서 Vercel Instant Rollback 또는 정상 코드 재배포를 선택한다. 공개 도메인·API·오류 로그 검증 후 어떤 SHA가 실제 서비스 중인지 기록한다. 웹 롤백 다음에 자동 도메인 할당과 Git 배포 상태를 확인한다. [Vercel Instant Rollback](https://vercel.com/docs/deployments/instant-rollback)
- **DB나 관리자 데이터가 문제일 때.** 발생 시각과 쓰기 영향을 파악한다. 새 쓰기와 알림 전송 제어가 필요하면 승인 범위에 넣는다. 먼저 현재 상태를 별도로 보존하고, 선택한 복구 시점을 격리 환경에서 검증한다. 정상 쓰기를 최대한 보존할 수 있으면 필요한 행·스키마의 복구를 우선 검토한다. 전체 복원이 필요하면 사고 후 데이터 유실 범위와 전환 계획을 제시한 뒤 승인받는다. Auth·Storage·Edge 설정과 애플리케이션 호환성을 확인한다. [Supabase Database Backups](https://supabase.com/docs/guides/platform/backups)
- **파일 유실 또는 오발송 위험.** DB 백업에는 Storage 파일 본체가 포함되지 않는다. 객체를 먼저 확인해 원래 경로에 복구하고 DB의 참조와 권한을 검증한다. 구독 데이터 복원 시 복구 시점 이후 수신거부·발송 이력을 공급자 기록과 대조하고, outbox 재개 전에 중복 발송 가능성을 확인한다. [Supabase Database Backups](https://supabase.com/docs/guides/platform/backups)

## 9. 회귀 검수 및 문서 갱신

| 변경 범위 | 통과 조건 |
|---|---|
| 홈·공통 메뉴 | 메뉴 7개와 경로 정상. PC·모바일에서 KV 카피 3줄, CI 간격, 첫 사진 및 메뉴 조작 정상 |
| NEWS·추천 도서 | 상태·정렬·페이지 경계·대표 사진·직접 상세 URL · 기존 데이터 공개 여부 확인 |
| 캘린더 | 개점 전, 다음 달과 그 이후 경계, 일·월, 공휴일, 종일 휴무, 인접·중복 일정, 구분별 메모 공개 범위, 상세 하단 `/news` 링크 확인 |
| 관리자 | 로그아웃과 잘못된 세션, 비관리자 제한, 관리자 갱신, 업로드 후 저장·재조회 확인 |
| 알림 | 테스트 가입·해지·수동등록, 잘못된 OTP, 만료·재사용, 거부 대상 제외, 발송 재시도·중복, 실제 발송 대상 제한 확인 |
| SEO | 운영 www canonical, 운영 robots/sitemap, 테스트 noindex, 관리자 제외, 공유 미리보기 확인 |
| GA4 분석 | 운영·테스트 측정 ID 분리, 운영 www에서만 추적, 관리자·API 경로 제외, 개인 브라우저 집계 제외, 운영 환경변수 범위, 보고서 인증, 실시간 page_view 수신, 다음 날 7일 카드 대조 |
| 격리 | 테스트에서 실행한 변경은 테스트 ref/버킷/계정/수신처에 기록. 운영 공개 페이지는 읽기 검증으로 상태 비교 |

### 복원한 주요 결정

| 이력 | 근거 및 상태 |
|---|---|
| 2026-10-22 개점 표식 | 두 DB 제약 및 관리자 정책으로 확인 |
| 캘린더와 비공개 메모, 공간 CMS, 모임·행사 분류, SMS·가입 이메일 | 두 프로젝트 마이그레이션과 스키마에서 확인. 개별 배포 시 최종 승인 원문은 일부 미확인 |
| 2026-11-16 NEWS 정렬 | 종료일 적용 후 페이지 나누기 및 수동 우선순위 반영을 코드 확인. [테스트 커밋](https://github.com/jeong1pumho/jakgastudio-temp/commit/944d05aadff26bc016dd792d2a03043f7a141ddd) |
| 2026-11-17 JEJU 표기·DB 원문 표시 | 관련 대화의 사용자 승인과 운영 커밋 확인 |
| 2026-11-17 배포 파일 정리 | 양쪽 저장소에 개발 파일 배포 제외 및 미사용 미디어 정리 커밋 확인. 과거 삭제량은 대화의 완료 보고 기준 |
| 2026-11-17 Key Visual 카피와 위치 | ‘테스트사이트 수정 완료’ 대화의 사용자 요청·양쪽 반영 승인, 양쪽 커밋, 양쪽 홈페이지 HTML에서 확인 |
| 추후 확인 | 백업·PITR·복구 담당자, 운영 Preview DB ref, 발신처 분리, 승인 디자인 원본과 전체 의사결정 이력, Git 브랜치 보호/승인 설정 |

조사에 사용한 이전 대화는 [개발기준 문서 작성](chatgpt-conversation://6afbe065-bba8-83ee-b364-41c2ae171a9c)과 [테스트사이트 수정 완료](chatgpt-conversation://6afba017-02e0-83e8-9711-8c4b246af080)다. 첨부 이미지 원본은 확보되지 않았다. 현재 소스, 공개 HTML, DB 메타데이터로 확인한 내용은 구현의 기준선이다. 디자인 승인과 실제 관리자 저장·발송 등은 위 확인 범위를 기준으로 판단한다.

**v1.5 개정 기록 (2026-11-20 KST).** 사용자 요청으로 운영 GA4 연결의 실제 환경변수 4개, 운영 속성 ID·측정 ID, 서비스 계정 뷰어 확인, 운영 배포 `dpl_9jdkppNnSaN2tQEDwMWeRxnxRcnM`(커밋 `1ad95369`), 공개 API/스크립트 응답, 관리자 화면 0회 출력과 남은 실수집 미검증을 기록했다. DB 마이그레이션 없음. 환경변수 롤백 시 기존 정상 배포만 복원하면 신규 환경변수는 유지될 수 있으므로 변수 삭제·변경과 재배포를 함께 검토한다.

새 변경을 반영한 뒤 이 문서에 변경 일시(KST), 승인 근거, 대상 환경, Git SHA, 배포 ID, DB 마이그레이션, 검수 및 복구 지점을 갱신한다. 저장소에 기준서를 등록할 때도 대상 저장소·브랜치와 배포 영향을 승인받는다.

## 부록. 조회 결과와 스키마 참조

### 공개 경로 조회 결과

2026-11-17 양쪽 도메인에서 `/`, `/space`, `/books`, `/news`, `/calendar`, `/visit`, `/robots.txt`, `/sitemap.xml`이 HTTP 200이었다. `/programs` 요청은 소스상 `/programs/bookstore`로 HTTP 308 이동하며, 조회에서 관측한 HTTP 200은 이동 후 책방 페이지의 응답이다. 홈 HTML에서 메뉴 7개와 KV 카피 3줄을 확인했다. 운영 홈 robots 메타는 `index, follow`, 테스트 홈은 `noindex, follow`였다. 테스트 `vercel.json`의 전역 헤더에는 `noindex, nofollow`가 지정돼 있다. 운영 robots.txt는 운영 sitemap을 안내하며 관리자·API 수집 제외가 설정돼 있다. 이 검증 범위는 HTTP 응답과 문서 내용이다.

### Edge Function 코드의 환경변수 이름

양쪽 `studio-sms-news` 배포 소스에 다음 이름이 확인됐다. 아래는 코드가 읽는 변수이며 환경별 설정값과 등록 여부는 별도로 확인한다.

| 이름 | 사용 목적 |
|---|---|
| `SUPABASE_URL` | 해당 Edge Function의 Supabase URL |
| `SUPABASE_SECRET_KEYS`, `SUPABASE_SERVICE_ROLE_KEY` | 서버 전용 DB 접근. 공개 응답이나 클라이언트에 포함 금지 |
| `SOLAPI_SENDER`, `SOLAPI_API_KEY`, `SOLAPI_API_SECRET` | Solapi 발신번호 및 API 인증 |
| `RESEND_API_KEY`, `RESEND_FROM_EMAIL` | 가입 알림 메일 발송 인증 및 발신자 |

### public 컬럼 목록

메타데이터로 조회한 컬럼과 자료형이다. 두 환경의 컬럼 목록은 일치했다. DDL 작성이나 복구에는 원본 마이그레이션과 현재 프로젝트의 스키마를 사용한다.

- **`public.news_posts`** `id` uuid, `slug` text, `title` text, `content` text, `images` jsonb, `cover_index` int, `status` text, `sort_order` int, `published_at` timestamptz, `created_at` timestamptz, `updated_at` timestamptz, `event_date_start` date, `event_date_end` date, `event_type` text, `operator_type` text, `external_organizer` text

- **`public.news_admins`** `user_id` uuid, `created_at` timestamptz

- **`public.schedule_events`** `id` uuid, `event_date` date, `category` text, `category_label` text, `title` text, `start_time` time, `end_time` time, `created_at` timestamptz, `updated_at` timestamptz

- **`public.schedule_event_memos`** `event_id` uuid, `memo` text, `updated_at` timestamptz

- **`public.space_gallery`** `id` smallint, `images` jsonb, `version` int, `updated_at` timestamptz

- **`public.sms_subscribers`** `id` uuid, `name` text, `phone` text, `status` text, `privacy_consented_at` timestamptz, `marketing_consented_at` timestamptz, `consent_version` text, `verified_at` timestamptz, `subscribed_at` timestamptz, `unsubscribed_at` timestamptz, `unsubscribe_source` text, `created_at` timestamptz, `updated_at` timestamptz, `entry_source` text, `consent_method` text, `manual_consent_date` date, `consent_evidence` text, `registered_by` uuid, `last_edited_by` uuid

- **`public.sms_challenges`** `id` uuid, `phone` text, `name` text, `purpose` text, `code_hash` text, `salt` text, `ip_hash` text, `attempts` int, `expires_at` timestamptz, `consumed_at` timestamptz, `sent_at` timestamptz, `privacy_agreed` bool, `marketing_agreed` bool, `created_at` timestamptz

- **`public.sms_campaigns`** `id` uuid, `event_id` uuid, `event_title` text, `message_body` text, `message_digest` text, `sender_number` text, `recipient_count` int, `blocked_count` int, `status` text, `solapi_group_ids` jsonb, `error_note` text, `created_by` uuid, `created_at` timestamptz, `completed_at` timestamptz

- **`public.sms_signup_email_outbox`** `id` uuid, `subscriber_id` uuid, `subscribed_at` timestamptz, `kind` text, `status` text, `attempts` int, `last_attempt_at` timestamptz, `sent_at` timestamptz, `provider_message_id` text, `last_error` text, `created_at` timestamptz, `updated_at` timestamptz

- **`public.recommended_books`** `id` uuid, `title` text, `recommendation` text, `images` jsonb, `sort_order` int, `published` bool, `created_at` timestamptz, `updated_at` timestamptz, `photo_edits` jsonb

- **`public.key_visual`** `id` int, `slides` jsonb, `revision` int, `originals` jsonb

### 마이그레이션 조회 목록

**테스트 / 26개**

- `20261101011245` bootstrap_isolated_test_news_cms
- `20261101011429` temporary_exact_media_seed_allowlist
- `20261101011527` revoke_temporary_media_seed_upload
- `20261101041438` news_optional_display_dates_only
- `20261101051707` create_calendar_events_overlap_guard_test
- `20261101060324` add_full_day_temporary_closure_category_test
- `20261101070758` schedule_admin_private_rich_memo_atomic_save_test
- `20261102025427` calendar_opening_20261022_boundary_marker_test_only
- `20261102035640` backup_temp_before_production_data_mirror_20261102
- `20261104033621` test_space_gallery_cms_20261104
- `20261104045719` test_news_event_categories_operator_type_20261104
- `20261107053415` test_sms_news_subscribers_challenges_campaigns_20261107
- `20261107054019` test_sms_news_temporary_network_healthcheck_20261107
- `20261107054225` test_sms_news_remove_temporary_pg_net_20261107
- `20261107061803` sms_connection_probe_pgnet_20261107
- `20261107062108` sms_connection_probe_cleanup_20261107
- `20261107062651` solapi_connection_check_temp_pg_net_20261107
- `20261107062814` cleanup_solapi_connection_check_pgnet_20261107
- `20261107065405` sms_onsite_manual_consent_without_fake_otp_20261107
- `20261107072830` sms_signup_gmail_notification_outbox_20261107
- `20261108022408` backup_pre_multipage_redesign_20261108
- `20261108163921` key_visual_admin_publishing
- `20261108171309` private_key_visual_originals
- `20261108171857` raise_key_visual_crop_limit
- `20261109130516` recommended_books_admin
- `20261111015512` book_originals

**운영 / 16개**

- `20261028083623` create_news_content_management_tables_and_rls
- `20261029051109` split_news_public_and_admin_select_rls_policies
- `20261029055229` news_expand_images_twenty_and_ongoing_ended_states
- `20261101113944` backup_before_calendar_migration_20261101
- `20261101114136` enable_calendar_news_dates_private_memos_production
- `20261102034143` backup_schedule_before_opening_calendar_20261102
- `20261102034223` add_20261022_opening_milestone_production_without_overwriting_events
- `20261104035151` production_space_gallery_cms_20261104
- `20261104035537` production_space_gallery_least_privilege_20261104
- `20261104073059` enable_pg_net_for_one_time_media_transfer_20261104
- `20261104073201` disable_temporary_pg_net_after_media_transfer_20261104
- `20261104073226` snapshot_news_posts_before_meetings_events_20261104
- `20261104073229` add_meeting_event_taxonomy_and_rental_columns_20261104
- `20261107104325` sms_notification_service_core_tables_20261107
- `20261107104433` sms_notification_new_signup_email_trigger_20261107
- `20261112045552` add_recommended_books_and_key_visual



## 2026-11-21 운영 방문 분석 추가 (v1.6)

- 사용자가 **운영사이트 우선 개발·배포, 테스트사이트 추후 반영**을 승인했다. 이 범위는 본 변경 1회에 한정하며 이후 기본 수정 대상은 여전히 테스트다.
- 운영 관리자 Top에 '방문 분석'(/admin/analytics.html) 신설. Google Analytics 4 기존 추적·보고서 함수와 운영 키는 유지했다.
- Microsoft Clarity는 별도 통계이며 기존 GA4 및 자체 추적 데이터와 합산하지 않는다. **2026-11-21 기준 CLARITY_PROJECT_ID와 CLARITY_API_TOKEN은 미등록**이어서 Clarity 실제 수집과 보고서는 아직 연동 대기 상태다.
- 정기 Clarity API 수집은 기존 /api/admin 함수에서 /api/cron-clarity rewrite로 처리한다. Vercel Hobby 함수 12개 제한으로 2개 추가 API 파일을 제거하고 lib 모듈로 통합했다. 1일 1회 4요청(Overall·Channel·Source·URL), DB 중복 예약, 관리자 조회에서는 Clarity API 호출 없음.
- 자체 페이지뷰·클릭·이동 구간은 운영 공개 페이지의 동의 선택 시에만 집계. 분석 입력값/쿼리문자열/개인 식별자 미저장. 상세 방문자 이동 경로를 복원하는 시스템이 아니라 개별 이동 구간의 건수 집계다.
- 운영 Supabase에 public.visit_daily_counts와 public.visit_clarity_snapshots, analytics_private 접근 검증·중복 방지 테이블 및 제한된 RPC를 신규 구축. 공개 통계 테이블은 RLS 활성화, 관리자 읽기만 허용.
- 운영 Vercel Production에는 VISIT_ANALYTICS_INGEST_SECRET 및 CRON_SECRET 등록. Clarity 프로젝트 자격 증명은 직접 발급 후 등록 및 별도 재배포 필요. 토큰을 저장소·문서에 기록하지 않는다.
- 첫 Vercel 배포(1a6f69d)는 Hobby의 서버리스 함수 수 초과로 실패. 수정 76b5595는 기존 /api/admin 함수로 통합해 READY 배포를 확인했다. 적용 점검 및 향후 SHA는 실제 Vercel 조회 결과에 따라 기록한다.
- 추가 검증 필요: 관리자 실로그인 및 GA4 비교, 동의 후 방문 통계 증가, Clarity 프로젝트 생성/실수집, 개인정보처리방침 외부 처리 고지, 브라우저 추적 차단 영향을 점검한다.


## 2026-11-21 운영 관리자 Clarity 우선 표시 (v1.7)

- 사용자 요청에 따라 운영 관리자 홈 방문 분석 카드 데이터 출처를 기존 GA4에서 **Microsoft Clarity 일일 캐시**로 교체했다. 카드 제목은 'Microsoft Clarity · 최근 24시간', 상세 링크는 '/admin/analytics.html'이며 카드 갱신 시 Clarity 외부 API를 직접 호출하지 않는다.
- 캐시 스냅샷이 없다면 수치를 만들어 내거나 GA4 지표를 대신 표시하지 않고 'Clarity 집계 대기'로 출력한다. 첫 수집 이후 세션·방문자와 유입 채널/출처를 표시한다.
- 관리자 '/admin/analytics.html'의 섹션 순서는 **Microsoft Clarity → 홈페이지 자체 행동 분석 → Google Analytics 4**. GA4 추적 스크립트 및 관리 보고서 기능은 유지하며 데이터는 합산하지 않는다.
- 이전 v1.6 기록 당시 미등록이던 CLARITY_PROJECT_ID 및 CLARITY_API_TOKEN은 이후 운영 Vercel Production에 등록·재배포되었다. 토큰 값은 문서나 GitHub에 포함하지 않는다.
- 테스트 저장소 및 테스트 Vercel 프로젝트는 변경하지 않는다.


## 2026-11-21 저장공간 안전 정리 및 미완료 항목 (v1.8)

### 수행 범위 및 결과
- 작업 대상은 운영 `jeong1pumho/jakgastudio`와 테스트 `jeong1pumho/jakgastudio-temp` 저장소/배포, 각각의 Supabase DB·Storage이다.
- 현재 홈페이지는 `lib/home-template.html`로 렌더링한다. 전체 런타임 소스 참조 점검에서 사용되지 않는 구버전 백업 `lib/legacy-home.html`(113,329 B/저장소)을 **운영·테스트 GitHub main에서 각각 제거**했다. 과거 버전은 Git 이력에 남아 복구 가능하다.
- 현재 Git 트리 기준 제거 분량: 운영 113,329 B + 테스트 113,329 B = **226,658 B**. 이는 소스/배포 포함 대상에서 제외된 크기이며, Vercel 청구 저장공간 또는 물리 스토리지 사용량이 동일하게 줄었다는 의미가 아니다.
- 테스트 정리 커밋 `14db5c2334a1e7f4787749cfa96b13d5c44849ec`, READY 배포 `dpl_9BZUVMqWcDSE57UXcxdkKeLSzQXT`; 운영 정리 커밋 `861c9f332fa90905dc92285714210f1b2aa4b285`, READY 배포 `dpl_DCN6CYZbEF5dahxYitQWRuAaBSg4`.
- 두 사이트에서 `/`, `/news`, `/space`, `/admin/`, `/admin/analytics.html` 응답 200, 해당 배포의 최근 서버 error/fatal 로그 미관측.

### 저장소 정밀 조사: 삭제하지 않은 항목
- Supabase Storage 메타데이터상 사용량: **운영 489,811,379 B**, **테스트 398,529,510 B**(2026-11-21 확인). 주요 데이터는 Key Visual, NEWS, 추천도서의 공개 이미지 및 원본이다.
- 현재 CMS(NEWS·추천도서·Key Visual·공간 갤러리)의 JSON/본문 문자열에 파일 이름이 나타나지 않는 객체가 **운영 11개(94,064,562 B), 테스트 11개(71,414,788 B)** 발견됐다. 이는 단순 문자열 비교로 구한 **삭제 후보**이지 사용하지 않는 파일로 확정된 것이 아니다. 비공개 원본, 외부 공유 URL, 임시 편집 이미지일 수 있다.
- 특히 `key-visual` 공개 변환물 중 운영 4개(67,401,794 B), 테스트 6개(65,118,544 B)는 추가 확인 우선 대상이다. `key-visual-originals`, `book-originals` 등 비공개 원본은 보존한다.
- Supabase 관리 연결에서 Storage 파일 객체 삭제 액션이 제공되지 않아 실제 대용량 스토리지 객체는 **삭제하지 않았다**. SQL로 `storage.objects` 행만 삭제하면 S3 파일이 남아 비용이 지속될 수 있으므로 엄격히 금지한다. 공식 문서: https://supabase.com/docs/guides/storage/management/delete-objects
- DB 테이블 크기는 매우 작고 `public` 스키마의 백업/임시 테이블은 발견되지 않았다. 행사·도서·모임·SMS·분석 데이터, DB migration 및 RLS는 변경하지 않았다.
- 기존 Vercel 배포본은 롤백·감사 목적을 보존했고, 안전한 삭제 동작을 지원하는 연결 도구도 확인되지 않아 삭제하지 않았다. Vercel Sandbox snapshot은 운영·테스트 프로젝트에서 각각 0개였다.
- `kv-fast` 이미지, `kv-20261109` 이미지, `space-refresh` 22개, 폰트 및 파비콘은 현행 템플릿 또는 런타임의 fallback·동적 경로에서 참조되어 삭제 금지한다.

### 추후 실제 스토리지 회수 절차
1. 각 객체의 저장 경로, CMS·코드 참조, 외부 공유 여부 및 필요 원본 여부를 함께 확인한다. 단순 DB 텍스트 매치가 없다는 이유만으로 삭제하지 않는다.
2. 필요하면 원본을 백업하고 정해진 보관기간을 확인한 다음, **Supabase Storage API `storage.from(bucket).remove([path])`** 또는 Storage 관리화면으로 삭제한다. SQL 직접 삭제는 하지 않는다.
3. 객체 수·총 바이트 전후 비교, 공개 URL·콘텐츠 렌더링, 관리자 기능 및 운영/테스트 분리까지 검증 후 실제 확보한 저장량을 기록한다.
4. 추가로 지원 도구가 제공되기 전까지 Storage 대용량 제거는 **미완료**로 취급하며, 이 절을 전체 정리 완료 기록으로 해석하지 않는다.
