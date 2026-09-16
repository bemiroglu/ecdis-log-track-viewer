# ECDIS Log Track Viewer

A single-file HTML5 viewer for selected own-ship track intervals exported from Sperry Marine VisionMaster FT ECDIS log packages.

**Current public development checkpoint: `v3.15 RC`**

> `v3.15 RC` is intentionally published as a release candidate/checkpoint, not as a declaration that development is finished. The long-log rendering, high-resolution basemap, fallback-map and print-path work described below has reached a useful verified state, while further improvements remain deferred in `ROADMAP.md`.

The application runs locally in the browser. It reads an exported ZIP package or an extracted log folder, treats the observed CCRS log timestamps as UTC for this workflow, converts them to a user-selected fixed UTC offset, displays the selected interval, performs internal consistency checks, and produces A4 landscape print/PDF output.

The viewer does **not** smooth or invent vessel positions to make a problematic log look plausible. Source coordinates remain the source coordinates; suspicious transitions may instead be marked or excluded from normal track-distance/rendering logic.

---

## English

### Download

- **Versioned v3.15 RC HTML:** [`ECDIS_Log_Track_Viewer_v3.15_RC.html`](https://raw.githubusercontent.com/bemiroglu/ecdis-log-track-viewer/main/ECDIS_Log_Track_Viewer_v3.15_RC.html)
- **Latest public checkpoint alias:** [`ECDIS_Log_Track_Viewer.html`](https://raw.githubusercontent.com/bemiroglu/ecdis-log-track-viewer/main/ECDIS_Log_Track_Viewer.html)
- **Release-candidate QA notes:** [`docs/QA_v3.15_RC.md`](docs/QA_v3.15_RC.md)
- **Checksums:** [`SHA256SUMS.txt`](SHA256SUMS.txt)

The versioned file is the preferred reference when results need to be reproducible. The unversioned file is only a convenience alias to the current public checkpoint.

### Main capabilities

- Single HTML file; no installation or local web server is required.
- Reads the exported Sperry Marine ZIP package directly.
- Can also read the extracted folder tree.
- Uses CCRS records as the primary own-ship track source.
- Optional OwnShipHistoryLog and Announcement packages are used when available for additional quality/event context.
- Fixed local UTC-offset handling; default `UTC+03:00` for Türkiye.
- Strict selected-interval clipping.
- Speed-coloured track rendering.
- Click/tap a sampled track point to inspect local time, UTC log time, position, speed and heading.
- Mobile pinch zoom and desktop wheel/double-click zoom.
- Track distance, duration, average track speed, speed range, log-gap count and data-audit statistics.
- Normal and dense A4 landscape print modes.
- Start/end labels and suggested PDF filenames derived from the selected interval.
- Browser-local log processing; the application itself does not upload the log package to a server.

### Long-log performance work in v3.11–v3.15 RC

The current checkpoint includes a dedicated long-log rendering path developed after multi-day datasets exposed browser limits.

- Removed large-array `Math.min(...array)` / `Math.max(...array)` calls that could trigger `Maximum call stack size exceeded` on 100k+ records.
- Statistics are calculated iteratively instead of passing very large arrays as function arguments.
- Screen rendering uses cached `Path2D` geometry by zoom level rather than rebuilding the entire track on every pan event.
- Pointer-move redraws are coalesced through `requestAnimationFrame`.
- A display-only level-of-detail (LOD) layer removes visually redundant sub-pixel vertices while preserving the source CCRS records and full-data calculations.
- Print-track rendering uses canvas/Path2D rather than constructing very large SVG path text.
- The route remains visible even when a basemap is unavailable.

The LOD layer is a **rendering optimization only**. It does not rewrite the log, smooth the track, synthesize positions or reduce the record set used for statistics/data-quality checks.

### Map architecture

`v3.15 RC` uses a layered basemap strategy:

1. **Primary detailed basemap:** OpenFreeMap vector map rendered with MapLibre GL JS. The map data is based on OpenStreetMap/OpenMapTiles data and provides the high-resolution road/place context expected from an OSM-style map.
2. **Offline fallback:** a lightweight embedded coastline/land context based on Natural Earth, with regional GSHHG coastline refinement for Turkish/Aegean/Black Sea waters.
3. **Track layer:** the ECDIS track canvas is explicitly stacked above both basemap layers.

The offline coastline is a continuity/fallback layer, not a replacement for the detailed online map. When OpenFreeMap loads successfully, it becomes the visible primary basemap.

Map attribution in the application is retained for the active data/provider path.

### Why the old direct OpenStreetMap raster-tile path was changed

Earlier builds requested the standard OpenStreetMap raster tile service directly. In extended use this could return `403 Access blocked` tiles. The viewer therefore no longer depends on that volunteer-run raster endpoint as its primary application basemap.

The current detailed-map path uses OpenFreeMap/MapLibre; if that online path is unavailable, the embedded coastline context remains available and the track can still be viewed and printed.

### Default data-consistency speed threshold

The default **Maximum plausible vessel speed** is now:

`20 kn`

This is a configurable integrity-screening parameter, not a vessel performance certification. It was raised from the older 16 kn default so valid high-speed/strong-current observations are not automatically treated as implausible in the present working vessel context.

### Quick start

1. Open `ECDIS_Log_Track_Viewer_v3.15_RC.html` in a current Chrome/Edge/Firefox-class browser.
2. Select the exported log ZIP with **CHOOSE ZIP**, or select its extracted directory with **CHOOSE FOLDER**.
3. Select the local UTC offset. Default: `UTC+03:00`.
4. Select **Start** and **End** local times.
5. Optionally enter an operation name.
6. Adjust marker interval or advanced data-consistency settings if required.
7. Select **SHOW MAP**.
8. Use **PRINT** or **DENSE PRINT** and select **A4 / Landscape** in the system print dialog.

### Time handling

This workflow uses:

`Local Time = UTC + Offset`

For example, with `UTC+03:00`, a local selection of `21:30` corresponds to `18:30 UTC` in the log.

Both local and underlying UTC times are shown when a track point is inspected.

### Exporting DataLog from Sperry Marine VisionMaster FT

On the installation used during development, the practical operator path is:

**System → Diagnostics → DataLog → Export**

Recommended sequence:

1. Open **System** on VisionMaster FT.
2. Open **Diagnostics**.
3. Select **DataLog**.
4. Select the required log interval using the controls available in the installed software revision.
5. Use **Export** to copy the package to approved removable media or a destination folder.
6. On the analysis computer, open the exported ZIP directly in the viewer, or extract it and use **CHOOSE FOLDER**.

The VisionMaster FT Ship's Manual documents DataLog under Diagnostics, the CCRS logging configuration and the DataLog directory structure. The exact Export control and screen arrangement can vary by installed revision/configuration.

The manual identifies `C:\Sperry\DataLog` as the default DataLog archive location and lists subfolders including CCRS, Announcement, Chart, Position Sensor and Prompt. CCRS logging is configurable from 1 to 60 seconds, with a documented default of 5 seconds.

**Removable-media precaution:** follow the vessel/company malware-control procedure before connecting USB or other external media to a VisionMaster PC.

### Supported source layouts

When present, the viewer accepts:

- complete exported outer ZIP packages;
- a direct `CCRS.zip` package;
- an extracted directory containing `CCRS.zip`, `OwnShipHistoryLog.zip`, `Announcement.zip`, etc.;
- fully extracted XML directory trees.

CCRS is required for track display. Other packages are optional.

### Data-consistency policy

- No coordinate smoothing is applied to hide discontinuities.
- No replacement/synthetic positions are generated.
- Short or physically inconsistent transitions may be flagged.
- A calculated transition speed is an integrity test between two logged positions, not a claim that the vessel actually achieved that speed.
- Internally inconsistent segments can be excluded from track-distance calculations and from normal-motion drawing.
- A log that is internally self-consistent but absolutely wrong cannot be corrected without an independent reference source.

### Print/PDF behaviour

- Normal and Dense print modes generate A4-landscape layouts.
- The track is rendered independently of the basemap, so a failed online map does not remove the recorded track from the print.
- When the online OpenFreeMap path is available, a high-resolution map snapshot is used for print.
- When it is unavailable, the embedded offline coastline fallback is used instead of waiting indefinitely for broken raster tiles.
- Browser/OS print services can still affect final PDF size and preparation time.

### Current limitations / deferred work

This checkpoint is useful but development is not considered complete. Important deferred items include:

- local detailed PMTiles/offline-map support so a high-resolution basemap can be used without any online provider;
- further long-log performance work for very large datasets;
- additional print caching/profiling;
- further quality/event visualization from OwnShipHistoryLog and Announcement data;
- broader browser/mobile regression coverage;
- a cleaner public release/build pipeline.

See [`ROADMAP.md`](ROADMAP.md).

### Safety and operational use

- This is a log-analysis/visualization tool, not an ECDIS or navigation system.
- It does not certify source sensor accuracy.
- Fixed UTC offsets are used; daylight-saving rules are not inferred automatically.
- Map context is for visualization and must not be used as a navigational chart.
- Operational conclusions should be checked against independent sources and normal bridge/company procedures.

---

## Türkçe

### İndir

- **Sürüm numaralı v3.15 RC HTML:** [`ECDIS_Log_Track_Viewer_v3.15_RC.html`](https://raw.githubusercontent.com/bemiroglu/ecdis-log-track-viewer/main/ECDIS_Log_Track_Viewer_v3.15_RC.html)
- **En güncel public checkpoint kısayolu:** [`ECDIS_Log_Track_Viewer.html`](https://raw.githubusercontent.com/bemiroglu/ecdis-log-track-viewer/main/ECDIS_Log_Track_Viewer.html)
- **RC kalite/test notları:** [`docs/QA_v3.15_RC.md`](docs/QA_v3.15_RC.md)
- **SHA-256 değerleri:** [`SHA256SUMS.txt`](SHA256SUMS.txt)

Sonucun tekrar üretilebilir olması önemliyse sürüm numaralı dosyanın kullanılması tavsiye edilir. Sürümsüz dosya yalnız mevcut public checkpoint'e işaret eden kolaylık kısayoludur.

### Temel özellikler

- Tek HTML dosyasıdır; kurulum veya yerel web sunucusu gerektirmez.
- Sperry Marine tarafından dışa aktarılan ZIP paketini doğrudan okuyabilir.
- Açılmış klasör ağacını da okuyabilir.
- Gemi izinin ana kaynağı olarak CCRS kayıtlarını kullanır.
- Varsa OwnShipHistoryLog ve Announcement paketlerini ek kalite/olay bağlamı için kullanabilir.
- Sabit UTC ofseti ile yerel saat gösterimi; Türkiye için varsayılan `UTC+03:00`.
- Seçilen zaman aralığına sıkı filtreleme.
- Hıza göre renklendirilmiş iz.
- Örneklenen bir iz noktasına tıklayarak/dokunarak yerel saat, UTC log saati, konum, hız ve heading bilgisi.
- Mobil pinch-zoom ve masaüstü fare tekerleği/çift tıklama desteği.
- İz mesafesi, süre, ortalama iz hızı, hız aralığı, kayıt boşlukları ve veri denetimi istatistikleri.
- Normal ve Yoğun A4 yatay baskı seçenekleri.
- Başlangıç/bitiş etiketleri ve zaman aralığından üretilen PDF dosya adı önerisi.
- Kaynak log verisi tarayıcı içinde işlenir; uygulamanın kendisi log paketini bir sunucuya yüklemez.

### v3.11–v3.15 RC uzun-log performans çalışmaları

Çok günlük kayıtların tarayıcı sınırlarını göstermesi üzerine mevcut checkpoint'e ayrı bir uzun-log render yolu eklendi.

- 100 bin+ kayıtta `Maximum call stack size exceeded` oluşturabilen büyük-dizi `Math.min(...array)` / `Math.max(...array)` çağrıları kaldırıldı.
- İstatistikler iteratif hesaplanıyor.
- Ekran izi, her pan hareketinde baştan kurulmak yerine zoom seviyesine göre cache'lenen `Path2D` geometrisiyle çiziliyor.
- Pointer-move çizimleri `requestAnimationFrame` ile birleştiriliyor.
- Yalnız görüntüleme katmanında, aynı/sub-pixel konuma düşen gereksiz vertexleri azaltan LOD uygulanıyor.
- Baskı izi çok büyük SVG metni üretmek yerine canvas/Path2D ile hazırlanıyor.
- Harita altlığı gelmese dahi iz ekranda ve baskıda görülebiliyor.

LOD **yalnız render optimizasyonudur**. Ham CCRS kayıtlarını azaltmaz; istatistik/veri denetimi kayıtlarını değiştirmez; rota yumuşatmaz ve yapay konum üretmez.

### Harita mimarisi

`v3.15 RC` üç katmanlı bir yaklaşım kullanır:

1. **Ana ayrıntılı harita:** MapLibre GL JS ile çizilen OpenFreeMap vektör haritası. OpenStreetMap/OpenMapTiles verisi üzerinden yüksek çözünürlüklü yol/yer bağlamı sağlar.
2. **Offline fallback:** Natural Earth tabanlı hafif dünya kara/kıyı bağlamı; Türkiye/Ege/Karadeniz çevresinde GSHHG kıyı verisiyle bölgesel iyileştirme.
3. **ECDIS izi:** rota canvas'ı hem online hem offline harita katmanlarının açık biçimde üstünde tutulur.

Offline coastline ana haritanın yerine geçmek için değil, bağlantı/provider sorunu sırasında iz ve coğrafi bağlamın tamamen kaybolmaması için vardır. OpenFreeMap başarılı olduğunda görünür ana altlık odur.

### Neden eski doğrudan OpenStreetMap raster karo yolu değiştirildi?

Önceki sürümlerde standart OpenStreetMap raster tile servisine doğrudan bağlanılıyordu. Uzun kullanım/testlerde `403 Access blocked` karo cevapları görüldü. Bu nedenle uygulama artık gönüllü OSM raster sunucusunu ana harita altyapısı olarak kullanmıyor.

Ayrıntılı online altlık OpenFreeMap/MapLibre yoluyla sağlanıyor. Bu yol çalışmazsa gömülü coastline fallback devrede kalıyor ve ECDIS izi yine gösterilebiliyor/basılabiliyor.

### Varsayılan makul hız eşiği

Varsayılan **Azami makul gemi hızı** artık:

`20 kn`

Bu değer bir gemi performans sertifikası değil, veri tutarlılığı tarama parametresidir. Mevcut çalışma gemisinde akıntı vb. koşullarda 16 kn üzerindeki gerçek gözlemlerin otomatik olarak şüpheli sayılmaması için eski 16 kn varsayılanından 20 kn'a çıkarılmıştır. Kullanıcı gerektiğinde değiştirebilir.

### Hızlı kullanım

1. `ECDIS_Log_Track_Viewer_v3.15_RC.html` dosyasını güncel Chrome/Edge/Firefox sınıfı bir tarayıcıda açın.
2. Dışa aktarılmış log ZIP'ini **ZIP SEÇ** ile veya açılmış klasörünü **KLASÖR SEÇ** ile seçin.
3. Yerel UTC ofsetini seçin. Varsayılan `UTC+03:00`.
4. Yerel **Başlangıç** ve **Bitiş** saatlerini seçin.
5. İsterseniz operasyon adı girin.
6. Gerekirse işaret aralığını veya veri denetimi ayarlarını değiştirin.
7. **HARİTADA GÖSTER** seçeneğini kullanın.
8. **YAZDIR** veya **YOĞUN YAZDIR** ile sistem yazdırma penceresini açıp **A4 / Yatay** seçin.

### Saat mantığı

Bu çalışma hattında:

`Yerel Saat = UTC + Ofset`

Örneğin `UTC+03:00` seçiliyken yerel `21:30`, log içindeki `18:30 UTC` zamanına karşılık gelir.

Bir iz noktası incelendiğinde hem yerel zaman hem ham UTC log zamanı gösterilir.

### Sperry Marine VisionMaster FT üzerinden DataLog alma

Geliştirme sırasında kullanılan kurulumdaki pratik işlem yolu:

**System → Diagnostics → DataLog → Export**

Önerilen sıra:

1. VisionMaster FT üzerinde **System** menüsünü açın.
2. **Diagnostics** bölümüne girin.
3. **DataLog** bölümünü seçin.
4. Kurulu yazılım sürümündeki kontrollerle gerekli kayıt aralığını belirleyin.
5. **Export** ile paketi onaylı harici ortama/hedef klasöre aktarın.
6. Analiz bilgisayarında ZIP'i doğrudan viewer'a verin veya açıp **KLASÖR SEÇ** kullanın.

VisionMaster FT Ship's Manual; Diagnostics altında DataLog'u, CCRS kayıt yapılandırmasını ve DataLog klasör yapısını belgeler. Export kontrolünün görünümü ve ekran düzeni kurulu sürüm/yapılandırmaya göre değişebilir.

Kılavuzda varsayılan DataLog yolu `C:\Sperry\DataLog` olarak belirtilir; CCRS, Announcement, Chart, Position Sensor ve Prompt gibi alt klasörler bulunabilir. CCRS kayıt aralığı 1–60 saniye arasında yapılandırılabilir ve belgelenmiş varsayılan 5 saniyedir.

**Harici medya:** USB/harici ortam bağlamadan önce gemi/şirket kötü amaçlı yazılım kontrol prosedürlerini uygulayın.

### Desteklenen kaynak düzenleri

Mevcut olduğu ölçüde:

- tam dışa aktarılmış ana ZIP;
- doğrudan `CCRS.zip`;
- `CCRS.zip`, `OwnShipHistoryLog.zip`, `Announcement.zip` vb. içeren açılmış klasör;
- tamamen açılmış XML klasör ağaçları

desteklenir.

İz için CCRS gereklidir; diğer paketler isteğe bağlıdır.

### Veri tutarlılığı ilkesi

- Süreksizliği saklamak amacıyla koordinat yumuşatması yapılmaz.
- Yapay/yedek konum üretilmez.
- Fiziksel olarak tutarsız kısa geçişler işaretlenebilir.
- İki kayıt arasından hesaplanan geçiş hızı geminin gerçekten bu hızı yaptığı iddiası değil, veri bütünlüğü testidir.
- Tutarsız segmentler normal hareket gibi çizilmeyebilir ve mesafe hesabından çıkarılabilir.
- Kendi içinde tutarlı fakat mutlak olarak yanlış bir log, bağımsız referans olmadan düzeltilemez.

### Baskı/PDF davranışı

- Normal ve Yoğun baskı A4 yatay çıktı üretir.
- İz, harita altlığından bağımsız çizilir; online harita başarısız olduğunda rota baskıdan kaybolmaz.
- OpenFreeMap yolu hazırsa baskıda yüksek çözünürlüklü harita snapshot'ı kullanılır.
- Online yol kullanılamıyorsa eski 403 raster karo mozağini beklemek yerine gömülü offline coastline devreye girer.
- Son PDF boyutu ve hazırlama süresi tarayıcı/işletim sistemi yazdırma servisine göre değişebilir.

### Mevcut sınırlamalar / tehir edilen işler

Bu checkpoint kullanışlıdır ancak geliştirme bitmiş sayılmamaktadır. Özellikle:

- internet/provider bağımlılığı olmadan ayrıntılı harita sağlayacak yerel PMTiles desteği;
- çok daha büyük loglarda ilave performans çalışmaları;
- baskı cache/profiling iyileştirmeleri;
- OwnShipHistoryLog ve Announcement verilerinin daha zengin kalite/olay katmanları;
- daha geniş mobil/tarayıcı regresyon testleri;
- daha temiz, sürüm-izli public build/release zinciri

sonraki çalışma için saklanmıştır.

Bkz. [`ROADMAP.md`](ROADMAP.md).

### Emniyet / operasyonel kullanım

- Bu araç ECDIS veya seyir sistemi değildir; log analiz/görselleştirme aracıdır.
- Kaynak sensör verisinin doğruluğunu garanti etmez.
- Sabit UTC ofseti kullanır; yaz/kış saati kurallarını otomatik çıkarmaz.
- Harita altlığı seyir haritası değildir.
- Operasyonel çıkarımlar bağımsız kaynaklarla ve normal köprüüstü/şirket prosedürleriyle doğrulanmalıdır.

---

## Reference / Kaynakça

Northrop Grumman Sperry Marine B.V. (2014). *VisionMaster FT Ship's Manual, Volume 2: Configuration & Commissioning* (Part No. 65900011V2-12, Rev. A).

Relevant sections include Diagnostics, CCRS Data Log, Data Log and Data Location. The manual itself is **not** distributed in this repository.

Third-party map/data notices are documented in [`NOTICE.md`](NOTICE.md).