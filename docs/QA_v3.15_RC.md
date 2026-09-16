# ECDIS Log Track Viewer v3.15 RC — Regression QA / Regresyon Kalite Raporu

## English

### Scope

This release candidate is deliberately based on v3.14 RC. It does not redesign the parser, UTC/fixed-offset handling, data-integrity guard, 20 kn default plausibility threshold, long-log LOD/cache optimizations, OpenFreeMap primary basemap, offline coastline fallback or print pipeline.

### Screen-track regression fixed

Observed in v3.14 RC: the track was present in print/PDF but disappeared from the interactive screen once the OpenFreeMap/MapLibre layer became visible.

Root cause: the online basemap child had an explicit positive z-index while the route canvas and pointer interaction layer used `z-index:auto`. The MapLibre canvas could therefore paint above the route canvas when the online map loaded. The offline coastline phase appeared correct, then the route became obscured when the online layer faded in.

Fix:

- `mapwrap` now establishes an isolated stacking context;
- tile pane: z-index 0;
- offline/online basemap: z-index 0/1 inside the tile pane;
- route `trackcanvas`: z-index 4;
- interaction layer: z-index 5;
- existing watermark/panels/attribution remain above these layers.

No route geometry, coordinates or source records are altered by this change.

### Automated regression

Using the real development test package previously used in this project:

- usable map records: **90,820**;
- screen LOD/path cache retained: **8,842 render vertices**;
- route canvas contains rendered pixels: **PASS**;
- simulated opaque online basemap at production stacking level: route remained visible above it: **PASS**;
- runtime page errors: **0**;
- normal print-track construction: approximately **337 ms** in the test container;
- print canvas contains rendered route pixels: **PASS**;
- JavaScript syntax (`node --check`): **PASS**;
- HTML IDs: **76/76 unique**.

### Long-log context retained from earlier RC work

The v3.15 RC checkpoint preserves earlier fixes/optimizations:

- removal of large spread-argument min/max calls that caused `Maximum call stack size exceeded` on large logs;
- iterative statistics;
- speed-class Path2D rendering;
- zoom-level geometry cache;
- `requestAnimationFrame` pan redraw coalescing;
- display-only LOD without modifying source CCRS data;
- canvas-based print-track rendering;
- OpenFreeMap/MapLibre primary detailed basemap;
- embedded Natural Earth/GSHHG coastline fallback;
- default maximum plausible speed: **20 kn**.

### Version consistency

Visible/browser/print version markers are `v3.15 RC`. The stale static `v3.12` print-kicker token was removed.

### Release status

`v3.15 RC` is published as a **development checkpoint / release candidate**, not as a final stable declaration. Further work remains recorded in `ROADMAP.md`.

---

## Türkçe

### Kapsam

Bu release candidate bilinçli olarak v3.14 RC üzerine kurulmuştur. Parser, UTC/sabit-ofset zaman mantığı, veri bütünlüğü koruması, 20 kn varsayılan makul hız eşiği, uzun-log LOD/cache optimizasyonları, OpenFreeMap ana harita, offline coastline fallback veya baskı pipeline'ı yeniden tasarlanmamıştır.

### Ekranda izin kaybolması regresyonu

v3.14 RC'de görülen durum: iz baskı/PDF içinde mevcutken OpenFreeMap/MapLibre katmanı görünür olduğunda etkileşimli ekrandaki iz kaybolabiliyordu.

Kök neden: online basemap child açık pozitif z-index kullanırken rota canvas'ı ve pointer interaction layer `z-index:auto` durumundaydı. Böylece online MapLibre canvas yüklenince rota canvas'ının üstüne çıkabiliyordu. Önce offline coastline ile iz doğru görünürken, online katman açıldığında iz örtülüyordu.

Düzeltme:

- `mapwrap` izole stacking context oluşturuyor;
- tile pane: z-index 0;
- offline/online basemap: tile pane içinde z-index 0/1;
- rota `trackcanvas`: z-index 4;
- interaction layer: z-index 5;
- watermark/panel/attribution katmanları bunların üzerinde kalıyor.

Bu değişiklik rota geometrisini, koordinatları veya kaynak kayıtları değiştirmez.

### Otomatik regresyon

Projede daha önce kullanılan gerçek geliştirme test paketiyle:

- kullanılabilir harita kaydı: **90.820**;
- ekran LOD/path cache: **8.842 render vertex**;
- rota canvas'ında gerçek çizim pixel'leri: **PASS**;
- üretim katman seviyesinde opak online basemap simülasyonu: rota üzerinde görünür kaldı: **PASS**;
- runtime page error: **0**;
- normal baskı rota katmanı hazırlığı: test container'ında yaklaşık **337 ms**;
- baskı canvas'ında rota pixel'leri: **PASS**;
- JavaScript syntax (`node --check`): **PASS**;
- HTML ID: **76/76 benzersiz**.

### Önceki RC çalışmalarından korunan uzun-log kazanımları

v3.15 RC aşağıdaki önceki kazanımları korur:

- büyük loglarda `Maximum call stack size exceeded` oluşturan büyük spread-argument min/max çağrılarının kaldırılması;
- iteratif istatistik hesabı;
- hız sınıfına göre Path2D render;
- zoom-bazlı geometry cache;
- pan redraw işlemlerinin `requestAnimationFrame` ile birleştirilmesi;
- kaynak CCRS verisini değiştirmeyen yalnız-görsel LOD;
- canvas tabanlı baskı izi;
- ayrıntılı ana altlık olarak OpenFreeMap/MapLibre;
- embedded Natural Earth/GSHHG coastline fallback;
- varsayılan azami makul hız: **20 kn**.

### Sürüm tutarlılığı

Tarayıcı, ekran ve baskı sürüm işaretleri `v3.15 RC` olarak tutarlıdır. Eski statik `v3.12` print-kicker ifadesi kaldırılmıştır.

### Yayın statüsü

`v3.15 RC` **geliştirme checkpoint'i / release candidate** olarak yayınlanmıştır; nihai stable ilanı değildir. Sonraki işler `ROADMAP.md` içinde kayıtlıdır.
