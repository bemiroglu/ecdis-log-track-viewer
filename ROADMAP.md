# Roadmap / Tehir Edilen Geliştirmeler

This file records work intentionally deferred after the `v3.16 RC` public checkpoint so that the current gains and development direction are not lost.

Bu dosya, `v3.16 RC` public checkpoint sonrasında bilinçli olarak tehir edilen işleri kaydeder; amaç mevcut kazanımları ve geliştirme rotasını kaybetmemektir.

---

## English

### Priority 1 — Detailed basemap reliability without provider dependence

The current primary map is OpenFreeMap/MapLibre with an embedded Natural Earth/GSHHG coastline fallback. The next major reliability step should be **local detailed map support**, preferably PMTiles or an equivalent browser-readable package.

Target behaviour:

- user can select a local regional map package;
- detailed road/place/coastline context remains available with no internet connection;
- online OpenFreeMap remains an optional convenience layer;
- print uses the same selected local map source where possible;
- attribution/licensing remains explicit.

### Priority 2 — Further long-log performance

`v3.16 RC` already removes the call-stack failure and uses zoom-level Path2D caching plus display-only LOD. Further work should focus on:

- moving heavy parsing/geometry preparation to Web Workers;
- incremental parsing for very large ZIP/XML datasets;
- adaptive LOD thresholds by screen DPI/zoom;
- avoiding unnecessary full-data scans after UI-only changes;
- profiling 250k, 500k and 1M-record datasets.

Source records and full-data statistics must remain untouched by visual LOD.

### Priority 3 — Print/PDF performance and fidelity

v3.16 RC adds a four-part Dense Print OpenFreeMap/MapLibre mosaic capture to avoid one oversized WebGL render target. Remaining work:

- cache reusable basemap snapshots;
- avoid unnecessary re-render if viewport/zoom has not changed;
- profile Normal vs Dense print independently;
- keep the track visible even if online map rendering fails;
- evaluate vector/PDF-friendly paths without reintroducing huge SVG strings;
- preserve clear start/end labels and attribution.

### Priority 4 — Data-quality / academic-analysis layer

Candidate workstream:

- CCRS validity/plausibility/source/origin/selection fields;
- OwnShipHistoryLog matching and sensor-selection changes;
- Announcement events;
- sensor/data-source transitions;
- log-gap classification;
- speed/position integrity events;
- exportable audit summaries suitable for academic analysis.

The viewer should continue to distinguish **recorded source data** from **derived integrity tests**.

### Priority 5 — Map/user-interface refinements

- map-style selection;
- optional labels/events without clutter;
- improved collision handling for start/end/event markers;
- better mobile control placement;
- clearer online/offline basemap status indication;
- a visible performance/debug panel for large-log diagnostics when needed.

### Priority 6 — Release engineering

- versioned release artifacts rather than only a moving filename;
- deterministic checksum generation;
- browser regression tests;
- a public synthetic long-log corpus;
- explicit release-candidate vs stable status;
- eventually split readable source/build structure if development resumes deeply.

### Non-goals / preserved principles

- Do not smooth a discontinuous log merely to make it look realistic.
- Do not invent replacement vessel positions.
- Do not turn a derived transition speed into a claim about actual vessel speed.
- Do not silently change source timestamps or coordinates.
- Do not treat the basemap as a navigation chart.

---

## Türkçe

### Öncelik 1 — Harita sağlayıcısına bağımlı olmayan ayrıntılı altlık

Mevcut ana harita OpenFreeMap/MapLibre; emniyet/fallback katmanı Natural Earth/GSHHG kıyı haritasıdır. Sonraki büyük güvenilirlik adımı **yerel ayrıntılı harita desteği** olmalıdır; tercihen PMTiles veya tarayıcıdan okunabilen eşdeğer paket.

Hedef davranış:

- kullanıcı bölgesel yerel harita paketi seçebilsin;
- internet olmadan ayrıntılı yol/yer/kıyı bağlamı korunabilsin;
- OpenFreeMap isteğe bağlı online kolaylık katmanı olarak kalsın;
- baskı mümkün olduğunda aynı yerel harita kaynağını kullansın;
- lisans/atıf açık biçimde korunsun.

### Öncelik 2 — Uzun log performansını daha ileri taşımak

`v3.16 RC` call-stack hatasını kaldırmış, zoom-bazlı Path2D cache ve yalnız görüntüleme katmanında LOD kullanmaktadır. Sonraki çalışma:

- ağır parse/geometri işlerini Web Worker'a taşımak;
- çok büyük ZIP/XML veri setlerinde incremental parsing;
- ekran DPI/zoom'a bağlı adaptive LOD;
- yalnız UI değiştiğinde gereksiz tam-veri taramalarını önlemek;
- 250 bin, 500 bin ve 1 milyon kayıtlık profil testleri

üzerine odaklanmalıdır.

Görsel LOD ham kayıtları veya tam-veri istatistiklerini değiştirmemelidir.

### Öncelik 3 — Baskı/PDF performansı ve doğruluğu

v3.16 RC, tek ve çok büyük WebGL render hedefinden kaçınmak için Yoğun Baskıda dört parçalı OpenFreeMap/MapLibre mozaik yakalama ekler. Kalan işler:

- tekrar kullanılabilir basemap snapshot cache;
- viewport/zoom değişmediyse gereksiz yeniden render yapmama;
- Normal ve Yoğun baskıyı ayrı profil etme;
- online harita başarısız olsa da izi baskıda koruma;
- dev SVG metinlerine geri dönmeden vector/PDF dostu yolları değerlendirme;
- başlangıç/bitiş etiketleri ve attribution okunaklılığını koruma.

### Öncelik 4 — Veri kalitesi / akademik analiz katmanı

Aday çalışma hattı:

- CCRS validity/plausibility/source/origin/selection alanları;
- OwnShipHistoryLog eşleştirmesi ve sensor-selection değişimleri;
- Announcement olayları;
- sensör/veri-kaynağı geçişleri;
- log-gap sınıflandırması;
- hız/konum bütünlük olayları;
- akademik analizde kullanılabilecek export edilebilir audit özetleri.

Viewer, **kaydedilmiş kaynak veri** ile **türetilmiş bütünlük testini** açıkça ayırmaya devam etmelidir.

### Öncelik 5 — Harita ve kullanıcı arayüzü

- harita stil seçimi;
- ekranı boğmadan isteğe bağlı etiket/olay katmanları;
- başlangıç/bitiş/olay etiketlerinde daha iyi collision handling;
- mobil kontrol yerleşiminin iyileştirilmesi;
- online/offline altlık durumunun daha açık gösterimi;
- gerektiğinde büyük-log tanılama için görünür performans/debug paneli.

### Öncelik 6 — Release mühendisliği

- yalnız hareketli dosya adı yerine sürüm numaralı release artifact'ları;
- deterministik checksum üretimi;
- tarayıcı regresyon testleri;
- public sentetik uzun-log test corpus'u;
- RC ve stable statüsünün açık ayrımı;
- geliştirme yeniden derinleşirse okunabilir source/build yapısına geçiş.

### Korunacak ilkeler / yapılmayacaklar

- Süreksiz logu gerçekçi göstermek için smoothing yapılmayacak.
- Yapay yedek gemi konumu üretilmeyecek.
- Türetilen geçiş hızı, geminin gerçekten yaptığı hız gibi sunulmayacak.
- Kaynak zaman damgası veya koordinat sessizce değiştirilmeyecek.
- Harita altlığı seyir haritası gibi kullanılmayacak.
