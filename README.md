# SeoulMarketers

상권 데이터 + **Three.js 3D 도시 탐색** + 실제 비용·경쟁 구조의 시뮬레이션. 예비 창업자와 기존 사업자를 위한 서비스. 3D는 첫 인상, 검증 가능한 분석이 제품의 신뢰를 만든다.

## 문서
- [제품 요구사항](docs/PRODUCT_REQUIREMENTS.md)
- [데이터·비즈니스 로직](docs/DATA_AND_BUSINESS_LOGIC.md)
- [Three.js 3D UX](docs/THREEJS_UX.md)
- [시스템 아키텍처](docs/ARCHITECTURE.md)
- [검증·로드맵](docs/TEST_AND_ROADMAP.md)

## 초기 범위
서울 일부 지역의 상권·업종 탐색 → 비용 가정 시뮬레이터 → 등록 사업자용 리포트 실험. 공개 데이터만으로 실제 개별 매장 매출·창업 성공 확률을 알 수 없음. 산출물은 의사결정 지원이지 보장 또는 개별 전문 자문이 아니다.

## 배포
Next.js + TypeScript + React Three Fiber/Three.js; PostGIS + Python ETL; Vercel preview, Ubuntu batch. 개인정보/매출자료는 별도 동의·보호 체계 이후에만 연동.

현재 문서 중심 시작 상태이며 구현 여부를 뜻하지 않는다.
