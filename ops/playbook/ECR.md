# path: ops/ECR.md
# ECR Repository (backend)

## 리포지토리
- 이름: `backend`
- 스캔: 취약점 스캔 ON
- 태그 정책: 불변(immutable), 기존 태그 재푸시 금지
- 라이프사이클(권장): 최근 20개 이미지 유지(자동 정리)

## 이미지 태그 규칙(이중 태그)
- `:<git-sha>` 예) `:3f2c1d0`
- `:vMAJOR.MINOR.PATCH` 또는 `:vMAJOR.MINOR.PATCH-rc.N`
- `latest` 사용 금지

## 푸시 예시(개념)
- 같은 이미지에 두 태그를 동시에 부여하고 푸시한다.
- 빌드/스캔/푸시까지만 CI에서 수행(배포 단계는 아직 비활성).
