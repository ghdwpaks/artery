# path: ops/PIPELINE_BACKEND.md
# Backend CI (빌드·스캔·ECR 푸시만)

## 목적
- 커밋/태그 시 백엔드 이미지를 생성하고, 보안 스캔 후 ECR에 푸시한다.
- 배포는 하지 않는다(Blue/Green은 다음 단계에서 연결).

## 트리거
- 태그: `v` 또는 `v-rc.`
- 브랜치(선택): `develop`에만 실행하도록 권장

## 입력(필수 전제)
- ECR 리포지토리: `backend` (취약점 스캔 ON)
- 이미지 태그: `:<git-sha>` + `:vMAJOR.MINOR.PATCH`(태그 있을 때)
- 리소스 권한: CodeBuild/CodePipeline/CodeDeploy용 IAM 역할 준비
- 시크릿: 레지스트리 푸시에 필요한 권한은 OIDC/역할로 처리(키 저장 금지)

## 단계
1) 체크아웃  
2) 버전 추출(`git-sha`, `tag`)  
3) 컨테이너 이미지 빌드  
4) 보안 스캔(중요도 Critical/High 발견 시 실패로 설정 권장)  
5) ECR 로그인  
6) 이중 태깅 적용 → ECR 푸시  
7) 산출물 기록: 리포 이름, 태그, 다이제스트를 릴리즈 메모에 남김

## 산출물(기록 포맷 예)
- repo: `backend`
- tags: `<git-sha>`, `vX.Y.Z`(있다면)
- digest: `sha256:...`

## 실패 기준
- 빌드 실패, 푸시 실패
- 스캔 결과 Critical/High ≥ 1 (정책은 RISK에 맞춰 조정 가능)

## 수락 기준(이 문서 기준)
- 위 단계 정의에 동의하고, CI에서 “빌드·스캔·푸시만” 수행됨(배포 없음)
