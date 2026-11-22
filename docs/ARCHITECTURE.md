# Architecture — SeoulMarketers

## Components
`apps/web` Next.js + TypeScript + R3F + 2D MapLibre
`services/etl` Python loaders, data quality rules
`services/api` versioned read API, bounded spatial queries
`db` PostgreSQL/PostGIS: public snapshot schema + private tenant-separated schema only after auth/privacy review.

## Logical endpoints
`GET /v1/districts?bbox=...`
`GET /v1/districts/{id}/categories?as_of=...`
`GET /v1/businesses?bbox=...&category=...&cursor=...`
`POST /v1/scenarios/evaluate` validates assumptions and returns transparent arithmetic, not secret success probability.
`GET /v1/data-freshness`
Never accept freeform SQL or unbounded bbox.

## Scale/performance
Tile/vector result cache, PostGIS GiST indexes, viewport clustering, pagination; static SEO region/category summary routes. Three renderer separate dynamic bundle from search snippets. Cache invalidation keyed by snapshot version.

## Vercel + home PC
At MVP use precomputed public GeoJSON/aggregates for statically rendered site; home PC refreshes snapshots. For interactive queries, a hardened API with HTTPS may be added. Do not expose home DB directly or rely on PC uptime for homepage.

## Safety / security
Server-side secrets, data minimization, geographic precision, tenant RBAC, proper consent for business private data, abuse/report process for community, moderation queue, no public admin route, SSRF/rate-limit/injection tests. No arbitrary uploaded 3D assets until file scanning strategy exists.

## Constraints
Compare WebGL support; 2D fallback always functional. Analytics collection subject to notice/consent as applicable. User engagement should measure completed tasks and return value, not infinite session length.
