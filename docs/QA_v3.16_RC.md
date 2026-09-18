# ECDIS Log Track Viewer v3.16 RC — Dense Print Basemap QA

Date: 2026-09-18

## Scope

This release candidate is a narrow correction on top of the recovered/working v3.15 RC baseline. Parser, time-zone conversion, data-integrity guard, screen-track rendering, OpenFreeMap screen basemap, offline coastline fallback, long-log LOD/cache logic, route geometry, statistics, and normal print semantics are intentionally preserved.

## User-observed defect

Normal print displayed the OpenFreeMap basemap, while **DENSE PRINT** fell back to the offline coastline-only image. The fallback avoided a blank page but did not meet the purpose of dense/high-detail printing.

## Correction

Dense OpenFreeMap capture no longer depends on one large 2400×1297 MapLibre/WebGL render target. v3.16 builds the same 2400×1297 print basemap as a **2×2 overlapped mosaic**:

- final dense basemap: 2400×1297 px;
- four approximately half-page MapLibre captures;
- 72 px capture overlap around each quadrant;
- each panel is rendered at the same geographic pixel density as the dense route canvas;
- panels are cropped to their central target areas and composited into one PNG before printing;
- the process waits for style/tile readiness after every camera move;
- button text reports `YOĞUN ALTLIK 1/4 … 4/4` while the mosaic is prepared;
- if any online panel cannot be completed, the existing full-page Natural Earth coastline fallback remains in place instead of producing an empty basemap.

Normal print still uses a single lower-resolution OpenFreeMap capture, but readiness waiting was made more robust.

## Why this design

The prior dense path asked MapLibre/WebGL for a single large off-screen surface. The user's browser successfully produced the normal 1200×649 capture but not the 2400×1297 dense capture. Splitting the dense image into four smaller render targets keeps each WebGL surface close to the already-working normal-print scale while preserving the final dense output resolution.

## Static regression checks

- JavaScript syntax (`node --check` on extracted inline script): PASS.
- HTML IDs: 76 / 76 unique.
- stale v3.10–v3.15 visible/source version tokens: 0.
- direct `tile.openstreetmap.org` occurrences: 0.
- dense mosaic function present: PASS.
- online-attribution detection covers both normal OpenFreeMap and dense mosaic provider strings: PASS.
- canonical candidate and named v3.16 RC are byte-identical.

## Environment limitation

This execution environment blocks local browser navigation and external map-network access under its browser policy, so an authoritative live OpenFreeMap dense-print capture cannot be reproduced here. The final acceptance test therefore remains the user's normal browser/network test with a real ECDIS log package. No claim is made that this live-network check was completed here.

## Diff scope

Unified diff: `10` added line(s), `17` deleted line(s) in the minified/single-file source representation. The substantive code change is confined to version markers and print-basemap preparation/capture logic.

## SHA-256

`8c5fe385f390a4a283534b01a764ca86f2b1cb06f8e0344cfa25ceb2244664ed`  `ECDIS_Log_Track_Viewer_v3.16_RC.html`