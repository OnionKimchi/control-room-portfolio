# Retrospective / 회고

README는 처음 보는 사람이 빠르게 이해할 수 있는 요약 문서로 유지하고, 개인적인 배경과 방향 전환의 이유는 이 문서에 더 자세히 정리합니다.

## Initial Motivation / 초기 동기

초기 목표는 Discord를 이용해 원격으로 내 로컬 PC에 있는 AI에게 작업을 지시하는 것이었습니다. Discord는 모바일 접근성이 좋고, 별도의 웹 콘솔을 열지 않아도 명령을 남길 수 있으며, 대화 기록 자체가 command log처럼 남는다는 장점이 있었습니다.

처음 상정한 형태는 다음에 가까웠습니다.

- 모바일 또는 외부 환경에서 Discord로 작업 지시
- 로컬 PC의 runner가 명령을 받아 AI/CLI 작업 수행
- 백엔드가 작업 상태와 대화 cycle을 관리
- 프론트엔드 콘솔에서 설정과 실행 상태 확인

하지만 제작 중 Codex Desktop 등 로컬 PC의 코딩 작업을 원격/대화형으로 지시할 수 있는 도구가 더 빠르게 성숙했습니다. 직접 CLI control layer를 만드는 방향은 실용성이 낮아졌고, 프로젝트의 초점은 멀티턴 멀티롤 AI agent 토론 봇으로 이동했습니다.

프로젝트를 폐기하지 않은 이유는 이미 만들어둔 구성 요소들이 여전히 가치가 있었기 때문입니다. Discord ingress, channel cycle, room configuration, prompt compilation, model profile, trace visibility는 다중 역할 agent 대화 시스템에도 그대로 필요한 기반이었습니다.

## Technical Decisions / 기술적 결정

주요 결정은 다음과 같습니다.

- Discord를 사용자-facing interaction surface로 사용
- n8n과 Activepieces로 workflow orchestration을 먼저 prototype
- CLI control은 핵심 범위에서 제외
- multi-turn, multi-role agent conversation으로 제품 방향 전환
- orchestration authority를 TypeScript backend로 이동
- frontend는 실행 주체가 아니라 operator console로 제한
- runner boundary는 남기되, local CLI execution은 후순위 기능으로 보류

처음부터 백엔드를 직접 구현하지 않은 이유는 workflow engine을 다루는 쪽이 더 익숙했기 때문입니다. n8n과 Activepieces는 시각적으로 흐름을 만들고, 각 step의 입출력을 보면서 빠르게 실험하기 좋았습니다.

다만 AI coding 도구가 발전하면서 직접 백엔드를 구축하는 비용이 낮아졌고, TypeScript로 backend/frontend/shared contracts를 통일하는 장점이 커졌습니다. 그 결과 workflow tool은 prototype과 behavior reference로 남기고, runtime은 code-owned architecture로 재구성했습니다.

## Hard Parts / 어려웠던 지점

특히 어려웠던 부분은 단순 model call이 아니라 conversation runtime의 상태 관리였습니다.

- Discord 채널별 active cycle 관리
- cycle 실행 중 새 user message가 들어왔을 때의 interruption 처리
- 오래된 callback이나 model output이 최신 cycle을 덮어쓰지 못하게 막는 것
- conductor와 assistant prompt를 서로 다른 규칙으로 compile하는 것
- secret value는 숨기고 secret reference만 trace에 남기는 것
- workflow tool의 시각성을 잃지 않으면서 실행 권한은 백엔드 코드로 가져오는 것

## What I Would Improve / 개선하고 싶은 점

다시 만든다면 초기 milestone을 더 작게 잡았을 것입니다. 처음에는 remote local AI control, workflow automation, multi-agent conversation, frontend console, runner app이 한 프로젝트 안에 함께 들어왔고, 그만큼 목표가 흔들리기 쉬웠습니다.

현재 기준으로는 다음 순서가 더 나았을 것 같습니다.

- Discord message recording과 channel cycle state를 먼저 고정
- conductor/assistant prompt compilation을 별도 contract로 먼저 안정화
- mock model call 기반으로 multi-role conversation loop 완성
- 그 다음 real provider, frontend visualization, runner integration 순서로 확장

그래도 n8n/Activepieces prototype을 먼저 만든 것은 의미가 있었습니다. 최종 구조를 바로 맞히지는 못했지만, 어떤 상태와 trace가 필요한지 빠르게 확인하는 데 도움이 되었기 때문입니다.
