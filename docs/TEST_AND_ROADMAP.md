# Tests & Parallel Roadmap — SeoulMarketers

## Data tests
Bad/empty coords, multiple businesses same address, category code drift, cross-boundary business, stale upstream, provider deletion, area CRS accuracy. Count aggregation stable for identical snapshots. Shop lifecycle not inferred from disappearance alone.

## Scenario logic tests
Revenue/rent/wages units and boundaries; ratio 0 or 1; zero revenue; sensitivity monotonicity; tenant isolation; stored assumptions reproducible; 'unknown' vs 0; only explanatory comparisons, no definitive success forecast.

## 3D/UX tests
Visual regression for 3D demo and 2D, frame budget, memory-dispose/reenter 20 times, mobile touch, keyboard navigation, no-WebGL fallback, reduced-motion, search crawler with JS disabled, URL state restore.

## Release gates
G0 public source license verified; 2 district coverage.
G1 geospatial regression fixtures + 100 sampled shops manually crosschecked.
G2 3D and 2D functional, data never hidden by canvas.
G3 scenario inputs/reports with explicit assumptions and disclaimers.
G4 beta interviews (prospective entrepreneurs & existing merchants); evidence of repeated decision use.

## Sprint plan
S0 data contract + UI storyboard; S1 ETL & 2D basemap; S2 3D street prototype; S3 filters, simulation, SEO; S4 merchant interview beta. 3D visual polish must not delay correct statistics.

## Success
scenario completion, compare save, 7d revisit, report feedback, source data error rate, mobile performance. Don't maximize raw time spent in app as sole objective.
