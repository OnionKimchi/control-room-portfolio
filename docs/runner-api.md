# Runner API

Runner API는 backend가 로컬 실행 기능을 호출할 때 사용하는 boundary입니다.

## Screenshot needed

Runner API는 UI보다는 endpoint/flow 다이어그램이 적합합니다.

Planned asset:

```text
assets/diagrams/runner-api-boundary.png
```

필요한 이미지:

- Backend API가 Runner API를 호출하는 구조
- `/health`, `/ready`, `/execute` 같은 endpoint가 어떤 역할인지 보여주는 다이어그램

## 현재 위치

초기 목표에서는 Runner API가 로컬 PC의 CLI/AI 작업 실행을 담당하는 중요한 컴포넌트였습니다. 하지만 Codex Desktop 등 외부 도구가 로컬 코딩 작업 지시 영역을 더 잘 해결하게 되면서, 이 프로젝트에서 CLI control은 핵심 범위에서 빠졌습니다.

그럼에도 Runner API boundary는 남겨두었습니다. 이유는 backend가 browser나 Discord bot에서 직접 로컬 실행을 수행하지 않고, 별도 service boundary를 통해 실행 capability를 다루는 구조가 더 안전하고 확장 가능하기 때문입니다.

## 주요 책임

| Endpoint / Concept | 설명 |
| --- | --- |
| `/health` | process가 살아 있는지 확인 |
| `/ready` | 실제 실행 준비 상태 확인 |
| `/execute` | backend가 허용된 tool/action을 요청하는 boundary |
| Workspace validation | 허용된 workspace만 실행 대상으로 인정 |
| Tool allowlist | 등록된 tool/action만 실행 가능 |
| Normalized result | stdout/stderr/exitCode/error를 backend가 다루기 쉬운 형태로 반환 |

## 보안 관점

Runner API는 특히 조심해야 하는 컴포넌트입니다. 로컬 PC에서 command나 tool을 실행할 수 있는 boundary이기 때문에 다음 원칙이 필요합니다.

- 외부에 직접 노출하지 않음
- backend가 허용한 action만 수행
- workspace allowlist 적용
- secret value와 command policy를 frontend에 넘기지 않음
- 실행 결과는 normalized shape로 backend에 반환

## 포트폴리오에서 보여주려는 점

Runner API 문서는 중단된 CLI control 방향까지 솔직하게 설명하되, local execution boundary를 별도 service로 분리하려 했던 설계 판단을 보여줍니다.
