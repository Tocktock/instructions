---
name: autonomous-interactive-html-explainer
version: 1.0.0
language: ko
purpose: 복잡한 제품·사용자 경험·업무 절차·정책·데이터·시스템을 누구나 직접 조작하며 이해할 수 있는 standalone 인터랙티브 HTML로 만든다.
default_output: 단일 standalone HTML 파일
---

# 자율형 인터랙티브 HTML 설명서·시뮬레이터 최종 제작 지침

## 0. 이 지침의 목적

이 지침은 복잡한 내용을 단순한 문서나 정적 다이어그램으로 요약하는 데 목적이 없다.

최종 결과물은 사용자가 직접 조건을 바꾸고, 이전·다음 단계로 이동하고, 핵심 대상을 눌러 확대해 보고, 서로 연결된 원인과 결과를 따라가며 다음을 자신의 말로 설명할 수 있는 **실행 가능한 이해 도구**여야 한다.

- 지금 무슨 일이 일어나는가?
- 무엇이 시작점이고 무엇이 영향을 받는가?
- 왜 이 단계가 필요한가?
- 사용자는 무엇을 보고, 내부에서는 무엇이 바뀌는가?
- 조건을 바꾸면 흐름이 어디서 갈라지는가?
- 실패하면 어디까지 취소되고 어떻게 다시 처리되는가?
- 누가 책임과 수정 권한을 가지는가?
- 무엇이 구현됐고, 무엇이 부분 구현·차단·제안·외부 검증 상태인가?

기본 산출물은 **외부 의존성이 없는 단일 standalone HTML**이다.

이 지침은 다음 모든 역할이 사용할 수 있도록 일반화한다.

- Product Manager / Product Owner
- UX/UI Designer / UX Researcher
- Frontend Engineer
- Backend / Platform Engineer
- Data / ML Engineer
- QA
- Operations / Customer Support
- Security / Release Reviewer
- Leadership
- 신규 입사자, 고객, 파트너

---

# 1. AI 에이전트의 역할과 자율성

AI 에이전트는 한 작업 안에서 다음 역할을 함께 수행한다.

- **Researcher**: 최신 자료와 실제 동작을 확인한다.
- **Domain Analyst**: 용어, 규칙, 예외, 상태와 책임을 정리한다.
- **Product Thinker**: 사용자 가치와 제품 목적을 흐름에 연결한다.
- **Information Architect**: 복잡한 정보를 학습 가능한 구조로 나눈다.
- **Interaction Designer**: 사용자가 무엇을 조작하며 무엇을 발견할지 설계한다.
- **Visual Designer**: 위계, 간격, 대비, 화살표, 상태, 모션을 설계한다.
- **Frontend Engineer**: 실제로 동작하는 standalone HTML을 구현한다.
- **Accessibility Reviewer**: 키보드, 터치, 화면 읽기, 색 대비를 검증한다.
- **QA Engineer**: 모든 시나리오와 입력 조합을 검증한다.
- **Evidence Reviewer**: 사실, 가정, 제안, 로컬 검증, 외부 검증을 분리한다.

## 1.1 자율 실행 원칙

다음 흐름을 처음부터 끝까지 스스로 수행한다.

```text
목표 이해
→ 최신 자료 조사
→ 대상 사용자와 학습 목표 정의
→ Entity·Relation·State·Scenario 모델링
→ 가장 적합한 인터랙티브 형식 선택
→ UI와 상호작용 설계
→ standalone HTML 구현
→ 정적·상태·브라우저 검증
→ 여러 역할 관점 자체 리뷰
→ 가장 약한 부분 실제 개선
→ 전체 재검증
→ 최종 파일과 증거 보고
```

다음 원칙을 지킨다.

- 자료에서 확인할 수 있는 내용을 사용자에게 다시 묻지 않는다.
- 이미 제공된 요구사항을 다시 확인받지 않는다.
- 되돌릴 수 있고 위험이 낮으며 약 70% 이상 확신하는 결정은 진행한 뒤 검증한다.
- 기능을 잘게 쪼개 결과를 불완전하게 만들지 말고, 가장 작은 **완전한 해결책**을 만든다.
- 초안을 만든 후 비판만 하지 말고 실제 HTML을 수정한다.
- 알 수 없는 사실은 그럴듯하게 채우지 않는다.
- 외부 전송, 계정 변경, 배포, 운영 데이터 수정, 권한 확대, 구매, 파괴적 작업은 별도 승인을 받는다.

---

# 2. 작업 입력 계약

가능한 범위에서 다음 정보를 사용한다.

```text
주제 또는 기능:
기준 자료:
대상 사용자:
사용자가 반드시 이해해야 할 것:
사용자가 결정해야 할 것:
반드시 포함할 시나리오:
반드시 지킬 행동:
브랜드·디자인 제약:
참고 HTML·이미지·영상:
출력 파일명:
```

정보가 일부 비어 있어도 자료로 해결할 수 있다면 바로 진행한다.

질문은 다음처럼 자료만으로 해결할 수 없는 경우에만 한다.

- 제품 의도나 정책 선택
- 브랜드 방향
- 위험 수용 여부
- 접근 권한
- 법무·보안 승인
- 실제 운영 변경 여부

---

# 3. Source of Truth와 Evidence 계약

## 3.1 Authority Map

작업 시작 시 무엇이 어떤 사실을 소유하는지 정리한다.

예:

```text
제품 의도: 최신 승인 PRD와 결정 로그
화면 구조: 최신 디자인 파일과 디자인 시스템
실제 동작: 최신 코드와 실행 가능한 테스트
API 계약: 현재 스키마와 구현
데이터 정의: 분석 이벤트 명세와 쿼리
운영 절차: 승인된 Runbook
출시 상태: 배포 기록과 기능 제어 상태
사용자 문제: 인터뷰·VOC·지원 티켓
```

## 3.2 일반적인 우선순위

자료가 충돌하면 보통 다음 순서를 우선한다.

1. 현재 실행되는 동작 또는 최신 실제 구현
2. 현재 스키마·API·정책·디자인 시스템 계약
3. 집중 테스트·Fixture·재현 가능한 증거
4. Runtime 설정과 Wiring
5. 최신 제품·디자인·운영·아키텍처 문서
6. PR·이슈 설명
7. 과거 문서와 요약

작업마다 실제 Authority Map을 명시한다.

## 3.3 Evidence 수준을 분리한다

다음은 서로 다른 증거다.

- 정적 코드·자료 검토
- 로컬 Unit/UI 테스트
- 로컬 DB·Integration 테스트
- CI
- Staging
- Production
- Human Review / Approval

한 수준의 성공을 다른 수준으로 확대 해석하지 않는다.

화면에서는 다음 상태를 명확하게 구분한다.

- `IMPLEMENTED`
- `LOCALLY_VERIFIED`
- `STAGING_VERIFIED`
- `PRODUCTION_VERIFIED`
- `PARTIAL`
- `SAFELY_BLOCKED`
- `PROPOSED`
- `NOT_IMPLEMENTED`
- `EXTERNAL_VALIDATION_REQUIRED`

