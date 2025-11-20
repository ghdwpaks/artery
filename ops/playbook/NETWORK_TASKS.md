# path: ops/NETWORK_TASKS.md
# 네트워크 생성 순서 (실행 체크리스트)

1) VPC 생성: `10.0.0.0/16`
2) Subnet 생성:
   - Public-A: `10.0.0.0/24` in AZ A
   - Public-B: `10.0.1.0/24` in AZ B
   - Private-A: `10.0.10.0/24` in AZ A
3) IGW 생성/연결 → Public RT에 `0.0.0.0/0 -> IGW` 추가
4) NAT 게이트웨이 1개(AZ A) → Private RT에 `0.0.0.0/0 -> NAT` 추가
5) 보안그룹 4종 생성(`sg-alb`, `sg-ecs`, `sg-rds`, `sg-redis`) 후 규칙 적용
6) Subnet ↔ RT 연결:
   - Public-A/B ↔ Public RT
   - Private-A ↔ Private RT
7) 서브넷 속성 확인:
   - Public: 자동 퍼블릭 IP 할당 비활성(ALB는 자체 ENI 사용), 라우트는 IGW 경유
   - Private: 라우트는 NAT 경유
8) 점검:
   - ALB가 Public-A/B에 배치 가능한지
   - Private-A에서 ECR/SM/Logs로 아웃바운드 통신 가능한지
9) 기록:
   - VPC/Subnet/RT/NAT/SG의 ID를 `ops/NAMING.md` 하단에 메모

## 주의
- dev/test는 NAT 1개 구성으로 비용 절감(해당 AZ 장애 시 이그레스 제한 감수)
- prod 전환 시 Private Subnet을 AZ B에도 추가하여 Multi-AZ(RDS 포함) 확장
