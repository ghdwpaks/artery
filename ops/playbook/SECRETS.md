# path: ops/SECRETS.md
# Secrets & Config 경로(정책 요약)

## 원칙
- 시크릿은 Secrets Manager에서 관리, 키 이름은 로컬 `.env`와 동일하게 유지
- 환경별 경로 분리: `/stg/app/<key>`, `/prod/app/<key>`
- KMS 기본 암호화 사용(기본키)

## 경로/키 목록(값은 비워둠)
### Staging (`/stg/app/...`)
- `/stg/app/db_host`
- `/stg/app/db_name`
- `/stg/app/db_user`
- `/stg/app/db_password`
- `/stg/app/secret_key`
- `/stg/app/redis_url`
- (선택) `/stg/app/sentry_dsn`

### Production (`/prod/app/...`)
- `/prod/app/db_host`
- `/prod/app/db_name`
- `/prod/app/db_user`
- `/prod/app/db_password`
- `/prod/app/secret_key`
- `/prod/app/redis_url`
- (선택) `/prod/app/sentry_dsn`

## 노출 금지 항목
- 장기 액세스 키(사용하지 않음). CI는 OIDC로 역할 가정
- 시크릿 값은 리포/로그/이슈에 남기지 않음

## 수락 기준
- 상기 경로를 기준으로 SM에 “키 이름만 동일”하게 생성해 둘 것(값은 추후 주입)