시각적으로 완성도가 높다는 이유만으로 실제보다 강한 검증 상태처럼 보이게 해서는 안 된다.

---

# 4. Audience Matrix와 Learning Objectives

## 4.1 Audience Matrix

최소한 다음 표를 내부적으로 작성한다.

| 대상 | 이미 아는 것 | 꼭 이해해야 할 것 | 피해야 할 과부하 | 필요한 상세 수준 | 사용 후 행동 |
|---|---|---|---|---|---|
| 일반 사용자 | 제품 목적 | 어떤 선택이 어떤 결과를 만드는지 | 내부 클래스·DB | 낮음 | 흐름 설명 |
| PM | 정책과 목표 | 예외·영향·지표·운영 위험 | 과도한 구현 용어 | 중간 | 결정·우선순위 |
| Designer | 화면과 사용자 여정 | 상태·오류·접근성·콘텐츠 | 무관한 저장소 세부 | 중간 | 설계 수정 |
| Frontend | UI 구조 | 이벤트·요청·상태·캐시 | 무관한 운영 내부 | 높음 | 구현 |
| Backend/Data | 도메인·API | 데이터·일관성·복구 | 없음 | 높음 | 구현·리뷰 |
| QA/Ops | 기대 결과 | 조합·경계·진단·복구 | 모호한 추상화 | 높음 | 테스트·운영 |
| Leadership | 목표와 위험 | 가치·비용·결정점 | 구현 세부 | 낮음 | 승인·투자 |

## 4.2 Learning Objectives

“전체 구조를 이해한다”처럼 모호한 목표를 쓰지 않는다.

좋은 목표 예:

- 사용자는 조건 A와 B에서 화면과 처리 결과가 어떻게 달라지는지 설명할 수 있다.
- PM은 정책 옵션 두 개의 사용자 영향과 운영 비용 차이를 비교할 수 있다.
- 디자이너는 초기·로딩·빈 상태·오류·성공 상태의 전환 조건을 확인할 수 있다.
- 프론트엔드 개발자는 클릭부터 요청, 캐시, 화면 상태 변경까지 추적할 수 있다.
- 백엔드 개발자는 저장·이벤트·재시도·Rollback 경계를 확인할 수 있다.
- QA는 역할·입력·네트워크·오류 조합에서 기대 결과를 만들 수 있다.

모든 시나리오, 버튼, 애니메이션, 패널은 최소 하나의 Learning Objective에 기여해야 한다.

---

# 5. 가장 적합한 인터랙티브 형식 선택

모든 주제를 아키텍처 박스와 화살표로 표현하지 않는다.

| 사용자가 알고 싶은 것 | 우선 형식 |
|---|---|
| 사용자가 무엇을 경험하는가 | User Journey / Screen Flow |
| 화면 상태가 어떻게 변하는가 | UI State Machine |
| 팀과 시스템이 어떻게 협업하는가 | Service Blueprint / Swimlane |
| 조건에 따라 어디로 갈라지는가 | Decision Tree / Scenario Simulator |
| 변경 전후가 어떻게 다른가 | Before/After / Option Compare |
| 시간에 따라 계획이 어떻게 진행되는가 | Timeline / Roadmap Playback |
| 지표가 어떻게 만들어지는가 | Data Lineage / Funnel |
| 누가 누구에게 순서대로 전달하는가 | Interactive Sequence Diagram |
| 무엇이 무엇에 의존하는가 | Architecture / Dependency Map |
| 장애와 복구가 어떻게 진행되는가 | Incident Replay / Recovery Simulator |
| 화면 내부 요소의 역할과 상태 | Interactive Component Anatomy |

두 개 이상의 View를 사용할 때는 하나의 Scenario·Step·State 모델을 공유한다.

View를 바꿔도 다음은 유지돼야 한다.

- 현재 시나리오
- 현재 단계
- 입력값
- 현재 상태
- 선택한 관점
- 필요하다면 선택한 컴포넌트

---

# 6. 설명 관점 모드

복수 역할이 함께 사용하는 경우 같은 흐름을 여러 언어 수준으로 전환한다.

권장 모드:

- **쉬운 설명 / 업무 흐름**
- **Product / PM**
- **UX / Design**
- **Frontend**
- **System / Backend**
- **Data / Operations**
- **QA / Release**

모드는 이름과 상세 수준만 바꾸며 실제 시나리오와 상태는 바꾸지 않는다.

## 6.1 쉬운 설명 작성 원칙

기술 용어를 한글로 직역하는 것만으로는 쉬운 설명이 아니다.

다음과 같이 인과관계와 결과를 설명한다.

```text
A가 저장되면 A와 연결된 B와 C도 같은 내용으로 맞출 작업을 함께 저장합니다.

처리가 끝나기 전에 다른 수정이 들어오면 실패시키지 않고 잠시 보관합니다.
앞선 확인이 끝난 뒤 보관된 수정부터 순서대로 다시 처리합니다.

같은 번호를 사용하더라도 서로 다른 종류의 데이터라면 자동으로 같은 것으로 연결하지 않습니다.
확인된 관계가 있을 때만 연결합니다.
```

쉬운 설명에서 지켜야 할 규칙:

- 실제 사람이 업무를 설명하듯 쓴다.
- 명사만 나열하지 않는다.
- “무엇이 바뀌고 그래서 무엇이 달라지는지”를 말한다.
- 사용자에게 보이는 변화와 내부 변화를 구분한다.
- 기술 용어는 쉬운 뜻을 설명한 뒤 보조적으로 표시한다.
- 실패·미구현·한계를 숨기지 않는다.
- 한 문장 안에 너무 많은 개념을 넣지 않는다.
- `처리`, `동기화`, `컨텍스트`, `프로젝션` 같은 추상어를 반복하지 않는다.

## 6.2 전문 설명 작성 원칙

- 실제 클래스·API·이벤트·상태·테이블 이름을 정확히 사용한다.
- 현재 단계와 무관한 세부는 접어 둔다.
- 구현 사실과 제안을 구분한다.
- 코드·문서·디자인·분석 근거를 연결한다.
- Transaction, Lock, Idempotency, Retry, Rollback 경계를 정확히 표시한다.

---

# 7. 기본 화면 정보 구조

## 7.1 Fixed Navbar

스크롤과 관계없이 핵심 조작을 상단에 고정한다.

P0 권장 항목:

```text
View 전환
관점 전환
Reset
Previous
Play / Pause
Next
Step Slider
현재 단계 / 전체 단계
재생 속도
왼쪽 패널 토글
오른쪽 패널 토글
화면 집중 모드
Theme / Help
```

버튼을 과도하게 넣지 말고 현재 학습 목표에 필요한 최소 완전 세트를 선택한다.

## 7.2 왼쪽 패널 — Scenario & Inputs

- 시나리오 선택
- 사용자 역할
- 조건과 입력값
- 권한·플랜·기능 상태
- 오류 주입
- 비교 대상
- 시나리오 목표
- 구현·검증 상태
- 색상과 관계 범례

## 7.3 가운데 — Main Interactive Workspace

권장 순서:

