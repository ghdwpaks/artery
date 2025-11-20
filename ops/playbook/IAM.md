# path: ops/IAM.md
# IAM Roles (CI/CD & ECS) — 최소 권장셋

## CodePipeline 서비스 역할
- 목적: 파이프라인 오케스트레이션
- 권한(관리형/유사): AWSCodePipelineFullAccess(또는 동등한 최소권한 조합)

## CodeBuild 서비스 역할
- 목적: 빌드/스캔/푸시
- 필수 권한
  - ECR: `ecr:GetAuthorizationToken`, `ecr:BatchCheckLayerAvailability`, `ecr:PutImage`, `ecr:InitiateLayerUpload`, `ecr:UploadLayerPart`, `ecr:CompleteLayerUpload`, `ecr:BatchGetImage`
  - Logs: `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`
  - S3(필요 시): 아티팩트 버킷 읽기/쓰기

## CodeDeploy 역할(ECS)
- 목적: ECS Blue/Green 제어(지금은 준비만)
- 권한(관리형/유사): AWSCodeDeployRoleForECS(또는 동등한 최소권한 조합)

## ECS Task Execution Role
- 목적: 런타임에서 이미지 풀/로그/시크릿 접근
- 권한(관리형): `AmazonECSTaskExecutionRolePolicy`
- 추가(최소): `secretsmanager:GetSecretValue`(해당 경로 한정), `logs:`(해당 로그그룹 리소스 한정), `ecr:`(이미지 풀 관련 읽기)

## ECS Task Role
- 목적: 애플리케이션이 외부 자원에 접근 시 사용
- 최소: 지금은 비워두고, 필요 리소스 생길 때마다 정밀 추가

## 태그/명명
- 리소스 태그 예: `Project=artery, Env=stg|prod, Owner=you`
