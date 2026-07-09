# Discord Control Room

Discord is the main interaction surface for Control Room.

![Discord conversation preview](../assets/screenshots/discord-conversation-preview.png)

## 역할

Discord 화면은 단순한 챗봇 UI가 아니라, multi-turn, multi-role agent conversation이 실제 사용자에게 드러나는 공간입니다. 사용자는 일반 채팅처럼 메시지를 보내고, backend는 해당 메시지를 기록한 뒤 현재 channel cycle 상태에 따라 AI agent 응답을 조율합니다.

## 동작 흐름

1. 사용자가 Discord 채널에 메시지를 보냅니다.
2. Discord ingress가 bot 메시지와 범위 밖 메시지를 필터링합니다.
3. normalized message payload가 backend API에 기록됩니다.
4. backend가 해당 channel의 active cycle 여부를 확인합니다.
5. idle 상태라면 새 run/cycle을 시작하고, active 상태라면 최신 사용자 메시지 포인터를 갱신합니다.
6. conductor 역할의 agent가 대화 지속 여부와 다음 발화자를 결정합니다.
7. assistant 역할의 agent가 자신의 role/context에 맞게 응답합니다.
8. backend는 실행 결과와 branch decision을 trace로 남깁니다.

## 포트폴리오에서 보여주려는 점

이 스크린샷은 다음을 보여주기 위한 대표 이미지입니다.

- Discord를 별도 제작 UI가 아닌 실제 사용자-facing surface로 활용
- 여러 AI role이 같은 대화 공간에서 자연스럽게 발화
- backend orchestration이 사용자에게는 일반 채팅 경험으로 보이도록 설계
- 프로젝트가 단순 API 서버가 아니라 end-to-end interaction flow를 가진 시스템이라는 점

## 공개 시 주의한 점

이 저장소에 포함하는 Discord 스크린샷은 공개 가능한 범위만 사용합니다.

- private server/channel 정보는 노출하지 않음
- token, webhook URL, internal endpoint는 포함하지 않음
- production credential이나 secret 값은 포함하지 않음
- 포트폴리오 설명에 필요한 대화 화면만 사용
