# Docker Local Runtime

Control Room은 필요할 때 로컬 PC에서 Docker stack을 구동하는 local-first runtime을 우선했습니다.

## Screenshot needed

Planned assets:

```text
assets/screenshots/docker-compose-status.png
assets/diagrams/docker-local-runtime.png
```

필요한 이미지:

- Docker Desktop 또는 compose service 상태 화면
- Backend API, Frontend Console, Runner API가 compose stack으로 묶이는 구조 다이어그램

## 왜 local-first인가

이 프로젝트는 개인용 AI control room 성격이 강했습니다. 따라서 처음부터 도메인을 구매하고 클라우드 서버를 상시 운영하기보다, 필요할 때 내 PC에서 실행하는 방식이 더 현실적이었습니다.

장점:

- 도메인 구매가 필요 없음
- 상시 cloud server 비용이 없음
- 개인 credential과 local runtime을 외부에 노출하지 않음
- 개발/실험 속도가 빠름
- Docker로 실행 환경을 반복 가능하게 유지

## Docker를 선택한 이유

Docker는 local-first와 future cloud deployment 사이의 절충점이었습니다. 로컬 PC에서 실행하더라도 backend, frontend, runner를 service 단위로 분리할 수 있고, 나중에 cloud VM이나 container platform으로 옮길 가능성을 남길 수 있습니다.

## Runtime 구성

| Service | 설명 |
| --- | --- |
| Backend API | state, prompt, cycle, run, trace, model/runtime API |
| Frontend Console | operator UI |
| Runner API | local execution boundary와 readiness endpoint |
| Discord Bot / Ingress | Discord gateway message ingress |
| SQLite | local-first state store |

## Cloud migration 가능성

현재 구조는 local-first이지만, Docker 기반이므로 장기적으로는 다음 형태로 옮길 수 있습니다.

- single cloud VM에서 Docker Compose로 운영
- container hosting 환경에 backend/frontend/runner 분리 배포
- Discord ingress만 cloud에 두고 runner는 local에 유지
- database를 SQLite에서 managed database로 교체

포트폴리오에서는 이 부분을 “지금은 개인용 local runtime으로 단순하게 시작했지만, 서비스 경계는 cloud migration을 고려해 나눴다”는 설계 판단으로 설명할 수 있습니다.
