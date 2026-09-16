# Changelog / Değişiklik Kaydı

## v3.15 RC

### English

- Fixed the interactive-map stacking regression seen in v3.14 RC: once the OpenFreeMap/MapLibre layer became visible, it could paint above the ECDIS track canvas. The track canvas and pointer layer now have explicit higher stacking levels inside an isolated map stacking context.
- Preserved all v3.14 long-log, basemap, fallback and print-path improvements.
- Confirmed the track remains visible above an opaque simulated online basemap in regression testing.
- Removed stale print-version text so browser, screen and print markers consistently report `v3.15 RC`.
- Current default maximum plausible vessel speed remains `20 kn`.

### Türkçe

- v3.14 RC'de görülen etkileşimli harita katmanlama hatası düzeltildi: OpenFreeMap/MapLibre katmanı görünür olduktan sonra ECDIS iz canvas'ının üstüne çıkabiliyordu. İz canvas'ı ve pointer katmanı artık izole bir map stacking context içinde açıkça daha yüksek katmanda tutuluyor.
- v3.14'te kazanılan uzun-log, ayrıntılı harita, offline fallback ve baskı optimizasyonlarının tamamı korundu.
- Regresyon testinde rota, opak online harita simülasyonunun üzerinde görünür kaldı.
- Tarayıcı, ekran ve baskı sürüm işaretlerinin tamamı `v3.15 RC` olarak tutarlı hale getirildi.
- Varsayılan azami makul gemi hızı `20 kn` olarak korunuyor.

## v3.14 RC

### English

- Restored the intended primary detailed-map path using a valid MapLibre GL JS release with OpenFreeMap vector styles.
- Online map loading now tries UNPKG first and jsDelivr as fallback.
- Kept the embedded coastline map only as a fallback instead of treating it as the normal primary map.
- Removed the overlapping dual-coastline effect: the regional GSHHG refinement and Natural Earth fallback are no longer drawn on top of one another in the same area.
- Retained zoom-level Path2D caching, requestAnimationFrame redraw coalescing and render-only LOD for long tracks.
- Retained optimized canvas-based print-track generation.

### Türkçe

- Geçerli MapLibre GL JS sürümü ve OpenFreeMap vector style kullanılarak amaçlanan ayrıntılı online ana harita yolu geri getirildi.
- Online map library yüklemesinde önce UNPKG, sonra jsDelivr deneniyor.
- Gömülü coastline katmanı normal ana harita değil, fallback olarak korunuyor.
- Regional GSHHG ile Natural Earth kıyılarının üst üste çizilmesinden doğan karışık görünüm kaldırıldı.
- Uzun izler için zoom-bazlı Path2D cache, requestAnimationFrame redraw birleştirmesi ve yalnız render katmanında LOD korunuyor.
- Canvas tabanlı hızlı baskı izi korunuyor.

## v3.13 RC

### English

- Added a guaranteed embedded coastline/land fallback so the viewer retains geographic context even when the online detailed basemap cannot load.
- Added zoom-level track geometry caching and display-only LOD to reduce repeated projection/Path2D work during pan/zoom.
- Added a faster fallback print-basemap path and avoided repeatedly waiting on unavailable online basemap timeouts.

### Türkçe

- Online ayrıntılı harita yüklenemediğinde dahi coğrafi bağlamın tamamen kaybolmaması için gömülü coastline/land fallback eklendi.
- Pan/zoom sırasında aynı projeksiyon/Path2D işinin tekrarını azaltmak için zoom-bazlı iz geometry cache ve yalnız görüntüleme katmanında LOD eklendi.
- Hızlı fallback baskı altlığı yolu eklendi; erişilemeyen online harita için gereksiz timeout beklemeleri azaltıldı.

## v3.12 RC

### English

- Raised the default maximum plausible vessel speed from 16 kn to 20 kn.
- Removed the direct standard OpenStreetMap raster-tile path from the print workflow after repeated `403 Access blocked` responses.
- Reworked print-track rendering away from huge SVG path generation toward canvas/Path2D.
- Added online vector-map snapshot use for print with offline fallback.

### Türkçe

- Varsayılan azami makul gemi hızı 16 kn'dan 20 kn'a çıkarıldı.
- Tekrarlanan `403 Access blocked` cevapları nedeniyle baskıda doğrudan standart OpenStreetMap raster karo yolu kaldırıldı.
- Çok büyük SVG path üretmek yerine baskı izi canvas/Path2D ile hazırlanacak şekilde yeniden düzenlendi.
- Baskıda online vector-map snapshot ve offline fallback yapısı eklendi.

## v3.11 RC

### English

- Fixed `Maximum call stack size exceeded` on long/multi-day logs caused by very large spread-argument min/max calculations.
- Replaced large-array spread statistics with iterative calculations.
- Improved long-track canvas rendering by batching speed-class geometry.
- Began migration from direct OSM volunteer raster tiles to OpenFreeMap/MapLibre for the detailed basemap.

### Türkçe

- Uzun/çok günlük loglarda büyük spread-argument min/max hesapları nedeniyle oluşan `Maximum call stack size exceeded` hatası düzeltildi.
- Büyük dizilerin spread edilmesi yerine iteratif istatistik hesabına geçildi.
- Hız sınıfına göre toplu canvas çizimiyle uzun iz performansı iyileştirildi.
- Ayrıntılı harita için doğrudan OSM gönüllü raster karolarından OpenFreeMap/MapLibre yoluna geçiş başlatıldı.

## v3.10

- Added bilingual Sperry DataLog export help to the welcome screen.
- Added the field-tested operator workflow `System → Diagnostics → DataLog → Export` with a revision/configuration caveat.
- Added removable-media handling precaution based on the VisionMaster FT Ship's Manual.
- Prepared public-repository documentation and synthetic demonstration assets.

## v3.9

- Corrected UTC-to-local interval conversion.
- Local-time selections now map to the corresponding UTC log interval.
- Added bilingual entry-screen labels and UTC/local preview.
- Default local offset set to UTC+03:00.
- Default maximum plausible vessel speed at that stage was 16 kn.

## v3.7–v3.8

- Enforced selected-interval filtering across display, interaction, statistics and print output.
- Added extracted-folder input support.
- Added UTC/local display logic and data-consistency controls.
- Removed route smoothing that could imply unrecorded vessel motion.

## v3.5–v3.6

- Improved print layout and statistics placement.
- Added navigation statistics, start/end labels, operation name and version metadata.
- Improved print filename suggestions and normal/dense print handling.

## v1–v3.4

- Initial Sperry log parsing, map rendering, speed-coloured track, print output and mobile/desktop map interaction.
