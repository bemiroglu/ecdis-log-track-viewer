# ECDIS Log Track Viewer

A single-file HTML5 viewer for selected own-ship track intervals exported from Sperry Marine VisionMaster FT ECDIS log packages.

The application runs locally in the browser. It reads an exported ZIP package or an extracted log folder, converts UTC-based log timestamps to a user-selected local UTC offset, displays the selected interval on an OpenStreetMap basemap, performs basic internal consistency checks, and produces A4 landscape print/PDF output.

![Entry screen](assets/entry-screen.png)

![Map screen](assets/map-screen.png)

A synthetic example output is available at [`examples/sample-output.pdf`](examples/sample-output.pdf). No operational vessel data is included in the repository examples.

---

## English

### Features

- Single HTML file; no installation or server component is required.
- Reads the exported Sperry Marine ZIP package directly.
- Can also read the extracted folder tree.
- Uses CCRS log records as the primary own-ship track source.
- Treats log timestamps as UTC and converts them to the selected fixed local UTC offset.
- Filters the displayed track strictly to the selected local-time interval.
- Default local time zone: `UTC+03:00` (Türkiye).
- Default maximum plausible vessel speed for data-consistency checking: `16 kn`.
- Speed-coloured track rendering.
- Click/tap a track point to inspect local time, UTC log time, position, speed and heading.
- Mobile pinch zoom and desktop wheel/double-click zoom.
- Optional OwnShipHistoryLog quality matching and Announcement event display when those sources are present.
- Internal data-consistency guard: suspicious transitions are marked rather than smoothed into a plausible-looking route. Source coordinates are not rewritten.
- Track distance, duration, average track speed, speed range, log-gap count and data-audit statistics.
- Normal and dense A4 landscape print modes.
- Start/end labels are positioned to avoid covering the plotted track.
- Suggested PDF filename is generated from the selected local date/time interval and optional operation name.
- Source data is processed locally in the browser. The application itself does not upload the log package to a server.

### Quick start

1. Open `ECDIS_Log_Track_Viewer.html` in a current Chromium-based browser such as Chrome or Edge.
2. Select the exported log ZIP with **CHOOSE ZIP**, or select its extracted directory with **CHOOSE FOLDER**.
3. Select the local UTC offset. The default is `UTC+03:00`.
4. Select the required **Start** and **End** times. These fields represent local wall-clock time in the selected offset.
5. Optionally enter an operation name.
6. Adjust marker interval or advanced options if required.
7. Select **SHOW MAP**.
8. Use **PRINT** or **DENSE PRINT** and select **A4 / Landscape** in the system print dialog.

### Time handling

VisionMaster uses a local-time offset convention expressed as:

`Local Time = UTC + Offset`

The viewer follows that convention. For example, with `UTC+03:00` selected, a user selection of `21:30` corresponds to `18:30 UTC` in the log.

Both local time and the underlying UTC log timestamp are shown when a track point is inspected.

### Exporting DataLog from Sperry Marine VisionMaster FT

On the installation used with this viewer, the practical operator path is:

**System → Diagnostics → DataLog → Export**

Recommended workflow:

1. Open **System** on the VisionMaster FT display.
2. Open **Diagnostics**.
3. Select **DataLog**.
4. Select the log interval required for the analysis using the controls available in the installed VisionMaster software revision.
5. Use **Export** to copy the package to approved removable media or a destination folder.
6. On the analysis computer, open the exported ZIP directly in the viewer, or extract it and use **CHOOSE FOLDER**.

The VisionMaster FT Ship's Manual confirms that **DataLog** is part of the Diagnostics menu and that DataLog functions are available from the VisionMaster interface. It also documents CCRS logging and the DataLog directory structure. The exact Export control and screen layout may vary with installed software revision and system configuration.

The manual identifies `C:\Sperry\DataLog` as the default DataLog archive location and lists subfolders including CCRS, Announcement, Chart, Position Sensor and Prompt. CCRS logging is configurable from 1 to 60 seconds, with a documented default interval of 5 seconds.

**Removable-media precaution:** the Sperry Marine manual cautions that USB or other external media should be scanned with current antivirus/malware protection before connection to a VisionMaster PC and, where practicable, reserved for VisionMaster use.

### Supported source layouts

The viewer accepts, when present:

- the complete exported outer ZIP package;
- a direct `CCRS.zip` package;
- an extracted outer directory containing `CCRS.zip`, `OwnShipHistoryLog.zip`, `Announcement.zip`, etc.;
- fully extracted XML directory trees.

CCRS is required for track display. Other packages are optional and enable additional quality/event information.

### Data-consistency policy

This viewer is intended to present the recorded data without inventing vessel motion.

