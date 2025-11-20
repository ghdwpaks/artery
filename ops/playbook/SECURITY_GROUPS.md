# path: ops/SECURITY_GROUPS.md
# Security Groups (최소 규칙)

## SG 목록
- `sg-alb`: ALB 전용
  - Inbound: TCP 443 from `0.0.0.0/0`
  - Outbound: TCP 80/8080 to `sg-ecs`
- `sg-ecs`: ECS 서비스
  - Inbound: TCP 80/8080 from `sg-alb`
  - Outbound: TCP 443 to `0.0.0.0/0` (ECR/SM/Logs 등 외부 통신)
- `sg-rds`: PostgreSQL
  - Inbound: TCP 5432 from `sg-ecs`
  - Outbound: all to `0.0.0.0/0`(기본)
- `sg-redis`: ElastiCache for Redis
  - Inbound: TCP 6379 from `sg-ecs`
  - Outbound: all to `0.0.0.0/0`

## 포트 정책 메모
- 애플리케이션 컨테이너 포트는 80 또는 8080 중 하나로 고정
- 관리용 SSH 등은 없음(ECS/Fargate 사용, 퍼블릭 접속 불필요)

## 태그 (공통)
- `Project=artery`, `Env=dev`, `Owner=<you>`
