# path: ops/RATELIMIT.md
# Rate Limit Policy

## 전역(CloudFront/WAF 앞단)
- IP당 5분 3000 요청 (초기값)

## 애플리케이션(엔드포인트별)
- 로그인: 사용자/아이피 기준 1분 30회
- 글 작성: 사용자 기준 1분 10회
- 위반 시: HTTP 429 + `Retry-After` 헤더

## 주석
- 값은 시작점. 트래픽/오탐을 보며 분기별 조정
- 관리자/내부 IP는 별도 화이트리스트 고려