- No coordinate smoothing is applied to make a discontinuity look realistic.
- No replacement positions are generated.
- Short or inconsistent transitions may be flagged as data inconsistencies.
- A calculated transition speed is an integrity test between two logged positions, not a claim that the vessel actually achieved that speed.
- Internally inconsistent segments can be excluded from track-distance calculations and are not drawn as normal vessel motion.
- A log that is internally consistent but absolutely wrong cannot be corrected without an independent reference source.

### Map and network use

Track/log processing is local. Internet access is required to load OpenStreetMap raster tiles for the basemap and print preparation.

Map data: © OpenStreetMap contributors.

### Limitations

- This is a log-analysis and visualization tool, not a navigation system or an ECDIS replacement.
- It does not certify the correctness of source sensor data.
- Fixed UTC offsets are used; daylight-saving rules are not inferred automatically.
- Browser print/PDF behaviour can vary by operating system and print service.
- Dense print mode requests higher-zoom map tiles and may take longer to prepare.

### Repository example data

`examples/synthetic_demo_log.zip` is a synthetic, structurally compatible demonstration package. It contains no operational or vessel-derived data and is supplied only for viewer testing and documentation.

---

## Türkçe

### Özellikler

- Tek HTML dosyasıdır; kurulum veya sunucu bileşeni gerektirmez.
- Sperry Marine tarafından dışa aktarılan ZIP paketini doğrudan okuyabilir.
- Açılmış klasör ağacını da okuyabilir.
- Gemi izinin ana kaynağı olarak CCRS log kayıtlarını kullanır.
- Log zaman damgalarını UTC kabul eder ve kullanıcının seçtiği sabit UTC ofsetine göre yerel saate dönüştürür.
- Haritada yalnız seçilen yerel zaman aralığındaki kayıtları gösterir.
- Varsayılan yerel saat dilimi: `UTC+03:00` (Türkiye).
- Veri tutarlılığı denetimi için varsayılan azami makul gemi hızı: `16 kn`.
- Hıza göre renklendirilmiş iz gösterimi.
- İz üzerindeki bir noktaya tıklayarak/dokunarak yerel saat, UTC log saati, konum, hız ve heading bilgisi görülebilir.
- Mobilde iki parmakla yakınlaştırma; masaüstünde fare tekerleği ve çift tıklama ile zoom.
- Mevcutsa OwnShipHistoryLog kalite bilgisi ve Announcement olayları kullanılabilir.
- Veri tutarlılığı koruması: şüpheli geçişler rota yumuşatılarak gerçekmiş gibi gösterilmez; kaynak koordinatlar değiştirilmez.
- İz mesafesi, süre, ortalama iz hızı, hız aralığı, kayıt boşlukları ve veri denetimi istatistikleri.
- Normal ve Yoğun A4 yatay baskı seçenekleri.
- Başlangıç/bitiş etiketleri iz çizgisini kapatmayacak biçimde yerleştirilir.
- Önerilen PDF dosya adı seçilen yerel tarih-saat aralığından ve varsa operasyon adından üretilir.
- Kaynak log verisi tarayıcı içinde yerel olarak işlenir; uygulamanın kendisi log paketini bir sunucuya yüklemez.

### Hızlı kullanım

1. `ECDIS_Log_Track_Viewer.html` dosyasını güncel Chrome veya Edge gibi Chromium tabanlı bir tarayıcıda açın.
2. Dışa aktarılmış log ZIP dosyasını **ZIP SEÇ** ile veya açılmış klasörünü **KLASÖR SEÇ** ile seçin.
3. Yerel UTC ofsetini seçin. Varsayılan `UTC+03:00`'dır.
4. İstenen **Başlangıç** ve **Bitiş** zamanlarını seçin. Bu alanlar seçilmiş ofsetteki yerel saat olarak yorumlanır.
5. İsterseniz operasyon adı girin.
6. Gerekirse konum işaret aralığını veya gelişmiş seçenekleri değiştirin.
7. **HARİTADA GÖSTER** seçeneğini kullanın.
8. **YAZDIR** veya **YOĞUN YAZDIR** ile sistem yazdırma penceresini açın ve **A4 / Yatay** seçin.

### Saat mantığı

VisionMaster yerel saat ofsetini şu ilişkiyle tanımlar:

`Yerel Saat = UTC + Ofset`

Görüntüleyici de aynı mantığı kullanır. Örneğin `UTC+03:00` seçiliyken kullanıcının `21:30` seçimi log içindeki `18:30 UTC` zamanına karşılık gelir.

Bir iz noktası sorgulandığında hem yerel saat hem ham UTC log zamanı birlikte gösterilir.

### Sperry Marine VisionMaster FT üzerinden DataLog alma

Bu görüntüleyicinin kullanıldığı sistemde pratik işlem yolu:

**System → Diagnostics → DataLog → Export**

