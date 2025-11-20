# path: ops/ACM.md
# TLS Certificates (ACM)

## 개요
CloudFront와 ALB에 각각 인증서 1개씩:
- CloudFront용: 리전 `us-east-1`, 도메인 `app.<your-domain>`
- ALB용: 서비스 리전(예: `ap-northeast-2`), 도메인 `api.<your-domain>`

## 요청 절차
1) 콘솔에서 ACM 인증서 요청(Request public certificate)
   - us-east-1: `app.<your-domain>`
   - ap-northeast-2: `api.<your-domain>`
   - (필요 시 SAN 추가 가능)
2) DNS 검증 선택 → ACM이 제공하는 CNAME을 Route 53에 추가
3) 상태가 Issued가 될 때까지 대기(전파 수분 소요)

## 바인딩(연결) 시점
- CloudFront 배포 생성 시 Alternate Domain Name에 `app.<your-domain>` 입력 후, us-east-1 인증서 선택
- ALB 리스너(HTTPS:443) 생성 시, ap-northeast-2 인증서 선택

## 운영 메모
- 자동 갱신: DNS 검증 상태를 유지하면 자동 갱신
- HSTS Preload는 추후(prod 도메인 확정 후)만 검토
- 산출물 기록: 아래 ARN을 배포 문서에 붙여넣기
  - CloudFront cert ARN: `arn:aws:acm:us-east-1:...`
  - ALB cert ARN: `arn:aws:acm:ap-northeast-2:...`
