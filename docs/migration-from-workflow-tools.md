# Migration From n8n and Activepieces

이 프로젝트는 처음부터 백엔드를 직접 구현하지 않고 n8n과 Activepieces로 시작했습니다. 당시에는 백엔드 서버를 직접 설계하는 것보다 workflow engine에서 노드를 연결하고 흐름을 검증하는 쪽이 더 익숙했고, 빠른 prototype을 만들기에도 적합했습니다.

n8n과 Activepieces가 유용했던 점은 다음과 같습니다.

- execution flow를 시각적으로 볼 수 있음
- Discord, HTTP call, model call, conditional branch를 빠르게 연결 가능
- 초기 실험에서 step-level input/output을 쉽게 확인 가능
- custom runtime을 만들기 전에 제품 형태를 검증 가능

## Why the Runtime Moved Into the Backend / 백엔드로 옮긴 이유

프로젝트가 단순 자동화에서 multi-turn, multi-role agent conversation으로 이동하면서 workflow tool을 source of truth로 유지하는 것이 점점 불편해졌습니다.

특히 다음 요구사항은 백엔드 코드로 직접 모델링하는 편이 더 적합했습니다.

- 명시적인 channel cycle state
- active cycle 도중 들어온 user interruption 처리
- stale callback/output이 최신 상태를 덮어쓰지 못하게 하는 보호 로직
- 테스트 가능한 prompt compilation
- 코드 리뷰 가능한 branch logic
- workflow tool credential 대신 backend-owned secret reference
- frontend가 볼 수 있는 durable trace record
- provider/tool adapter의 stable contract

목표는 workflow tool을 부정하는 것이 아니었습니다. workflow tool이 제공하던 **보이는 흐름**은 유지하되, 실행 권한과 상태 관리는 TypeScript backend가 소유하도록 옮기는 것이었습니다.

또한 AI coding 도구가 발전하면서 백엔드를 직접 구현하는 비용이 낮아졌습니다. frontend, backend, shared contracts를 TypeScript로 통일하면 schema, prompt contract, runtime envelope, trace shape를 한 언어권에서 관리할 수 있다는 장점도 있었습니다.

## What Stayed From the Workflow Phase / 남긴 것

workflow prototype 단계에서 확인한 제품 모델은 그대로 남겼습니다.

- Discord는 main user-facing surface
- room은 participants, prompts, model assignments, tool assignments를 묶는 단위
- conductor-style decision step은 다음 발화자와 cycle 지속 여부를 결정하는 데 유용
- execution은 node level traceability가 필요
- operator는 compiled prompt와 runtime state를 볼 수 있어야 함

## What Changed / 바뀐 것

| Before | After |
| --- | --- |
| Visual workflow runtime owns execution | Backend graph runner owns execution |
| Credentials stored in workflow-tool connections | Backend-owned secret references and masking |
| Implicit step payloads | Explicit run envelopes and node trace contracts |
| Debugging through workflow run UI | Debugging through backend traces and frontend visualization |
| Harder-to-test branch behavior | Code-owned branch logic and stale-cycle checks |

## Direction Change / 방향 전환

초기 목적은 Discord를 통해 로컬 PC의 AI/CLI runtime을 원격 조작하는 것이었습니다. 하지만 제작 중 Codex Desktop 등 외부 도구들이 이 영역을 더 안정적으로 해결하기 시작했고, 직접 CLI control을 구현하는 우선순위는 낮아졌습니다.

이후 프로젝트는 remote CLI control보다는 **Discord 기반 멀티턴 멀티롤 agent 토론 시스템**에 집중했습니다. 이 방향에서는 Discord ingress, room 설정, prompt compiler, model profile, cycle state, trace visualization이 핵심이었고, 기존 prototype에서 얻은 구조를 계속 활용할 수 있었습니다.
