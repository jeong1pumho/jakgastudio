# 운영 방문 분석 변경 기준 (2026-11-21)

- 운영 www.jakgastudio.com만 수정. 테스트 환경 미변경.
- 관리자 Top 메뉴 '방문 분석' 추가: /admin/analytics.html.
- 기존 GA4 측정/보고서 그대로 유지. GA4 최근 7일과 Clarity 24시간(UTC) 보고서 통합 합산 금지.
- Clarity API 정기 수집: 매일 UTC 01:15, 4요청(Overall/Channel/Source/URL). DB의 UTC 일자 예약으로 중복 실행 차단. 관리자 새로고침에서 Clarity 호출 없음.
- Clarity API 제한: 프로젝트당 10회/일, 최근 72시간, 요청당 3차원, 1000행.
- 운영 Supabase 마이그레이션 production_visit_analytics_aggregate_and_clarity_cache. RLS 적용, 관리자만 집계 SELECT. raw 방문자 ID/이름/연락처/URL query 미저장.
- 운영 Vercel 비밀키 VISIT_ANALYTICS_INGEST_SECRET / CRON_SECRET 등록. CLARITY_PROJECT_ID 및 CLARITY_API_TOKEN은 Microsoft 관리자 계정에서 별도 발급 후 운영 Vercel에 등록 필요.
- 자체 행동 집계는 방문자가 동의한 경우만 운영하며 기존 집계 제외 브라우저 선택도 존중. 주요 이벤트 page_view / click / transition.
- Clarity 상세 녹화 및 히트맵은 공식 대시보드 링크로 이동.
- 검증: 운영 배포 READY, 관리자 인증·비관리자 차단, 자체 집계 동의 후 반영, GA4 정상 작동, Clarity 프로젝트 연결 후 실수집, 개인정보처리방침 검토.
