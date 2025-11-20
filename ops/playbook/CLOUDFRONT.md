# path: ops/CLOUDFRONT.md
# CloudFront (Frontend CDN)

## 도메인
- `app.<your-domain>` → CloudFront (ACM: us-east-1)

## 오리진
- S3 정적 버킷 + OAC 사용
- 오리진 경로: `app/latest/`

## 캐시 정책(권장)
- HTML: `Cache-Control: no-store, max-age=0`
- 정적 자산(해시 파일): `Cache-Control: public, max-age=31536000, immutable`

## 동작(Behaviors)
- 기본 경로: `/` → S3 오리진
- 압축: Gzip/Brotli ON
- HTTP → HTTPS 강제 리다이렉트

## 보안
- TLSv1.2 이상
- WAF 연결(선택)
- 헤더 전달 최소화(쿠키·헤더 불필요)

## 무효화(Invalidation) 원칙
- 릴리즈 시 HTML만 무효화:  
  - 예: `/index.html`, `/` 또는 SPA 엔트리 목록
- 정적 자산은 파일명 해시로 캐시 무효화가 자연 발생

## 로그(선택)
- 로그 버킷: `artery-cf-logs`
- 보존 180~365일

## 산출물(릴리즈 시 기록)
- 배포 ID, 무효화 ID, 무효화 경로 목록
