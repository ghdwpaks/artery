# path: backend/README.md
# Backend Service

Health check: GET /healthz → 200, <100ms, no DB. ALB: 10s/5s, 3/2, 200–299

## Overview
- Django + DRF API 서비스
- 세션 기반 인증(쿠키)
- CloudWatch Logs(JSON), Sentry/X-Ray 연동 예정

## Run (env keys only; 실제 값은 ops/ENV..example 참고)
- 必: DB_HOST, DB_PORT, DB_NAME, DB_USER, DB_PASSWORD
- 必: SECRET_KEY
- 推奨: ALLOWED_ORIGINS, CSRF_TRUSTED_ORIGINS, CORS_ALLOWED_ORIGINS
- 推奨: LOG_LEVEL, SENTRY_DSN