```text
1. 현재 단계의 영향 방향
2. 컴포넌트 포커스·줌 도구
3. 실제 Diagram / Journey / State View
4. Timeline
5. 현재 상태 카드
6. 실행·결정 로그
```

## 7.4 오른쪽 패널 — Current Step / Entity Inspector

현재 단계 설명:

- 지금 무슨 일이 일어나는가
- 시작점과 영향 대상
- 사용자에게 보이는 변화
- 내부 변화
- 확인 조건
- 읽는 정보와 저장하는 정보
- 책임자·소유자
- Transaction·Rollback
- 실패와 복구
- 다음 단계에 미치는 영향
- Evidence와 현재 한계

컴포넌트 선택 시:

- 역할과 존재 이유
- 현재 단계에서 하는 일
- 입력과 출력
- upstream / downstream
- 읽고 쓰는 데이터
- 소유 팀과 수정 권한
- 안전 규칙
- 실패와 복구
- 관련 시나리오와 단계
- 코드·디자인·문서 근거
- 현재 제한사항

---

# 8. 현재 영향 방향 영역 — 절대 Overlay로 만들지 않는다

현재 단계의 `시작점 → 행동 → 영향 대상`은 Diagram보다 먼저 보여야 하지만, Diagram을 가려서는 안 된다.

## 8.1 필수 레이아웃 계약

현재 영향 방향은 **Diagram Canvas 바깥의 독립된 레이아웃 행**에 배치한다.

```text
┌──────────── 현재 단계의 흐름 ────────────┐
│ 시작점 → 무엇을 전달하거나 변경 → 영향 대상 │
└───────────────────────────────────────────┘

┌──────────── Main Diagram ────────────────┐
│ 컴포넌트, 화면, 화살표                    │
│ 영향 방향 박스가 이 영역을 덮지 않음      │
└───────────────────────────────────────────┘
```

금지:

- Canvas 위에 `position:absolute`로 띄우기
- 컴포넌트 카드나 화살표 위에 겹치기
- Scroll 시 다른 섹션을 침범하기
- 화면이 좁을 때 텍스트를 잘라 의미를 잃게 하기

## 8.2 표시 내용

```text
[지금 시작한 곳]
사용자 입력 화면

          → 배송 주소 변경 요청을 전달 →

[이번에 영향을 받는 곳]
주문 기준 기록
```

전문 모드:

```text
FROM: AddressForm
ACTION: submit address-change command
TO: OrderChangeService
```

## 8.3 반응형

- Desktop: 시작점, 화살표·행동, 영향 대상을 한 행에 표시
- Mobile: 시작점 → 행동 → 영향 대상을 세로로 쌓음
- 긴 문장은 2줄까지 자연스럽게 줄바꿈
- Tooltip이 없어도 핵심 의미가 읽혀야 함

---

# 9. 컴포넌트·Entity Catalog

기술 서비스만 컴포넌트로 보지 않는다.

다음 모두 Entity가 될 수 있다.

- 사용자·Persona
- 화면·페이지·모달
- 버튼·폼·UI 컴포넌트
- 팀·담당자·승인자
- 정책·규칙·결정
- 비즈니스 객체
- API·서비스·Queue·DB
- 이벤트·문서·지표·알림
- 외부 파트너

모든 View에서 하나의 Catalog를 공유한다.

```js
const entityCatalog = {
  requestForm: {
    kind: "screen",
    names: {
      plain: "변경 요청 화면",
      technical: "RequestChangeForm"
    },
    summary: {
      plain: "사용자가 변경할 내용을 입력하고 제출하는 화면입니다.",
      technical: "Collects and validates a change request before submission."
    },
    responsibilities: [],
    inputs: [],
    outputs: [],
    reads: [],
    writes: [],
    owner: "Customer Experience Team",
    states: ["default", "invalid", "submitting", "success", "error"],
    invariants: [],
    failureModes: [],
    recovery: [],
    sourceRefs: [],
    limitations: []
  }
};
```

모든 화면 Node와 Sequence Actor는 같은 Entity ID를 사용한다.

---

# 10. Relationship와 화살표

## 10.1 관계 유형

필요에 따라 다음을 사용한다.

```text
USER_ACTION
NAVIGATES_TO
TRANSITIONS_TO
CAUSES
UPDATES
READS
WRITES
REQUESTS
RESPONDS
HANDOFF
APPROVES
BLOCKS
DEPENDS_ON
NOTIFIES
MEASURES
FEEDBACK
RECOVERS
```

## 10.2 화살표 규칙

- Source와 Target을 명확히 표시한다.
- 화살표에 명사가 아니라 동사형 설명을 쓴다.
- 현재 관계만 강하게 강조한다.
- 비활성 관계는 낮은 대비로 둔다.
- 큰 Arrowhead를 사용한다.
- Node 내부를 관통하지 않게 한다.
- 선과 라벨이 겹치지 않게 한다.
- 필요하면 곡선, 직교 Routing, Lane, Grouping을 사용한다.
- Resize, 패널 토글, 줌 변화 뒤 연결선을 재계산한다.
- 색상만으로 관계 종류를 구분하지 않는다.

나쁜 라벨:

```text
Order
Publication
Sync
```

좋은 라벨:

```text
변경 내용을 저장 요청
같은 기준으로 연결 주문 생성
적용 결과를 다시 확인
운영 담당자에게 검토 전달
```

## 10.3 Lane 제목

- 항상 가로 텍스트로 표시한다.
- 세로 회전·글자 단위 줄바꿈을 금지한다.
- 좁으면 자연스럽게 2줄로 줄바꿈한다.
- Lane은 사용자·화면·업무·데이터·운영처럼 이해 가능한 그룹을 우선한다.

---

# 11. 현재 활성 요소의 시각 언어

현재 단계에서 무엇이 실제로 동작하는지 한눈에 보여야 한다.

권장 상태:

```text
청록색  : 지금 시작한 곳
노란색  : 이번 단계에서 직접 영향을 받는 곳
보라색  : 같은 처리 안에서 함께 동작하는 곳
초록색  : 앞 단계에서 완료된 곳
주황색  : 잠시 대기·보관·재시도 상태
빨간색  : 오류·차단·중단 상태
낮은 명도: 현재 단계와 무관한 곳
```

시작점과 영향 대상은 단순히 “조금 더 밝게” 하지 않는다.

다음을 함께 사용한다.

- 2~3px 명확한 테두리
- 서로 다른 색
- Glow 또는 그림자
- 카드 내부 Role Badge
- `시작`, `영향`, `함께 동작` 텍스트
- 현재 화살표와 동일한 색상 연결

색만으로 의미를 전달하지 않는다.

---

# 12. 컴포넌트 Hover·Focus·Click·Touch

모든 핵심 Entity는 상세 설명을 가진다.

| 동작 | 결과 |
|---|---|
| Hover | 짧은 역할 Tooltip |
| Keyboard Focus | 동일한 Tooltip |
| Click / Enter / Space | 컴포넌트 포커스·줌인 |
| Touch | 모바일 상세 화면 또는 Bottom Sheet |
| Outside Click | 임시 Tooltip 닫기 |
| Esc | 줌아웃 또는 선택 해제 |

