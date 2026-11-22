# 22. Step-by-step Implementation WBS — SeoulMarketers

## 순서 규칙
각 작업이 1개 PR과 1개 학습 레슨에 대응한다. 작업은 1~3개의 사용자 관찰 가능 결과를 가진다. 작업 완료 전 다음 의존 작업을 시작할 수 없으나 다른 트랙 독립 작업은 가능하다.

| ID | 작업 | 완료 증거 | 창업자가 학습할 개념 |
|---|---|---|---|
| SM-000 | spec + README + ADR + task board | 문서 링크/위험표 | 레포 구조 |
| SM-001 | package/linters/test runner/CI | green pipeline | npm/Python test |
| SM-002 | sample Fixture and source adapter | contract test, fake/real 구분 | API JSON |
| SM-003 | domain types, pure functions | RED/GREEN logs | 함수·타입 |
| SM-004 | SQL migration and persistence adapter | DB integration | table/key/index |
| SM-005 | query service + pagination | API/e2e | request/response |
| SM-006 | visible frontend first slice | screenshot + navigation | Next.js routing |
| SM-007 | core business analysis | sample arithmetic tests | algorithm |
| SM-008 | data/UI integration | compare to upstream samples | fullstack flow |
| SM-009 | comparison / save / explain | task-completion e2e | state |
| SM-010 | SEO, sitemap, accessible routes | indexability smoke | SEO |
| SM-011 | fault injection, retry/cache | outage replay logs | reliability |
| SM-012 | preview, security, performance, deploy | audited checklist | operations |

## 각 PR 리뷰 순서
1. 3줄 무엇을 만든 것인지 읽기
2. 설명된 명령으로 테스트를 실행하고 출력 확인
3. 브라우저에서 정해진 시나리오 실행
4. 가이드에서 지정한 세 파일을 위에서 아래로 읽기
5. 이해가 안 가는 줄에 질문 남기기 — 제품 배포 승인에 기술 지식을 요구하지 않음

## 완료 기준
구현 주장과 실제 증거를 분리하여 보고. 외부 API 미승인 항목은 mock 데모를 실데이터 배포로 바꾸지 않는다.

## 제품 개요
3D 도시 → 지역·업종 선택 → 근거 확인 → 시나리오 비교
