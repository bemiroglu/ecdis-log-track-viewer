# Notices / Bildirimler

## English

### Product names and affiliation

Sperry Marine, VisionMaster and related product names are the property of their respective owners.

ECDIS Log Track Viewer is an independent log-analysis utility. It is not an official Sperry Marine product and is not affiliated with or endorsed by Sperry Marine or Northrop Grumman.

### Detailed online basemap

The `v3.15 RC` detailed online basemap uses **OpenFreeMap** rendered through **MapLibre GL JS**. OpenFreeMap styles/data attribution displayed by the application includes:

- OpenFreeMap
- © OpenMapTiles
- Data © OpenStreetMap contributors

The application does not treat the standard OpenStreetMap volunteer raster tile endpoint as its primary detailed basemap. This change followed repeated `403 Access blocked` responses during earlier development/testing.

MapLibre GL JS is a third-party open-source library and remains subject to its own license and project terms.

### Embedded offline fallback map

To preserve basic geographic context when the online detailed basemap cannot load, the application includes a lightweight embedded fallback based on:

- **Natural Earth** generalized land/coastline geometry. Natural Earth data is distributed as public-domain map data.
- **GSHHG (Global Self-consistent, Hierarchical, High-resolution Geography Database)** regional coastline geometry used to refine Turkish/Aegean/Black Sea waters. GSHHG-derived data remains subject to the licensing/attribution terms of the GSHHG project and its underlying distribution.

The embedded fallback is intentionally simplified. It is not a navigation chart and is not intended to replace the online detailed map.

### OpenStreetMap data

Where OpenStreetMap-derived data is displayed, attribution to OpenStreetMap contributors is retained in the application and/or generated print/PDF output.

### VisionMaster documentation

The repository may cite the following publication for interoperability and operating context:

Northrop Grumman Sperry Marine B.V. (2014). *VisionMaster FT Ship's Manual, Volume 2: Configuration & Commissioning* (Part No. 65900011V2-12, Rev. A).

The manual is not included in this repository. Its copyright and distribution terms remain with the publisher.

### Operational data

Do not commit vessel log packages, extracted XML logs, screenshots containing operationally sensitive information, or operational PDF outputs to a public repository unless they have been reviewed and cleared for public release.

The public repository should use synthetic or otherwise cleared demonstration material only.

### Safety

ECDIS Log Track Viewer is a log-analysis and visualization utility. It is not an ECDIS, electronic chart, route-monitoring system, positioning source or navigation decision system.

---

## Türkçe

### Ürün adları ve bağlılık

Sperry Marine, VisionMaster ve ilgili ürün adları kendi hak sahiplerine aittir.

ECDIS Log Track Viewer bağımsız bir log analiz aracıdır. Resmî Sperry Marine ürünü değildir; Sperry Marine veya Northrop Grumman tarafından geliştirilmiş, onaylanmış ya da desteklenmiş olduğu iddia edilmez.

### Ayrıntılı online harita altlığı

`v3.15 RC` ayrıntılı online harita altlığı **MapLibre GL JS** üzerinden **OpenFreeMap** kullanır. Uygulamada gösterilen atıf zinciri şunları içerir:

- OpenFreeMap
- © OpenMapTiles
- Data © OpenStreetMap contributors

Uygulama standart OpenStreetMap gönüllü raster karo sunucusunu artık ana ayrıntılı altlık olarak kullanmaz. Bu değişiklik, önceki geliştirme/test sürecinde tekrarlanan `403 Access blocked` cevaplarından sonra yapılmıştır.

MapLibre GL JS üçüncü taraf açık kaynak yazılımdır ve kendi lisans/proje koşullarına tabidir.

### Gömülü offline fallback haritası

Online ayrıntılı harita yüklenemediğinde temel coğrafi bağlamın korunması için uygulamaya hafif bir fallback katmanı gömülmüştür:

- **Natural Earth** genelleştirilmiş kara/kıyı geometrisi. Natural Earth verileri public-domain harita verisi olarak dağıtılır.
- **GSHHG (Global Self-consistent, Hierarchical, High-resolution Geography Database)** kökenli bölgesel kıyı geometrisi; Türkiye/Ege/Karadeniz çalışma alanında kıyı ayrıntısını iyileştirmek için kullanılır. GSHHG türevi veriler GSHHG projesinin/dağıtımının ilgili lisans ve atıf koşullarına tabidir.

Bu gömülü fallback bilerek sade tutulmuştur. Seyir haritası değildir ve ayrıntılı online haritanın yerini almak amacıyla kullanılmaz.

### OpenStreetMap verisi

OpenStreetMap kökenli veri gösterildiğinde uygulama ve/veya üretilen baskı/PDF çıktılarında OpenStreetMap contributors atfı korunur.

### VisionMaster dokümantasyonu

Repository, birlikte çalışabilirlik ve işletim bağlamı için aşağıdaki yayına atıfta bulunabilir:

Northrop Grumman Sperry Marine B.V. (2014). *VisionMaster FT Ship's Manual, Volume 2: Configuration & Commissioning* (Part No. 65900011V2-12, Rev. A).

Kılavuz repository'ye dahil edilmez. Telif ve dağıtım koşulları yayıncıya aittir.

### Operasyonel veri

Gemi log paketleri, açılmış XML logları, operasyonel hassasiyet taşıyabilecek ekran görüntüleri veya gerçek operasyon PDF çıktıları kamuya açık repository'ye ancak ayrıca incelenip kamuya açılmasına izin verildikten sonra konulmalıdır.

Public repository örneklerinde yalnız sentetik veya ayrıca temizlenmiş/onaylanmış veri kullanılmalıdır.

### Emniyet

ECDIS Log Track Viewer bir log analiz ve görselleştirme aracıdır. ECDIS, elektronik seyir haritası, rota izleme sistemi, konum kaynağı veya seyir karar sistemi değildir.