HTML `title` 속성만으로 구현하지 않는다.

Tooltip은 다음을 지킨다.

- Viewport 안에 배치
- 고정 Navbar를 침범하지 않음
- Panel toggle·Resize·Scroll 후 재배치
- 짧고 현재 이해에 필요한 내용만 표시
- 핵심 정보는 Tooltip에만 두지 않음

---

# 13. 컴포넌트 포커스와 줌인·줌아웃

이 기능은 단순 확대가 아니라 **Contextual Drill-down**이다.

## 13.1 클릭 시 동작

핵심 컴포넌트를 클릭하면:

```text
자동 재생 중지
→ 현재 단계 유지
→ 전용 줌 화면 진입
→ 선택 컴포넌트를 중앙에 크게 표시
→ 관련 입력·출력 관계만 표시
→ 무관한 컴포넌트는 숨기거나 강하게 흐림
→ 강화된 설명 Inspector 표시
```

컴포넌트 포커스는 실제 시뮬레이션 상태를 변경하는 명령이 아니라 현재 상태를 바라보는 **탐색 Lens**다.

```text
현재 Scenario와 Step = 실행 상태
선택한 Component = 탐색 상태
```

## 13.2 관계 범위

기본값은 `직접 연결`이다.

```text
[ 직접 연결 ] [ 2단계 ] [ 전체 Downstream ]
```

- 직접 연결: 바로 들어오고 나가는 관계만
- 2단계: 이웃의 앞뒤 관계까지
- Downstream: 선택 컴포넌트 이후 영향을 받는 전체 흐름

관계가 많아지면 카드 개수, 그룹, 스크롤, 최대 높이를 제한한다.

## 13.3 줌 화면 구성

```text
┌──────────── Zoom Header ─────────────┐
│ 컴포넌트 이름 · 관계 범위 · 80~150% · 줌아웃 │
└───────────────────────────────────────┘

┌──────── Related Flow ────────┬──── Detailed Explanation ────┐
│ Incoming → Selected → Outgoing│ 왜 필요한가                    │
│ 1-hop / 2-hop / downstream    │ 현재 단계에서 하는 일           │
│ 관련 컴포넌트 클릭 이동        │ 입력·출력·읽기·쓰기              │
│                                │ 안전 규칙·실패·복구·한계          │
└───────────────────────────────┴───────────────────────────────┘
```

## 13.4 줌인 설명 강화 계약

최소한 다음 내용을 보여준다.

- 이 컴포넌트가 왜 필요한가
- 없으면 어떤 문제가 생기는가
- 현재 단계에서 정확히 무엇을 하는가
- 무엇을 입력받는가
- 무엇을 출력하거나 변경하는가
- 어떤 데이터를 읽고 쓰는가
- 누가 소유하고 수정 권한을 가지는가
- 어떤 Guard와 불변조건을 지키는가
- 어디까지 같은 Transaction인가
- 실패하면 어디까지 Rollback되는가
- Retry·Replay·Manual Recovery는 어떻게 되는가
- 어떤 시나리오와 단계에서 등장하는가
- 관련 코드·디자인·문서·지표는 무엇인가
- 현재 구현의 한계와 외부 검증 항목은 무엇인가

쉬운 설명 모드에서는 자연어로, 전문 모드에서는 정확한 구현 이름으로 표시한다.

## 13.5 줌 상태와 흐름 재생 분리

줌인 상태에서 단계가 그대로 재생되면 관계 화살표와 현재 단계 화살표가 뒤섞인다.

따라서 다음·이전·Slider·Timeline·Reset·Play는 반드시 다음 순서를 따른다.

```text
1. 줌 화면 닫기 애니메이션
2. 선택 컴포넌트와 Focus Dim 제거
3. 전체 Diagram 복원
4. 전체 화면이 안정된 뒤 단계 이동 또는 재생 시작
```

컴포넌트 줌인 중 자동 재생이 켜져 있다면 즉시 일시정지한다.

권장 구현:

```js
async function leaveZoomBefore(action) {
  if (!uiState.zoomedComponentId) {
    action();
    return;
  }

  await closeZoomAnimation();
  uiState.zoomedComponentId = null;
  uiState.focusScope = "direct";
  renderFullDiagram();
  await waitForLayoutStability();
  action();
}

nextButton.addEventListener("click", () => {
  leaveZoomBefore(() => goToStep(currentStep + 1));
});
```

## 13.6 수동 줌아웃

`줌아웃`은 다음을 모두 수행한다.

```text
줌 화면 닫기
컴포넌트 선택 해제
관련 없는 Node의 흐림 제거
오른쪽 패널을 현재 단계 설명으로 복귀
전체 Diagram 복원
```

`Esc`도 같은 의미로 동작한다.

화면 집중 모드와 컴포넌트 줌을 혼동하지 않는다.

```text
화면 집중 모드
→ 좌우 패널을 숨기고 중앙 공간 확대

컴포넌트 줌
→ 하나의 Entity와 관련 관계를 자세히 탐색
```

## 13.7 확대 비율

- 기본 100%
- 최소 80%
- 최대 150%
- `+`, `-`, `0`, `Ctrl/Cmd + Wheel`
- `맞춤` 버튼
- 모바일에서는 화면 폭에 맞추고 별도 확대보다 스크롤을 우선

---

# 14. 좌우 패널과 화면 집중 모드

- 왼쪽과 오른쪽 패널은 독립적으로 열고 닫을 수 있어야 한다.
- 한쪽 패널을 닫으면 중앙 영역이 자동으로 넓어진다.
- 화면 집중 모드는 양쪽 패널을 숨긴다.
- 집중 모드를 해제하면 이전 패널 상태를 복원한다.
- 패널 상태는 필요하면 localStorage에 기억한다.
- 모바일에서는 한 번에 하나의 Drawer만 연다.
- Backdrop과 Esc로 닫을 수 있어야 한다.
- 패널 변화 뒤 화살표와 Tooltip 위치를 재계산한다.

---

# 15. Scenario·Step·State 데이터 모델

DOM을 직접 여기저기 수정하는 방식이 아니라 데이터 모델을 Source of Truth로 사용한다.

## 15.1 Simulator State

```js
const simulatorState = {
  scenarioId: "",
  stepIndex: 0,
  viewMode: "journey",
  perspectiveMode: "plain",

  selectedEntityId: null,
  zoomedEntityId: null,
  focusScope: "direct",
  zoomScale: 1,

  leftPanelOpen: true,
  rightPanelOpen: true,
  layoutFocusMode: false,

  playing: false,
  playbackSpeedMs: 1200
};
```

## 15.2 Scenario Model

```js
const scenario = {
  id: "change-success",
  category: "변경 처리",
  titles: {
    plain: "사용자가 정보를 바꾸고 관련 화면도 함께 갱신",
    technical: "Successful change propagation"
  },
  audience: ["product", "design", "frontend", "backend", "qa"],
  goal: "변경 저장부터 관련 화면과 알림까지의 흐름을 이해한다.",
  inputs: [],
  initialState: {},
  buildSteps(inputs) { return []; },
  verdict: "IMPLEMENTED"
};
```

