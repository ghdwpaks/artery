# path: ops/VPC_PLAN.md
# VPC Skeleton (dev/test)

## 주소 계획 (권장 기본값)
- VPC CIDR: `10.0.0.0/16`
- Public Subnet A (AZ A): `10.0.0.0/24`
- Public Subnet B (AZ B): `10.0.1.0/24`
- Private Subnet A (AZ A): `10.0.10.0/24`

## 구성 요소
- 인터넷 게이트웨이(IGW): Public RT에 연결
- NAT 게이트웨이: 1개(AZ A), Private RT의 기본 경로로 사용
- 라우트 테이블
  - Public RT: `0.0.0.0/0 -> IGW`
  - Private RT: `0.0.0.0/0 -> NAT(AZ A)`
- 가용영역(AZ) 선택: 예) `ap-northeast-2a`, `ap-northeast-2c` (두 AZ는 반드시 다르게)

## 네이밍
- VPC: `artery-dev-vpc`
- Subnets: `artery-dev-public-a`, `artery-dev-public-b`, `artery-dev-private-a`
- RTs: `artery-dev-public-rt`, `artery-dev-private-rt`
- NAT: `artery-dev-nat-a`
- IGW: `artery-dev-igw`

## 메모
- ALB는 서로 다른 AZ의 퍼블릭 서브넷 2개가 필수(고가용성 요구)
- Private Subnet은 dev/test에서 1개만 사용(비용 절감)
