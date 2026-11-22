# Three.js City UX — SeoulMarketers

## Design intent
First impression: isometric Seoul street miniature, approachable game-like city. **3D is the entry layer; useful decisions are explicit and accessible in 2D.** No fabricated representation of individual real stores without reliable geolocation and appropriate rights.

## Scene hierarchy
`CityScene > TerrainGrid > Roads > BuildingInstancedMeshes > DistrictOverlays > StoreCategoryMarkers > HoverSelection > CameraController`.
Scene region tiles loaded based on viewport, spatial LOD, screen-space labels (HTML overlay). Never render an entire metropolitan area's building meshes at once.

## User journey
1. Intro 3D city preview with '지역 선택'.
2. Pan/zoom and tap region; info panel immediately loads official counts and last-updated.
3. Choose a category; competing shops highlighted.
4. Tap '이 업종 창업하기' → costs input → comparison report.
5. Save/share scenario; optionally explore alternative regions.
3D interactions must never block data exploration on low power mobile devices.

## Implementation
Next.js App Router + client-only Canvas (R3F), Three.js geometry & InstancedMesh for repeated buildings, MapLibre GL for geospatial basemap/2D fallback. Avoid using unlicensed map tiles or Google Earth imagery. Source coordinates and district polygons must be documented. Dynamic import canvas with SSR disabled; surrounding SEO text SSR.

## Performance budgets (hypotheses, verify)
First meaningful textual data <= 2.5s on test baseline; scene interactive <= 5s target on midrange Android. Limit draw calls, texture memory, GPU usage; use LOD/culling, devicePixelRatio clamp and reduced motion. Pause render loops when hidden; dispose materials/geometries/textures on unmount; renderer.info leak tests.

## Accessibility
Pointer + keyboard + touch controls, focus management, alternative 2D list, contrast, reduced-motion toggle, static fallback for no WebGL, screen-reader item tables. 3D doesn't own necessary factual content.

## Authenticity
Building geometry is a stylized visualization; include '시각화된 모델' badge within scene description, not exact 3D survey. Published count always linked to source snapshot. Avoid implying game simulation equals real financial prediction.

## Visual iterations
V0 mock data scene with explicit DEMO label → V1 2 Seoul districts real geo polygons → V2 category highlights → V3 scenario animation → V4 mobile polish. Each acceptance requires task-based usability, not only screenshot beauty.
