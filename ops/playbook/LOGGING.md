# path: ops/LOGGING.md
# Logging Spec (JSON)

## 필수 필드
- timestamp (RFC3339)
- level (DEBUG|INFO|WARN|ERROR)
- logger (모듈/로거명)
- request_id (없으면 생성)
- method (HTTP)
- path (요청 경로)
- status (HTTP 응답코드)
- duration_ms (처리 시간)
- user_id (로그인 시)
- message (요약)
- extra (선택: dict)

## 예시(JSON 1라인)
{"timestamp":"2025-01-01T12:00:00Z","level":"INFO","logger":"app.api","request_id":"abcd-1234","method":"POST","path":"/api/posts","status":201,"duration_ms":42,"user_id":17,"message":"post.create.ok"}