## 15.3 Step Model

```js
const step = {
  id: "persist-change",
  phase: "SAVE",

  from: "changeApi",
  to: "masterRecord",
  relationType: "WRITES",

  labels: {
    plain: "변경 내용을 기준 기록에 저장합니다.",
    technical: "Persist validated change in the source-of-truth record."
  },

  summaries: {},
  activeEntities: [],
  participatingEntities: [],
  visitedEntities: [],
  entityStates: {},

  conditions: [],
  reads: [],
  writes: [],
  userVisibleChanges: [],
  internalChanges: [],
  ownership: [],
  transaction: {},
  rollback: {},

  statePatch: {},
  logs: [],
  sourceRefs: [],
  evidence: [],
  limitations: [],
  nextHint: ""
};
```

## 15.4 상태 재현

현재 상태는 항상 다음처럼 계산한다.

```text
initialState
+ step 1 statePatch
+ step 2 statePatch
+ ...
+ current step statePatch
= currentState
```

이전 단계로 갈 때 DOM을 역으로 수정하지 않는다.

다음 동작은 모두 동일한 currentState를 사용한다.

- Previous
- Next
- Slider
- Timeline direct jump
- Auto play
- View switch
- Perspective switch
- Component Inspector

---

# 16. 시나리오 범위

모든 항목을 억지로 넣지 말고 학습 목표와 위험에 맞는 최소 완전 세트를 선택한다.

검토 후보:

## 16.1 제품·화면

- 첫 진입
- 로딩
- 데이터 있음
- 데이터 없음
- 입력 오류
- 서버 오류
- 재시도
- 취소·뒤로 가기
- 권한 부족
- 역할·플랜 차이
- 모바일·데스크톱
- 키보드·화면 읽기
- Optimistic update 성공·실패
- 느린 네트워크·Offline

## 16.2 업무·정책

- 정상 승인
- 수동 검토
- 자동 차단
- 정책 A/B 비교
- 예외 처리
- 소유권·책임 이전
- 운영 Escalation
- SLA 초과

## 16.3 데이터·시스템

- 정상 저장
- Duplicate / Idempotency
- Stale / Out-of-order
- Concurrent update
- Transaction rollback
- Retry
- Terminal failure
- Replay
- Delayed feedback
- Feature flag OFF
- Catch-up
- External dependency failure
- Before/after migration
- Mixed-version compatibility

## 16.4 Evidence와 Release

- Local only
- CI
- Staging canary
- Production not approved
- Human approval required
- Safe rollback

안전하게 차단된 경로를 “기능 완료”로 표현하지 않는다.

---

# 17. Frontend·Design 특화 표현

화면 또는 UI Entity에는 다음 상태를 포함한다.

```text
Default
Hover
Focus
Disabled
Loading
Empty
Error
Success
```

각 화면에서 확인할 내용:

- 사용자 행동
- 콘텐츠와 오류 문구
- 입력값 유지 여부
- 재시도 방법
- Focus order
- Keyboard interaction
- Responsive 차이
- 분석 이벤트
- 접근성 상태
- Browser back/forward
- Deep link

Screen Flow는 단순 Screenshot 나열이 아니라 상태 전환 조건을 보여줘야 한다.

---

# 18. PM·Operations·Cross-team 특화 표현

Service Blueprint 또는 Swimlane에 다음 Lane을 사용할 수 있다.

```text
Customer Action
Visible Experience
Backstage Process
Operations
Support
Data / Analytics
External Partner
```

Handoff에는 다음을 표시한다.

- 무엇을 전달하는가
- 누가 다음 책임을 가지는가
- 승인 또는 차단 조건
- SLA
- 실패 시 Escalation
- 고객에게 보이는 안내
- 운영자가 확인할 근거

---

# 19. Standalone HTML 구현 규칙

기본값:

- Semantic HTML
- CSS custom properties
- Vanilla JavaScript
- HTML + SVG Hybrid
- Inline data model
- 외부 Font·Library·CDN 없음
- Build step 없음
- 서버 없이 파일을 직접 열어 실행 가능

코드 구조:

```text
Entity / Relation Catalog
→ Scenario Builder
→ State Engine
→ View Renderers
→ Interaction Controller
→ Zoom / Focus Controller
→ Accessibility Layer
→ Verification Hooks
```

피해야 할 구조:

- Domain 사실을 Render 함수 여러 곳에 복제
- Step마다 개별 DOM 조작
- View마다 다른 Scenario 데이터
- Inline onclick 남발
- 불안정한 랜덤 ID
- 외부 네트워크가 없으면 핵심 기능이 깨짐

---

# 20. 접근성

최소 요구사항:

- 모든 핵심 기능 키보드 조작 가능
- 명확한 Focus 표시
- 버튼과 Toggle의 Accessible Name
- `aria-pressed`, `aria-expanded` 등 상태 전달
- 현재 단계 변경을 Live Region으로 알림
- Tooltip을 Focus와 Touch로도 접근
- 핵심 시각 정보의 텍스트 설명
- 애니메이션 Pause 가능
- `prefers-reduced-motion` 지원
- 충분한 색 대비
- 충분한 Touch target
- Esc로 Dialog·Zoom·Drawer 닫기
- 논리적인 DOM 순서
- Hover-only 핵심 정보 금지

---

# 21. 반응형

## Desktop

- 좌우 패널 + 중앙 Canvas
- Hover Tooltip
- Zoom 2-column layout
- Focus mode

## Tablet

- 패널 축소 또는 Overlay
- Canvas horizontal pan
- Navbar 우선순위 조정

## Mobile

- 현재 단계와 핵심 결과 우선
- 패널은 Drawer / Bottom Sheet
- Entity detail은 Tap 기반
- Zoom은 단일 컬럼
- 가로 Diagram에는 명확한 Scroll affordance 또는 축약 View
- Lane 제목 가로 유지
- 한 손 조작 가능한 재생 컨트롤

데스크톱 UI를 단순 축소하지 않는다.

---

# 22. 보안·개인정보·성능

## 보안

- 사용자 입력을 그대로 `innerHTML`에 넣지 않는다.
- Dynamic text를 Escape한다.
- 임의 Script 실행 기능을 넣지 않는다.
- URL parameter는 Allowlist로 검증한다.
- 실제 개인정보, Token, 비밀 URL, 운영 Payload를 포함하지 않는다.

## 개인정보

예시는 Synthetic data를 사용한다.

다음 값을 로그·지표·예시로 노출하지 않는다.

- 이름
- 전화번호
- 주소
- 계정 ID
- 실제 주문·계약·결제 정보
- Secret
- Raw production payload

## 성능

- 필요 없는 모든 Node를 계속 Animation하지 않는다.
- 현재 Edge만 움직인다.
- Resize 연산을 Debounce한다.
- 대규모 Scenario는 필요한 View만 Render한다.
- Tooltip과 SVG 경로 계산을 매 frame 반복하지 않는다.
- 줌에서 관련 Entity 수를 제한하고 필요하면 pagination 또는 group을 사용한다.

---

# 23. Reference HTML과 이미지 사용 계약

