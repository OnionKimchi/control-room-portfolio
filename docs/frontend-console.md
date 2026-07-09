# Frontend Console

Frontend Console은 Control Room의 operator UI입니다. 실행 권한을 갖는 runtime이 아니라, backend state와 run trace를 편집/검사하는 도구입니다.

## Screenshot needed

Planned asset:

```text
assets/screenshots/frontend-console.png
```

필요한 스크린샷:

- room/agent/model/prompt 설정 화면
- workflow/run 또는 node trace를 볼 수 있는 화면
- secret 값은 보이지 않고 configured/missing 상태만 보이는 화면

## 역할

Frontend Console은 사용자가 backend 상태를 다루기 쉽게 만드는 layer입니다. 실제 workflow 실행, secret resolution, model call, branch decision은 backend가 소유합니다.

## 주요 기능

| Feature | 설명 |
| --- | --- |
| Room configuration | room context, participant, role, max turn, channel binding 설정 |
| Agent management | agent별 이름, 역할, instruction, model profile 지정 |
| Prompt preview | backend가 compile한 conductor/assistant prompt를 read-only로 확인 |
| Model/tool assignment | room 또는 agent에 model profile과 tool catalog 항목 연결 |
| Workflow visualization | backend-owned workflow definition을 시각화 |
| Run inspection | run status, node trace, sanitized input/output, error 확인 |
| Secret status | 실제 secret value 없이 configured/missing 상태만 표시 |

## 왜 frontend가 실행 주체가 아닌가

이 프로젝트에서 frontend는 operator console입니다. frontend가 node 실행, branch decision, secret resolution을 직접 수행하면 다음 문제가 생깁니다.

- secret value가 browser로 노출될 위험
- workflow 실행 로직이 UI 상태와 섞임
- Discord ingress와 runner가 같은 execution authority를 공유하기 어려움
- trace와 stale-cycle protection을 일관되게 유지하기 어려움

그래서 frontend는 backend API를 통해 상태를 편집하고 실행 결과를 관찰하는 역할로 제한했습니다.

## 포트폴리오에서 보여주려는 점

Frontend Console 문서는 이 프로젝트가 Discord 채팅 화면만 있는 것이 아니라, 운영자가 system state와 workflow execution을 검사할 수 있는 관리 화면까지 고려했다는 점을 보여줍니다.
