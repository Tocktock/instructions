# Interactive HTML Explainer Reference Kit

이 묶음은 복잡한 제품·업무·화면·데이터·시스템을 사용자가 직접 조작하며 이해할 수 있는 standalone HTML로 만들기 위한 재사용 패키지입니다.

## 파일

- `AUTONOMOUS_INTERACTIVE_HTML_EXPLAINER_MASTER_INSTRUCTION_KO.md`
  - 조사, 모델링, 상호작용, 레이아웃, 컴포넌트 줌, 접근성, 검증, 자체 리뷰와 완료 조건을 담은 최종 마스터 지침
- `interactive-html-explainer-reference-example.html`
  - 배송 주소 변경이라는 중립적 예제로 만든 실행 가능한 standalone HTML

## 권장 사용법

1. 마스터 지침과 예시 HTML을 AI 에이전트에 함께 제공합니다.
2. 예시 HTML을 단순 이미지 참고가 아니라 Interaction Grammar와 State Model의 실행 가능한 기준으로 사용하라고 명시합니다.
3. 작업별로 대상 사용자, Learning Objective, 자료, 필수 시나리오를 추가합니다.
4. 최종 결과가 P0 계약과 Browser Runtime 검증을 통과하기 전에는 완료로 보고하지 않게 합니다.

## 예시 HTML이 구현하는 핵심 계약

- Fixed Navbar
- 업무 흐름 / 구현 흐름
- Architecture / Sequence
- Scenario와 조건 변경
- Previous / Next / Play / Slider
- Canvas 외부의 현재 영향 방향
- 명확한 Source / Target / Participant 강조
- 좌우 패널과 화면 집중 모드
- Timeline, State, Log
- Hover / Focus / Click / Touch
- Component Zoom
- Direct / 2-hop / Downstream
- Zoom 강화 설명
- Zoom 중 단계 이동 시 먼저 전체 보기 복원
- Desktop / Mobile / Keyboard / Reduced Motion
- 외부 의존성 없는 단일 HTML