## 23.1 우선순위

동일한 품질을 재현하려면 다음 순서가 가장 효과적이다.

```text
1. 실제 동작하는 Reference HTML
2. 상태별 Screenshot
3. 짧은 영상 또는 GIF
4. Master Instruction
5. Task-specific Brief
```

이미지 한 장만으로는 상태 변화와 인터랙션 계약을 알 수 없다.

## 23.2 Reference HTML이 주어졌을 때

Reference HTML을 단순 영감이 아니라 **실행 가능한 레이아웃·인터랙션 명세**로 취급한다.

```text
Reference의 Interaction Grammar
State Model
Responsive behavior
Accessibility semantics
Verification quality
```

를 유지한다.

처음부터 새로 디자인하지 않는다.

현재 주제에 맞게 다음을 바꾼다.

- Entity
- Relation
- Scenario
- Step
- State
- 문구
- 브랜드 색상
- 정보 밀도
- 적합한 View

기능을 제거할 때는 학습 목표에 불필요한 이유가 있어야 한다.

## 23.3 Screenshot 세트

권장 상태:

1. Desktop 기본 화면
2. 화면 집중 모드
3. 컴포넌트 줌인
4. 2-hop 또는 Downstream
5. 쉬운 설명 모드
6. 전문 설명 모드
7. Sequence View
8. Mobile Drawer
9. Error / Retry 상태

각 Screenshot에는 무엇을 검증하는지 설명을 붙인다.

---

# 24. 우선순위 계약

## P0 — 없으면 실패

- Standalone 단일 HTML
- 명확한 학습 목표
- Scenario와 Input
- Previous / Next / Slider
- 하나의 Scenario·Step·State 모델
- 현재 Source → Target 방향
- 현재 활성 요소의 명확한 구분
- 쉬운 설명과 전문 설명의 동기화
- 현재 영향 방향을 Canvas 외부에 배치
- 좌우 패널 독립 토글
- 화면 집중 모드와 상태 복원
- Entity Hover·Focus·Click·Touch
- Component Zoom과 강화 설명
- 줌 상태에서 단계 이동 시 먼저 줌아웃
- Mobile Drawer / Bottom Sheet
- Keyboard
- 정적·Browser Runtime 검증

## P1 — 목적에 맞으면 포함

- Auto play
- Playback speed
- Architecture / Sequence 전환
- Compare mode
- Failure injection
- State cards
- Execution log
- Evidence panel
- Related step / scenario jump

## P2 — 명확한 학습 목적이 있을 때만

- Pan / Zoom mini-map
- Export image / JSON
- Annotation
- Guided tour
- Quiz
- Bookmarkable URL state
- Role-only filter

---

# 25. Must-pass 상호작용 계약

| 사용자 동작 | 반드시 발생할 결과 |
|---|---|
| Next | 현재 단계 +1, 상태 재계산 |
| Previous | 현재 단계 -1, 상태 재계산 |
| Slider | 직접 이동해도 동일한 상태 |
| View 전환 | Scenario·Step·State 유지 |
| 관점 전환 | 사실·상태 유지, 문구만 변경 |
| 왼쪽 패널 토글 | 중앙 자동 확장 |
| 오른쪽 패널 토글 | 중앙 자동 확장 |
| 화면 집중 모드 | 양쪽 숨김, 해제 시 이전 상태 복원 |
| Entity Hover | 짧은 설명 |
| Entity Click | 재생 일시정지, 줌인, 관련 관계 표시 |
| 관계 범위 전환 | direct / 2-hop / downstream 갱신 |
| Zoom 관련 Entity 클릭 | 줌 대상 이동 |
| Zoom 중 Next/Previous/Slider | 먼저 줌아웃, 전체 복원 후 이동 |
| Zoom out | 선택·흐림 해제, 전체 흐름 복원 |
| Esc | 가장 위의 임시 UI부터 닫음 |
| Mobile Entity Touch | 상세 Bottom Sheet 또는 Zoom View |
| Resize / Panel change | 화살표·Tooltip 위치 재계산 |

---

# 26. 검증 계약

## 26.1 정적 검증

- HTML Parse 가능
- JavaScript syntax 통과
- 중복 HTML ID 0
- 모든 내부 Anchor 유효
- 모든 Entity가 Catalog에 존재
- 모든 Relation의 Source·Target 존재
- 모든 Step의 Entity·Relation 참조 존재
- Scenario·Step ID 중복 없음
- 의도하지 않은 외부 Dependency 없음
- Secret-like value 없음

## 26.2 Scenario Matrix

- 모든 기본 시나리오
- 모든 중요한 Boolean 조합
- 모든 Enum 옵션
- Failure injection
- 직접 Timeline jump
- Reset
- Previous / Next
- Auto play 중단
- View 전환
- Perspective 전환
- Zoom 상태 전환

## 26.3 Interaction Matrix

- Mouse
- Keyboard
- Touch
- Hover
- Focus
- Click / Pin
- Outside Click
- Esc
- Resize
- Scroll
- Drawer
- Reduced motion

## 26.4 Layout

- Fixed Navbar가 콘텐츠를 가리지 않음
- 현재 영향 방향이 Canvas를 침범하지 않음
- Source와 Target이 명확함
- 활성 Node가 충분히 강하게 보임
- 화살표가 Node를 관통하지 않음
- 라벨과 선이 겹치지 않음
- Lane 제목이 가로로 읽힘
- 패널 닫기 후 Canvas 확장
- Tooltip이 Viewport 밖으로 나가지 않음
- Zoom에서 관련 정보만 보임
- Zoom out 후 Dim 상태가 남지 않음

## 26.5 Browser Runtime

가능하면 실제 브라우저에서 확인한다.

- Desktop
- Tablet
- Mobile
- Console error 0
- Page error 0
- 모든 Scenario 직접 이동
- 모든 Entity Click
- 모든 Zoom 범위
- Zoom → Next 순서
- Drawer와 Backdrop
- Keyboard
- Reduced motion

브라우저를 사용할 수 없다면 가능한 최강의 정적 검사를 하고 한계를 명시한다.

렌더링 검증 없이 “브라우저 검증 완료”라고 주장하지 않는다.

## 26.6 Screenshot 비교

Reference Screenshot이 있다면 다음 차이를 측정하고 개선한다.

- 전체 Layout
- Navbar 높이
- 좌우 Panel 폭
- Canvas 밀도
- Current impact row 위치
- Active Node 강조
- Arrow clarity
- Zoom composition
- Mobile composition

초기 결과와 Reference의 가장 큰 차이 최소 3개를 실제 수정하고 다시 확인한다.

---

# 27. 자율 Self-review

첫 완성본 이후 다음 관점으로 전수 리뷰한다.

1. 처음 보는 사람
2. Product / PM
3. Product Designer / UX Researcher
4. Frontend Engineer
5. Backend / Data Engineer
6. QA / Operations
7. Accessibility Reviewer
8. Evidence / Release Reviewer

각 관점에서 질문한다.

## 처음 보는 사람

- 첫 화면에서 목적을 이해하는가?
- 현재 무엇이 시작되고 무엇이 바뀌는지 보이는가?
- 용어가 자연스러운가?

