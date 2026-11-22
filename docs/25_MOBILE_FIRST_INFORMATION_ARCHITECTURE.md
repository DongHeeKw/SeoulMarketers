# Mobile-first Information Architecture & Independent App Boundary (v1.0)

> Status: product architecture specification. The attached visual mockups are **desktop concept references**, not literal requirements to cram every panel into a mobile screen.

## 0. Product identity
**SeoulMarketers is its own independent application, brand, GitHub repository, frontend, navigation, release pipeline, domain deployment, data contract and user journey.** Do not merge routes, databases, authentication sessions, product analytics or UI branding with the other two products simply because they have a common author. A reusable package may be copied or versioned only if a real shared requirement is demonstrated and its compatibility is tested.

3D 도시를 첫 진입의 시각적 매력으로 활용하되, 지역→업종→분석→사업 시나리오를 각각 독립된 화면으로 나눈다. 사용자가 3D를 꺼도 동등한 핵심 정보를 얻을 수 있어야 한다.

## 1. Decision architecture
Choose progressively: one primary task per mobile screen; reveal details only when requested. Do not render the desktop dashboard at 40% width. "Progressive disclosure" means routes/states, not hidden content that is inaccessible on mobile. A map is not a substitute for an accessible information list.

## 2. Page-by-page journey
| Stage | Page | Mobile primary content | Proposed route |
|---|---|---|---|
| 01 | 세계관·진입 | 가벼운 3D 미니어처 도시와 CTA 하나: '상권 탐험 시작'. 모바일 저사양에서는 정적 포스터. | `/` |
| 02 | 지역 선택 | 지도에서 서울 지역 선택 또는 주소/지역 검색. 선택된 지역만 다음 페이지로 전달. | `/explore` |
| 03 | 업종 선택 | 카페·음식점·교육·서비스 등 업종 선택. 분류 체계와 선택 범위 안내. | `/explore/category` |
| 04 | 상권 요약 | 선택 지역의 공개 점포 분포·경쟁 밀도·데이터 기준일. 핵심 숫자 3개 이내, 상세 분석은 다음 화면. | `/districts/[id]` |
| 05 | 사업 시뮬레이션 | 월매출·임대료·인건비 등 **사용자가 입력한 가정**으로 손익 시나리오를 계산. 예측값처럼 포장하지 않는다. | `/simulate` |
| 06 | 결과·비교 | 가정과 민감도, 다른 지역과 비교, 저장/공유. 근거 및 데이터 공백 표시. | `/reports/[scenarioId]` |
| 07 | 사장님 도구 | 동의 기반 업무 데이터, 주변 상권 변화, 구독 기능. 향후 별도 인증/테넌트. | `/merchant` |

홈은 3D 첫인상 + 지역 검색 CTA까지만. 기존 예시 이미지 속 유동인구·매출·순이익·경쟁 점포 목록은 한 화면에 두지 않고 단계별 상세·리포트로 분리. 3D는 explore에 집중시키고 차트/수치는 Next.js DOM 접근성 UI에서 표현.

## 3. Three distinct responsive compositions
| Viewport guidance | Composition | Navigation | Data visibility |
|---|---|---|---|
| Mobile <= 599 CSS px | Single-column, 1 primary CTA, stacked wizard, separate detail route | Bottom nav; native back / persistent route params | 1 main metric group, progressive expansion; full-screen or sheet filters |
| Compact tablet 600–1023 px | Two panes when usable; otherwise mobile stack | Sidebar or tabs | Selected item + one detail panel |
| Desktop >= 1024 px | 2–3 column explorer/detail/analysis workspace | Sidebar or top nav | Rich summary panels, but still grouped by task |
These widths are initial design tokens, not a hard breakpoint to force layout on all devices. Also respond to container width, zoom and touch/hover capabilities. Portrait tablet may use mobile behavior.

## 4. Route-state contract
- Refresh and deep-link must preserve region/category/search/filter/selected item via safe URL query params or persistent session state.
- Browser back returns to the exact previous step with prior selections retained, not back to home.
- No invalid ID silently redirects to an unrelated listing. Offer explicit recovery.
- Loading/empty/error/stale/permission-denied states have unique screens and tests.
- Shareable pages contain server-rendered explanatory content for SEO, not solely WebGL canvas.
- Auth gates only for private personalized features; public analysis stays browsable without signup.

## 5. Design reference migration
- Images in `assets/` or `docs/` should be cataloged as `desktop-concept`, `mobile-flow`, `tablet-split-view`, `3d-scene-reference`.
- Existing beautiful desktop mockups define palette, atmosphere, card treatment, typography, material, and visual hierarchy. Their multiple panels must be mapped into **separate mobile routes**.
- UI copyright/assets: only use images actually present in repo or licensed content, never treat generated demo numbers as real data.
- Mobile illustration must be planned for each key stage independently. Day/night modes change atmospheric lighting, not data truth or navigation structure.

## 6. Agent implementation sequence and TDD
1. Write route-state tests for stage progression, back navigation, and preservation of query params.
2. Implement mobile component skeletons and loading/empty/error states; Playwright viewport screenshots.
3. Add tablet split layout and desktop workspace **without duplicate business logic**.
4. Link domain layer through typed view models. Business rules are independent of CSS or Three.js.
5. Add 3D as an enhancement where applicable with tested 2D fallback and WebGL disabled scenario.
6. Add accessibility tests for keyboard, screen readers, focus order, reduced motion, dynamic zoom up to 200%.
7. Visual regression at widths 375, 430, 768, 1024, 1440 CSS px; use curated stable fixtures.
8. Regression: switching viewport must not lose user-selected state; DOM and selected 3D feature ID match.

### Acceptance scenarios
- Starting on a 375px phone, the user can perform the full journey without horizontal scrolling and without needing dashboard knowledge.
- The user can exit/re-enter each step and retrieve prior selection through route state.
- Critical factual data and source links are readable with no WebGL.
- Desktop supports simultaneous panels but each panel is purposeful, not obligatory.
- No product routes or names from the other two apps appear in this app's main navigation.
- Analytics measure task completion and useful return visits, not time wasted in multi-panel complexity.

## 7. Navigation rules
Mobile bottom nav: 탐색 / 비교 / 내 시나리오 / 사장님. 시뮬레이션에서는 단계형 입력(위치→업종→비용→결과), 단계 진행과 뒤로가기를 지원.

## 8. Business safety / correctness
실매출·폐업·성공확률은 공개 상가 위치 데이터만으로 알 수 없음. 3D의 실제 건물 지리적 정밀도는 공식 라이선스 자료·좌표 검증 전에는 시각화/DEMO로 표시. 2D 대비 기능 동등성을 테스트.

## 9. Engineering ownership
No technical approval required from the founder. Agent supplies tests, screenshots, performance reports, architecture rationale and teachable file-reading order. Founder reviews the **experience** and business direction; CI and source data checks verify implementation correctness. Do not claim a user preview or WebGL test passed before running it.

## 10. Work packages
- UX-IA-001: independent routing / page inventory / route graph / breadcrumb specification
- UX-IA-002: mobile shell and navigation + Playwright back/forward tests
- UX-IA-003: each screen one goal: search/select/detail/report component suite
- UX-IA-004: tablet split-view
- UX-IA-005: desktop concept decomposition
- UX-IA-006: 3D adaptive rendering + fallback where applicable
- UX-IA-007: responsive + accessibility regression + screenshots
Each PR contains a task-focused demo, test-first evidence, and learning guide.