Önerilen işlem sırası:

1. VisionMaster FT ekranında **System** menüsünü açın.
2. **Diagnostics** bölümüne girin.
3. **DataLog** sekmesini seçin.
4. Kurulu VisionMaster yazılım sürümünde sunulan seçim alanlarıyla gerekli kayıt zaman aralığını belirleyin.
5. **Export** komutuyla log paketini onaylı bir USB belleğe veya hedef klasöre aktarın.
6. Analiz bilgisayarında dışa aktarılan ZIP'i doğrudan görüntüleyiciye verin veya ZIP'i açıp **KLASÖR SEÇ** seçeneğini kullanın.

VisionMaster FT Ship's Manual, Diagnostics menüsü altında **DataLog** bulunduğunu ve DataLog işlevlerinin VisionMaster arayüzünden erişilebilir olduğunu doğrulamaktadır. Kılavuz ayrıca CCRS log yapılandırmasını ve DataLog klasör yapısını açıklar. **Export** kontrolünün tam görünümü veya menü yerleşimi, kurulu yazılım sürümüne ve sistem yapılandırmasına göre değişebilir.

Kılavuza göre varsayılan DataLog arşiv yolu `C:\Sperry\DataLog` olup CCRS, Announcement, Chart, Position Sensor ve Prompt gibi alt klasörler bulunabilir. CCRS kayıt aralığı 1-60 saniye arasında yapılandırılabilir ve belgelenmiş varsayılan değer 5 saniyedir.

**Harici medya uyarısı:** Sperry Marine kılavuzu, USB veya diğer harici medyanın VisionMaster PC'ye bağlanmadan önce güncel antivirüs/kötücül yazılım taramasından geçirilmesini ve mümkün olduğunca yalnız VisionMaster kullanımı için ayrılmasını tavsiye eder.

### Desteklenen kaynak düzenleri

Mevcut olduğu ölçüde aşağıdaki yapılar desteklenir:

- dışa aktarılmış tam ana ZIP paketi;
- doğrudan `CCRS.zip`;
- içinde `CCRS.zip`, `OwnShipHistoryLog.zip`, `Announcement.zip` vb. bulunan açılmış ana klasör;
- tamamen açılmış XML klasör ağaçları.

İz gösterimi için CCRS gereklidir. Diğer paketler isteğe bağlıdır ve ek kalite/olay bilgisi sağlar.

### Veri tutarlılığı ilkesi

Bu araç, kaydı değiştirmeden göstermeyi amaçlar.

- Konum süreksizliklerini gerçekçi göstermek amacıyla koordinat yumuşatması uygulanmaz.
- Yeni/yapay konum üretilmez.
- Kısa veya fiziksel olarak tutarsız geçişler veri tutarsızlığı olarak işaretlenebilir.
- İki kayıt arasından hesaplanan geçiş hızı, geminin gerçekten bu hıza çıktığı iddiası değil, iki log kaydı arasındaki tutarlılık testidir.
- Tutarsız segmentler normal gemi hareketi gibi çizilmez ve iz mesafesi hesabından çıkarılabilir.
- Kendi içinde tutarlı fakat mutlak olarak yanlış bir log konumu, bağımsız bir referans kaynağı olmadan düzeltilemez.

### Harita ve ağ kullanımı

Log ve rota işleme yereldir. OpenStreetMap raster harita parçalarının gösterilmesi ve baskıya hazırlanması için internet bağlantısı gerekir.

Harita verisi: © OpenStreetMap contributors.

### Sınırlamalar

- Seyir sistemi veya ECDIS yerine kullanılmaz; log inceleme ve görselleştirme aracıdır.
- Kaynak sensör verisinin doğruluğunu garanti etmez.
- Sabit UTC ofseti kullanılır; yaz/kış saati kuralları otomatik uygulanmaz.
- Tarayıcı ve işletim sistemine göre yazdırma/PDF davranışı değişebilir.
- Yoğun baskı daha yüksek zoom seviyesindeki harita parçalarını istediği için daha uzun hazırlanabilir.

### Depodaki örnek veri

`examples/synthetic_demo_log.zip` tamamen sentetik ve görüntüleyiciyle yapısal olarak uyumlu bir test paketidir. Operasyonel veya gerçek gemi verisi içermez; yalnız test ve dokümantasyon amacıyla sunulur.

---

## Reference / Kaynakça

Northrop Grumman Sperry Marine B.V. (2014). *VisionMaster FT Ship's Manual, Volume 2: Configuration & Commissioning* (Part No. 65900011V2-12, Rev. A). Relevant sections include Chapter 2 Diagnostics, Section 8.4.4 CCRS Data Log, Section 8.5.2 Data Log, and Section 8.5.4 Data Location.

The Sperry Marine manual itself is not distributed with this repository.