## PM

- 정책과 예외가 보이는가?
- 사용자 영향과 운영 위험을 비교할 수 있는가?

## Designer

- 화면 상태와 오류·재시도·접근성이 보이는가?
- 문구가 실제 사용자 언어인가?

## Frontend

- 이벤트·요청·상태·캐시·분석이 추적되는가?

## Backend/Data

- 저장·일관성·권한·Retry·Rollback 경계가 정확한가?

## QA/Ops

- 테스트 조합과 실패 대응을 만들 수 있는가?

## Accessibility

- 키보드와 Touch만으로 사용할 수 있는가?

## Evidence

- 구현과 검증 수준을 과장하지 않았는가?

가장 약한 부분을 실제 HTML에서 고친 뒤 영향을 받는 전체 검증을 다시 수행한다.

---

# 28. 피해야 할 안티패턴

- 모든 주제를 Backend Architecture로 표현
- PM·Designer에게 클래스와 DB 이름부터 노출
- 정적 다이어그램에 Next 버튼만 추가
- 모든 정보를 한 화면에 노출
- 현재 영향 방향을 Diagram 위에 Overlay
- 활성 Node가 단순히 조금 밝아 구분되지 않음
- 화살표에 동사가 없음
- 세로 회전 Lane 제목
- 중요한 정보를 Hover로만 제공
- View마다 다른 사실과 상태 사용
- 관점 전환 시 Step 초기화
- Zoom 상태에서 Flow가 계속 재생돼 화살표가 뒤섞임
- Zoom out 뒤 Focus dim이 남음
- 쉬운 설명이 기술 용어의 직역에 그침
- 미구현 미래 동작을 완료처럼 표시
- Local test를 Production proof로 표현
- 모바일에서 Desktop을 단순 축소
- 이유 없이 외부 Library 사용
- 실제 개인정보·Secret·운영 Payload 포함
- 기능 수를 늘리는 것을 품질로 착각

---

# 29. Definition of Done

다음 조건을 모두 충족해야 완료다.

- Learning Objective가 명확하고 충족된다.
- 대상 사용자가 자신의 말로 흐름을 설명할 수 있다.
- 인터랙션이 실제 이해를 바꾼다.
- 현재 Source·Target·행동·상태 변화가 명확하다.
- 쉬운 설명과 전문 설명이 같은 사실을 사용한다.
- 정상·경계·실패·복구 중 중요한 흐름이 포함된다.
- 구현·부분 구현·차단·제안·외부 검증 상태가 구분된다.
- 현재 영향 방향이 Canvas를 덮지 않는다.
- Active Node가 명확하게 구분된다.
- Component Zoom에서 관련 내용만 보이고 설명이 충분하다.
- Step 이동 전에 Zoom이 해제돼 전체 흐름이 복원된다.
- Desktop·Mobile·Keyboard·Touch가 사용 가능하다.
- 정적 검증과 가능한 Runtime 검증이 통과한다.
- 남은 위험과 Evidence gap이 명시된다.
- 최종 파일 경로를 실제로 확인하고 다운로드 링크를 제공한다.

---

# 30. 최종 결과 보고 형식

```text
Summary:
- 무엇을 만들었는가
- 누구를 위한 것인가
- 어떤 이해·결정 목표를 지원하는가

Answer:
- 파일 링크
- 주요 시나리오
- 주요 관점
- 핵심 인터랙션

Assumptions:
- 확정하지 못한 가정

Key Checks:
- 최신 기준 자료
- 정적 검증
- Scenario/Input Matrix
- Zoom/Panel/Flow 상호작용
- Desktop/Mobile Runtime
- Self-review 후 실제 개선

Risks:
- 미구현
- 외부 검증 필요
- Evidence 제한

Confidence: 0.0–1.0
Verdict: done / needs follow-up / blocked
```

---

# 31. 복사해서 사용할 수 있는 완성형 Master Prompt

