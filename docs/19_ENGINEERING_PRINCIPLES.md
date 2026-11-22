# 19. Engineering Principles — SeoulMarketers

## 대상 독자
기술 리뷰 경험이 거의 없는 창업자. 구현자는 AI Agent. 품질 책임은 Agent와 CI에 있으며 사용자의 '승인'을 테스트 대용으로 요구하지 않는다.

## 근거 글
- [Toss: 선언적인 코드 작성하기](https://toss.tech/article/frontend-declarative-code)
- [Frontend Fundamentals: 좋은 코드를 위한 4가지 기준](https://frontend-fundamentals.com/code-quality/code/)
본문을 복제하지 않고 핵심 원칙을 프로젝트에 맞게 재해석한다.

## 코드 품질의 4축
1. 가독성: 파일·함수명으로 '무엇'이 보인다. 맥락 전환 최소화, 복잡한 조건에 명칭 부여, 숫자 상수에 의미 있는 이름.
2. 예측가능성: 동일 함수는 동일한 입력에서 같은 결과. 일관된 반환 타입, 오류 결과 명시, 숨은 네트워크 요청 금지.
3. 응집도: 같이 수정해야 하는 규칙·타입·테스트는 가까이 배치. 동일한 계산식이 여러 곳에 복제되지 않도록 책임자 하나 유지.
4. 결합도: UI가 DB SQL을 모르고, 도메인 규칙은 React/Three.js를 모른다. 외부 API는 adapter/port로 격리. 그러나 추상화를 위한 추상화는 만들지 않는다.

## 선언적 코드 규약
- 화면 컴포넌트는 '로딩/빈 결과/오류/정상' 상태를 명확하게 표현한다.
- 데이터를 화면에서 직접 계산하지 말고 도메인 함수로 분리한다.
- 조건문 길이보다 읽는 사람이 이해할 맥락을 줄이는 것을 목표로 한다.
- UI 추상화는 실제 중복/변경 패턴이 확인되면 도입한다. 만능 공통 컴포넌트 금지.
- 비즈니스 규칙은 이름 있는 순수 함수와 타입으로 정의, 외부 I/O는 repository adapter로 분리한다.
- 변경이 발생하면 문서→테스트→구현→증거 순서로 갱신한다.

## 예시: Presentation과 Domain 분리
```ts
type DataState<T> =
  | { status: "loading" }
  | { status: "error"; reason: string }
  | { status: "empty" }
  | { status: "ready"; value: T };

function isPublishable<T>(state: DataState<T>): state is Extract<DataState<T>, {status: "ready"}> {
  return state.status === "ready";
}
// 상태 판정과 도메인 계산을 컴포넌트 밖에서 검증.
```

## 금지 사례
- Three.js Canvas 안에 공공데이터 fetch/비즈니스 집계/요금 계산 몰아넣기
- 무의미한 Manager/Helper/Util 계층 증가
- 통계 결측을 0으로 바꾸기
- 실패한 테스트를 삭제하거나 기대값을 구현에 맞게 무단 수정
- 화면 데모 데이터를 실제 공공 기록처럼 보이게 하기
- Agent가 구현 불확실성을 사용자에게 기술 승인만 요청하고 넘기기

## 예외와 ADR
위 원칙의 충돌은 ADR로 선택·이유·대안·영향·되돌림을 기록한다.
