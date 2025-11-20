# path: ops/FRONT_RELEASE.md
# Frontend Release Playbook

## 목표
- 버전 경로(`app/<git-sha>/`)에 업로드 후
- `app/latest/`를 새 버전으로 원자적 스위치
- HTML만 CloudFront 무효화

## 릴리즈 입력값
- `<git-sha>` (커밋 해시)
- 산출물 패키지(빌드 결과)

## 절차
1) 업로드: 산출물을 `app/<git-sha>/` 경로로 업로드  
2) 스위치: `app/latest/` 를 `<git-sha>`로 전환(객체 복사 또는 매니페스트 갱신)  
3) 무효화: CloudFront에 HTML만 invalidation 등록  
   - 예: `["/index.html", "/"]`  
4) 확인: `app.<your-domain>` 접속 → 버전/빌드 타임 확인

## 롤백
- `app/latest/` 를 직전 `<git-sha>` 로 되돌린다
- HTML만 다시 무효화(같은 경로)

## 수락 기준
- 새 버전에서 주요 경로 로드 OK
- 이전 버전으로 1분 내 롤백 가능 확인
- 배포 로그에 아래 정보 기록
  - 새 `<git-sha>`, 이전 `<git-sha>`
  - 무효화 경로·ID
  - 확인 시각/담당

## 주석
- SPA 라우팅을 쓰면 엔트리 HTML 목록을 별도로 관리
- 무효화 범위를 `/`로 하지 않는다(비용·지연 증가)
