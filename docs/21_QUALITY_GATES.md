# 21. Objective Quality Gates & Evidence

## '사용자 승인' 대신 객관적 통과 기준
| Gate | 자동 검증 | 수동/전문 검증 | 전환 조건 |
|---|---|---|---|
| Q0 요구사항 | ID, 테스트 가능 문장, 의존성 검사 | 제품 범위만 선택 | Open Issue 구분 |
| Q1 데이터 | 계약 Fixture, 결측/중복/스키마 테스트 | 공식 원본 소량 대조 | 핵심 출처 실증 |
| Q2 도메인 | 정상/경계/오류 TDD | 계산식 수기 예제 | 함수 재현성 |
| Q3 통합 | DB/API integration, idempotence | 출처 비교 샘플 | 정합성 |
| Q4 UI | 컴포넌트/E2E/접근성 | 사용성 피드백 | 핵심 과업 완료 |
| Q5 운영 | 빌드/비밀키/장애/복원 | 서비스 정책 | 배포 후보 |
| Q6 출시 | 모니터링, rollback, health | 출시 범위 결정 | 공개 가능 |

## Evidence Manifest 예시
```yaml
task: SM-001
requirement_ids: []
commit_sha: null
red:
  command: null
  exit_code: null
  log_path: null
green:
  command: null
  exit_code: null
  log_path: null
ci_url: null
preview_url: null
dataset_snapshot: null
manual_source_sample_ids: []
known_issues: []
owner_learning_doc: null
```

## 최소 범위
- 정상·경계·결측·실패 4종 테스트. 신규 규칙별 1개 이상.
- 네트워크·DB 없는 순수 Domain 테스트는 모든 PR에서 빠르게 실행.
- DB 마이그레이션은 재현 가능한 새 DB에서 검증하고 이전 버전 복귀 계획을 기록.
- 3D UI는 접근 가능한 2D 대체 화면, WebGL 실패 대체 UI 필수.
- 테스트 결과 없이 완성/배포되었다고 쓰지 않는다.
- coverage 숫자만으로 품질을 주장하지 않는다. 핵심 규칙·오류 경로를 우선한다.