```text
[주제/기능/제품/프로세스]를 대상 사용자가 직접 조작하며 완전히 이해할 수 있는
standalone 인터랙티브 HTML 설명서·시뮬레이터로 만들어라.

이 작업을 단순한 문서 작성이나 정적 다이어그램 제작으로 취급하지 마라.
Researcher, Domain Analyst, Product Thinker, Information Architect,
Interaction Designer, Visual Designer, Frontend Engineer,
Accessibility Reviewer, QA Engineer, Evidence Reviewer 역할을 맡아
자료 조사부터 모델링, 구현, 검증, 자체 리뷰와 개선까지 끝까지 책임져라.

## 목표
- 사용자가 무엇이 일어나고 왜 일어나는지 이해한다.
- 입력, 선택, 역할, 상태를 바꾸며 결과 차이를 직접 확인한다.
- 사용자에게 보이는 경험과 내부 처리·운영 영향을 연결한다.
- 구현, 가정, 제안, 부분 구현, 외부 검증 상태를 정확히 구분한다.

## 조사
- 최신 Source of Truth와 Authority Map을 정의한다.
- 최신 제품, 디자인, 코드, 스키마, 테스트, 분석, 운영 자료를 전수 조사한다.
- 실제 current head 또는 최신 기준을 확인한다.
- 충돌하는 자료는 권위와 최신성을 기준으로 해결한다.
- 알 수 없는 내용은 추측하지 않고 Assumption 또는 Proposal로 표시한다.

## 대상 사용자와 학습 목표
- Audience Matrix를 만든다.
- 각 역할이 무엇을 이해·결정·구현·검증해야 하는지 Observable Learning Objective로 정의한다.

## 인터랙티브 형식
학습 목표에 따라 User Journey, Screen Flow, Service Blueprint,
State Machine, Decision Tree, Before/After, Timeline, Data Lineage,
Sequence, Architecture, Incident Replay 중 가장 적합한 조합을 선택한다.
모든 주제를 Backend Architecture로 표현하지 않는다.

## 관점 모드
필요한 모드를 2개 이상 제공한다.
- 쉬운 설명 / 업무 흐름
- Product / PM
- UX / Design
- Frontend
- System / Backend
- Data / Operations
- QA / Release

모든 관점은 하나의 Scenario·Step·State 모델을 공유한다.

## 필수 상호작용 P0
- Fixed Navbar
- Reset / Previous / Play-Pause / Next
- Step Slider
- Scenario와 입력 조건
- View 전환
- 관점 전환
- 좌우 패널 독립 토글
- 화면 집중 모드와 이전 상태 복원
- 현재 Source → Action → Target 표시
- 현재 영향 방향을 Diagram 바깥의 독립 행에 배치
- Active Source·Target·Participant를 강하게 구분
- Entity Hover / Focus / Click / Touch
- Component Zoom
- Direct / 2-hop / Downstream 관계
- Zoom 강화 설명
- Zoom 상태에서 Step 이동 시 먼저 Zoom out 후 전체 흐름 복원
- Mobile Drawer / Bottom Sheet
- Keyboard와 Reduced Motion

## 문구
쉬운 설명은 실제 사람이 업무를 설명하듯 쓴다.
“A가 저장되면 관련 B와 C도 함께 갱신된다”처럼 원인과 결과를 명확히 설명한다.
기술 용어를 단순 번역하지 말고, 사용자에게 보이는 변화와 내부 변화를 구분한다.
전문 모드에서는 실제 클래스·API·이벤트·테이블·상태 이름을 정확하게 보여준다.

## 시각 표현
- 화살표에 동사형 라벨을 쓴다.
- 현재 관계만 강하게 강조한다.
- Lane 제목은 가로로 표시한다.
- 선이 Node·Label을 관통하지 않게 한다.
- Panel·Resize·Zoom 이후 Edge를 재계산한다.
- 색상만으로 상태를 전달하지 않는다.

## Component Zoom
컴포넌트를 클릭하면 자동 재생을 멈추고 전용 확대 화면을 연다.
선택 컴포넌트를 중앙에 두고 관련 입력·출력만 표시하며 무관한 내용은 숨기거나 강하게 흐린다.
왜 필요한지, 현재 단계 역할, 입력·출력, 읽기·쓰기, 소유권, 안전 규칙,
Transaction, 실패·복구, 관련 단계·시나리오, 근거, 한계를 설명한다.
Next/Previous/Slider/Timeline/Play를 누르면 Zoom을 먼저 닫고 Focus를 완전히 해제한 뒤
전체 Diagram이 복원된 다음 단계 이동을 시작한다.

## 데이터 모델
Entity Catalog, Relation Catalog, Scenario, Step, initialState, statePatch,
currentState, selectedEntityId, zoomedEntityId, focusScope를 분리한다.
Previous/Next/Slider는 initialState + statePatch로 상태를 재구성한다.

## 시나리오
현재 주제에 중요한 정상, 초기, 로딩, 빈 상태, 오류, 권한, 취소, 재시도,
중복, 오래된 상태, 동시 변경, 기능 OFF, Catch-up, Rollback, Recovery,
모바일, 접근성, 운영 승인, 외부 검증을 선택해 포함한다.

## 구현
외부 의존성이 없는 standalone 단일 HTML을 기본으로 한다.
Semantic HTML, CSS custom properties, Vanilla JS, SVG/HTML hybrid를 우선한다.
Data model, state engine, renderer, interaction, zoom controller, accessibility를 분리한다.
개인정보, Secret, 실제 운영 Payload를 포함하지 않는다.

## 검증
- HTML/JavaScript syntax
- duplicate ID = 0
- Entity/Relation/Step 참조 무결성
- 모든 Scenario와 중요한 Input 조합
- Previous/Next/Slider/Play 상태 재현
- View/Perspective 전환 상태 유지
- Panel/Focus mode 상태 복원
- 모든 Entity Click과 Zoom 범위
- Zoom → Next 순서
- Desktop/Tablet/Mobile
- Keyboard/Touch/Reduced motion
- Page error = 0
- Console error/warning = 0

Reference HTML이 제공되면 처음부터 새로 디자인하지 말고,
그 Interaction Grammar, State Model, Responsive behavior,
Accessibility와 Verification 품질을 실행 가능한 명세로 사용한다.

초기 구현 후 처음 보는 사용자, PM, Designer, Frontend, Backend/Data,
QA/Ops, Accessibility, Evidence 관점으로 전수 리뷰한다.
가장 큰 문제 최소 3개를 실제 HTML에서 수정하고 전체 검증을 다시 수행한다.

최종 파일 링크, 주요 상호작용, Scenario, 관점, 검증 결과,
Assumption, 미검증 위험, Confidence, Verdict를 보고한다.
```

---

# 32. 압축 Prompt

```text
[주제]를 여러 역할이 직접 조작하며 이해할 수 있는 standalone 인터랙티브 HTML로 만들어라.
최신 자료를 조사하고 Authority Map, Audience, Learning Objective, Entity, Relation, State, Scenario를 모델링한다.

학습 목표에 맞는 User Journey, Screen Flow, Service Blueprint, State Machine,
Decision Tree, Compare, Timeline, Data Lineage, Sequence, Architecture, Incident Replay를 선택한다.

Fixed Navbar, Previous/Next/Play/Slider, Scenario Inputs, 관점 전환, 좌우 패널,
화면 집중 모드, Canvas 외부의 현재 영향 방향, 명확한 Active Source/Target,
Hover/Focus/Click/Touch 설명, Component Zoom, direct/2-hop/downstream,
Zoom 강화 설명, Mobile Drawer를 구현한다.

Zoom 중 단계 이동을 요청하면 Zoom과 선택 상태를 먼저 완전히 해제하고
전체 흐름이 복원된 뒤 다음 단계로 이동한다.

쉬운 설명은 실제 사람이 인과관계를 설명하듯 쓰고,
전문 설명은 실제 구현 이름과 Transaction·Failure·Recovery를 정확히 보여준다.

모든 View와 관점은 하나의 Scenario·Step·State 모델을 공유한다.
정적 검증, 모든 중요 입력 조합, Browser Runtime, Keyboard, Touch,
Responsive, Reduced motion, Zoom→Next 순서를 검증한다.
여러 역할 관점으로 자체 리뷰하고 실제 개선한 최종본만 제공한다.
```

---

# 33. 예시 파일

이 지침과 함께 제공되는 예시 HTML:

```text
interactive-html-explainer-reference-example.html
```

예시는 다음 기능을 축약형으로 구현한다.

- 고객의 배송 주소 변경 시나리오
- 정상 변경 / 처리 시작 후 차단 / 알림 실패와 재시도
- 업무 흐름 / 구현 흐름
- Architecture / Sequence
- Fixed Navbar
- 좌우 패널과 화면 집중 모드
- Canvas 외부 현재 영향 방향
- 명확한 Source·Target·Participant 강조
- Timeline·State·Log
- Component Hover·Click·Zoom
- Direct / 2-hop / Downstream
- Zoom 강화 설명
- Zoom 중 Next를 누르면 먼저 전체 보기로 복귀
- Keyboard·Mobile·Reduced motion
- 외부 의존성 없는 단일 HTML

예시를 새 작업에 사용할 때는 도메인 문구만 바꾸지 말고 Entity·Relation·Scenario·State와 Evidence를 실제 대상에 맞게 다시 조사해 교체한다.

---

# 34. 최종 원칙

좋은 인터랙티브 설명서는 많은 박스와 애니메이션을 보여주는 화면이 아니다.

좋은 결과는 사용자가:

- 조건을 직접 바꾸고,
- 원인과 결과를 한 단계씩 따라가고,
- 자신의 역할에 맞는 관점으로 전환하고,
- 핵심 컴포넌트를 확대해 관련 내용만 깊게 보고,
- 전체 흐름으로 자연스럽게 돌아오고,
- 정상과 실패·복구를 비교하고,
- 무엇이 사실이고 무엇이 아직 미검증인지 구분하며,
- 최종적으로 자신의 말로 설명하거나 결정을 내릴 수 있게 한다.

최적화 대상은 박스 수, 글자 수, 기능 수가 아니다.

**사용자가 직접 조작한 뒤 정확히 이해하는 순간**을 최적화한다.
