# path: ops/DOMAINS.md
# Domains (Route 53)

## 목적
- 프런트: app.<your-domain> → CloudFront
- 백엔드(API): api.<your-domain> → ALB

## 절차
1) Route 53에서 Hosted zone 생성: `<your-domain>` (예: example.com)
2) 도메인 레지스트라에서 NS 레코드를 Route 53 NS로 교체
3) 레코드 계획 (생성 시점은 리소스가 생긴 뒤 Alias로 연결)
   - `app.<your-domain>` → A/AAAA: Alias to CloudFront
     - 생성 시점: CloudFront 배포 생성 후
     - 값: `dxxxxxxxxxxxxx.cloudfront.net` (배포 도메인)
   - `api.<your-domain>` → A/AAAA: Alias to ALB
     - 생성 시점: ALB 생성 후
     - 값: `internal-...ap-northeast-2.elb.amazonaws.com` (ALB DNS)

## ACM 검증용 CNAME
- 인증서 요청 시 ACM이 제시하는 CNAME들을 이 Hosted zone에 추가
- 예: `_xxxx.app.<your-domain> -> _yyyy.acm-validations.aws`

## 참고
- HSTS Preload 신청 금지(프로덕션 도메인 확정 전)
- 레코드 생성은 “리소스 생성 → 도메인 Alias 연결” 순서로 진행
