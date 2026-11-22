# Data & Business Logic — SeoulMarketers

## Sources
- 상가 업소 위치·업종: https://www.data.go.kr/data/15012005/openapi.do
- 상업업무용 **매매** 실거래: https://www.data.go.kr/data/15126463/openapi.do
- 추가 인구/유동/카드매출 데이터는 이용허락, 공개범위, 정확도 확인 전 'planned' 상태.
**매매 실거래 데이터를 월세 데이터라고 표시하면 안 된다.**
상가업소 목록만으로 실제 창업 성공률이나 점포 매출은 산정할 수 없다.

## Schema
`business_unit(source_id,unit_id,name,category_code,lat,lon,geom,observed_at,status_confidence)`
`category_mapping(source_category,normalized_category,mapping_version)`
`district(district_id,boundary,crs,version)`
`district_snapshot(district_id,period,category,count,source_coverage)`
`property_sale(source_id,region_code,contract_month,asset_type,amount,location_precision)`
`scenario(id,region,category,user_revenue_assumption,rent_assumption,variable_cost_ratio,fixed_cost_assumption,created_at)`
`user_workspace(user_id,consent_version,retention_policy)` — only after privacy design.

## Geometry
Use EPSG:4326 storage; geography/appropriate projected CRS for accurate distances; spatial indexes (GiST). For point-in-polygon use ST_Intersects with boundary policy; density = eligible businesses / area_km2. Don't confuse operational presence with officially licensed status. Snapshot changes do not equal confirmed opening/closure without validated lifecycle feed.

## Simulation engine
Inputs: selection zone, category, **user-entered** monthly revenue, estimated rent (explicitly assumed), materials %, staff costs, fixed costs, tax placeholder. Output: scenario cashflow range based on chosen assumptions, break-even monthly sales under assumptions.
`monthly_contribution = assumed_revenue * (1 - variable_cost_ratio)`
`operating_result = monthly_contribution - rent - wages - other_fixed`.
Range via scenarios low/base/high only; not probabilities or guarantees. Validate ratios 0..1, no NaN/negative malformed inputs, monetary unit KRW consistent.

## 'Recommended industry' engine
Start non-predictive: explainable opportunity comparison based on competition density, accessibility and user constraints. Configurable weights & evidence; source missing => omit factor and downrank completeness, never fabricate. High shop density can indicate demand **or competition**; do not label as guaranteed opportunity.
Later predictive ML only after ground-truth outcomes and time-split out-of-sample evaluation. Leakage check, calibration, subgroup error, drift monitoring, abstain if confidence poor.

## Update pipeline
Provider contract → parse → normalize category/address → dedup by source store ID with history → publish verified snapshots → invalidate tiles. Persist ingest time, upstream data date, coverage, join-confidence, origin URL. Deleted upstream item not automatically 'closed'.

## Subscriptions & privacy
Separate public market data and private merchant operational data; tenant isolation; explicit consent, data portability/deletion, role-based access, encryption at rest, no leaking a merchant's revenue into neighboring shop maps. No unverified claims about competitors.

## Quality KPIs
geocode validity, category mapping coverage, location outliers, area denominator, data snapshot consistency, user scenario completion, compare report saved, shop report usefulness feedback.
