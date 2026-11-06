# TYBS Başvuru Metinleri — TÜBİTAK 2209-A (2026)

**Tarih:** 6 Kasım 2026 · **Son gün:** 11 Kasım 2026, 17.30 (yürütücünün son onayı + danışman onayı)  
**Dayanak:** [Güncel başvuru rehberi](TUBITAK_2209A_2026_Basvuru_Rehberi.md) · [Protokoller ve stratejiler](../protokoller_ve_stratejiler.md) · [Deney planı](../deney_plani.md) · [Maliyet](../maliyet.md) · [Kaynakça notları](../kaynakca.md) · [Bırakma mekanizması](../birakma_mekanizmasi.md)

Dosya, TYBS ekran sırasını izler. ` ```text ` blokları **olduğu gibi** kopyalanıp ilgili kutuya yapıştırılır. Blok dışındaki notlar ekip içindir, TYBS'ye yazılmaz. Sınırların kontrolü dosyanın sonunda.

> **Numaralandırma notu (6 Kasım düzeltmesi):** TYBS metninde protokoller **P1/P2/P3** olarak numaralandı. İç belgelerdeki **P3** (ham ESP-NOW) burada **P2**, **P5** (SBZ) burada **P3**. Hipotezler de araştırma sorularının sırasına göre yeniden numaralandı: eski H1 → H1, eski H3 → H2, eski H4 → H3, eski H2 → H5; **H4 (protokol × kural etkileşimi) yeni**. S★ yerine "en iyi gözlenen yerleşim" kullanılıyor. Rafa kaldırılan sade tasarım: [sadelestirme-onerisi.md](../sadelestirme-onerisi.md).

---

## Ekip ve görev dağılımı

| Kişi | Rol | Sorumluluk |
|---|---|---|
| **Aleyna** | Yürütücü | Proje koordinasyonu; SBZ protokolünün tasarımı ve geliştirilmesi (ham ESP-NOW zinciri dâhil); ayar belgesi; istatistiksel analiz; kod ve veri setinin yayımlanması |
| **Hatice** | Ortak | ESP-WIFI-MESH uygulaması ve bekleme modu; yerleştirme kuralları; sinyal haritası ve pilot; analiz |
| **Hacer** | Ortak | Bırakıcı robot (2WD şasi, sürüş kartı, motorlar ve enkoderler, güç sistemi, kumanda); mekanizmanın elektroniği, montajı ve kabul testi |
| **Mustafa** | Ortak | Ölçüm altyapısı (merkez düğüm kaydı, RTT, koşu betikleri); bırakma mekanizması ve düğüm kutularının 3D tasarımı ve baskısı |

Robotlu deneylerde (İP5) dört kişi birlikte çalışır.

---

## EKRAN 04 — Bütçe

| TYBS alanı | Tutar (TL) |
|---|---:|
| Makine Teçhizat Giderleri | **1.858** |
| Hizmet Alımı Giderleri | **0** |
| Sarf Malzemesi Gideri | **10.142** |
| Ulaşım Giderleri | **0** |
| **Toplam** | **12.000** |

- **Makine-teçhizat:** Demirbaşa kaydedilecek iki cihaz: deneye özel router (~750 TL) ve lazer mesafe ölçer (1.108,05 TL). Kartlar, piller, motorlar, filament ve vidalar gibi diğer her şey sarf malzemesidir. Kalem kalem döküm: [maliyet.md](../maliyet.md).
- Kategori ayrımını danışmana ya da üniversitenin proje ofisine bir kez teyit ettirin.

---

## EKRAN 05 — Araştırma Projesi Bilgileri

### Proje başlığı

```text
Kapalı ve Engelli Ortamlarda Robottan Röle Bırakma (Breadcrumbing): Düşük Maliyetli Wi-Fi Düğümleriyle Bırakma Kuralları ve Ağ Protokollerinin Deneysel Karşılaştırması
```

### Anahtar kelimeler

```text
Çok Atlamalı Kablosuz Ağlar, Röle Düğümü Yerleştirme, Breadcrumbing, Mobil Robotik, Gömülü Sistemler
```

### Bilimsel ve teknolojik faaliyet alanı

- **Birinci alan:** Mühendislik → Elektrik-Elektronik Mühendisliği → *Telekomünikasyon / Bilgisayar ağları / Kablosuz haberleşme* (listede hangisi varsa).
- **İkinci alan (eklenebiliyorsa):** *Robotik / Kontrol ve otomasyon*.

Değerlendiriciler bu seçime göre atanır. Projenin özü kablosuz ağ olduğu için birinci alan haberleşme veya ağ olmalıdır. Seçimi danışmanla netleştirin.

### Öncelikli alan

**Evet** seçin.

- **Birincil:** *Afet yönetimi teknolojileri.* Projenin problemi (göçük, maden, tünel) doğrudan bu alana ait.
- **İkincil (birden fazla seçilebiliyorsa):** *Robotik-mekatronik.*
- *Yeni nesil haberleşme teknolojileri*'ni seçmeyin. Strateji belgelerinde 5G/6G çağrışımı var; 2,4 GHz Wi-Fi için zorlama görünür.

Bilgi notu, öncelikli alanla ilişkinin **1.1'de** açıklanmasını istiyor. Bu gerekçe 1.1'in ikinci paragrafında yazılı. Öncelikli alan otomatik puan getirmez, yalnızca ayrı destek taban puanı belirlenebilir. Ayrıntı: [öncelikli alan araştırması](../../tmp/TUBITAK_2209A_2026_Oncelikli_Alanlar_Arastirmasi.md).

TYBS ayrıca bir gerekçe kutusu açarsa:

```text
Proje, kapalı ve engelli ortamlarda çalışan arama-kurtarma robotlarının haberleşmesini düşük maliyetli röle düğümleriyle sürdürmeyi araştırmaktadır. Göçük altındaki yapılar, maden ve tüneller gibi altyapısız ortamlara yönelik bu çalışma Afet Yönetimi Teknolojileri alanıyla doğrudan, Robotik-Mekatronik alanıyla dolaylı olarak ilişkilidir.
```

### Proje özeti (sınır 25–450 kelime)

```text
Maden, tünel, göçük altındaki yapılar ve bodrum katları gibi kapalı ve engelli ortamlarda görev yapan robotların kablosuz bağlantısı beton, metal ve köşeler nedeniyle kısa mesafede kopabilmekte; robot hem kumandayı hem de görüntü ve veri akışını kaybetmektedir. Bu sorunun bilinen bir çözümü, robotun ilerlerken arkasına röle düğümleri bırakarak çok atlamalı bir ağ kurmasıdır (breadcrumbing). DARPA Yeraltı Yarışması'nı kazanan ekip bu yöntemi ticari mesh radyolarıyla uygulamış, ancak düğümlerin nereye ve ne zaman bırakılacağına insan operatör karar vermiştir. Bırakma kurallarının düşük maliyetli donanımda, kontrollü ve tekrarlı deneylerle karşılaştırılması ise eksik kalmıştır. Bu projede, ESP32-C3 Wi-Fi mikrodenetleyicileriyle kurulan röle zincirlerinin kapalı ortamlarda bağlantıyı ne kadar uzağa ve hangi kalitede taşıyabildiği ile bırakma kuralının ve ağ protokolünün bu başarıya etkisi araştırılacaktır.

Operatörün kablolu kumandayla sürdüğü iki tekerlekli, diferansiyel sürüşlü bir robot, step motorla dönen revolver tipi bir mekanizmayla en fazla beş röle düğümünü zemine bırakacaktır. Bırakma kararı üç kuraldan biriyle otomatik olarak verilecektir: sabit mesafe, sinyal gücü (RSSI) eşiği ve sinyal eğilimine dayalı öngörü. Ağ tarafında kendini kuran hazır ESP-WIFI-MESH protokolü, sade bir ESP-NOW zinciri ve bu projede geliştirilecek Sıra Bilen Zincir (SBZ) protokolü karşılaştırılacaktır. SBZ, robotun bıraktığı her röleye üst düğümlerini bildirerek ağ keşfi beklemeden bağlanmayı, sınırlı yeniden gönderimi ve arızalanan düğümün atlanmasını sağlar.

Deneyler üç aşamada yürütülecektir. Kalibrasyon ve pilot aşamasında ortamların sinyal haritası çıkarılacak, deney ayarları önceden yazılmış kurallarla belirlenip dondurulacaktır. Statik zincir aşamasında protokoller sabit röle konumlarında kapasite, gecikme, yeni röleye bağlanma ve arıza sonrası toparlanma bakımından karşılaştırılacaktır. Dinamik aşamada robot, düz koridor, L dönüşlü koridor ve betonu yoğun bodrum ortamlarında bırakma kurallarını uygulayacaktır. Her koşul beş kez tekrarlanacak (yaklaşık 180 koşu); kamera trafiği hızı ayarlanabilen sentetik paket akışıyla temsil edilecektir. Paket teslim oranı, goodput, gidiş-dönüş gecikmesi, kesinti sayısı ve süresi, kullanılan röle sayısı ve ulaşılan mesafe ölçülecek; sonuçlar gün etkisini hesaba katan karma etkili modellerle, etki büyüklüğü ve %95 güven aralığıyla raporlanacaktır.

Proje, yürütücü ve üç ortaktan oluşan dört kişilik öğrenci ekibi tarafından 12 ayda altı iş paketiyle yürütülecektir. Ölçüm altyapısı ve analiz, ağ protokolleri, robot ve bırakma mekanizması ile yerleştirme kuralları farklı ekip üyelerinin sorumluluğundadır. Statik deneyler robottan bağımsız planlandığından mekanik gecikmeler ağ deneylerini durdurmaz; mekanizma çalışmazsa düğümler aynı karar kuralıyla elle bırakılır.

Beklenen çıktılar; düğüm başına yaklaşık 650 TL malzeme maliyetli bir breadcrumbing prototipi, açık kaynak SBZ protokolü ve deney yazılımları, açık veri seti ve bir bildiri taslağıdır. Sonuçlar, altyapısız ortamlarda çalışan düşük bütçeli robot ekiplerine hangi koşulda hangi bırakma kuralının ve ağ protokolünün tercih edilmesi gerektiğine dair ölçülmüş bir dayanak sunacaktır.
```

---

## EKRAN 06 — Araştırma Önerisi

### 1.1 Konunun Önemi ve Araştırma Önerisinin Bilimsel Niteliği (en az 350 karakter)

```text
Maden, tünel, göçük altındaki yapılar ve bodrum katları gibi kapalı ve engelli ortamlarda insanlı müdahale risklidir; bu nedenle arama-kurtarma ve keşif görevlerinde uzaktan kumandalı ya da otonom robotlar giderek daha çok kullanılmaktadır. Bu ortamlarda robotun temel darboğazı haberleşmedir: beton, metal ve köşeler kablosuz sinyali hızla zayıflatır; robot görüş hattından çıktığında bağlantı kısa mesafede kopabilir ve robot hem kumandayı hem de görüntü ve veri akışını kaybeder. DARPA Yeraltı Yarışması'nın düzenleyicileri de haberleşmeyi yeraltı ortamlarında ekiplerin karşılaştığı temel teknik zorluklardan biri olarak raporlamıştır (Chung vd., 2023).

Bu sorun Türkiye için özellikle önemlidir. 6 Şubat 2023 Kahramanmaraş depremleri göçük altındaki yapılarda, Soma (2014) ve Amasra (2022) maden kazaları ise yeraltında arama-kurtarma ihtiyacını açıkça göstermiştir. Bu ortamlarda çalışan robotların haberleşmesini düşük maliyetle sürdürmeyi amaçlayan proje, TÜBİTAK öncelikli alanlarından Afet Yönetimi Teknolojileri ile doğrudan, Robotik-Mekatronik ve Nesnelerin İnterneti alanlarıyla dolaylı olarak ilişkilidir. Projenin bu alana katkısı, harcanabilir ve düşük maliyetli düğümlerle kurulan röle zincirlerinin hangi koşulda yeterli olduğuna dair ölçülmüş veri sağlamasıdır.

Bu soruna yaygın bir çözüm, robotun ilerlerken arkasına röle düğümleri bırakarak çok atlamalı bir ağ kurmasıdır; yaklaşım literatürde "breadcrumbing" olarak anılır. Görüş hattı olmayan ortamlarda insansız kara aracının arkasına radyo rölesi bırakan sistemler (Pezeshkian vd., 2007) ve bırakma kararını RSSI ile video veri hızından tahmin edilen bağlantı kalitesine dayandıran yöntemler (Pezeshkian vd., 2012) önerilmiştir. Hareketli röle robotlarının Wi-Fi sinyal modeline göre konumlandırılması (Gao vd., 2017), bağlantı ölçümleriyle doğrusal röle ağlarının kurulması (Kim vd., 2013) ve çok atlamalı röle robotlarının sinyali zayıflatan binalarda yerleştirilmesi (Imaizumi vd., 2013) de incelenmiştir. Bağlantı farkındalıklı yerleştirmeyi mesafe temelli yerleştirmeyle karşılaştıran CARA algoritması (Abu-Aisheh vd., 2022) ve iç mekânda RSSI'yi Gauss süreç regresyonuyla öngörerek bağlantıyı koruyan model öngörülü kontrol yaklaşımı (Kim vd., 2026) bu alandaki güncel örneklerdir. DARPA Yeraltı Yarışması'nı kazanan CERBERUS ekibi de robotların taşıyıp bıraktığı Wi-Fi düğümleriyle ağını genişletmiştir (Tranzatto vd., 2022a).

Bununla birlikte, bırakma kararının nasıl verileceği açık bir sorudur. Kazanan ekip, kablosuz yayılımın yeraltında öngörülemez olması nedeniyle düğüm bırakmayı otomatikleştirmemiş; final etabında düğümlerin nereye ve ne zaman bırakılacağına insan operatör karar vermiştir. Aynı ekibin değerlendirmesinde, yarışmadaki başka bir ekibin mesafe, görüş hattı ve RSSI'yi birleştiren bir maliyet fonksiyonu kullandığı aktarılmaktadır (Tranzatto vd., 2022b). Otomatik bırakmanın uygulanabilir olduğu daha önce gösterilmiş olsa da (Pezeshkian vd., 2012), farklı bırakma kurallarından hangisinin hangi ortamda daha az röleyle daha kesintisiz bağlantı sağladığı, aynı donanım üzerinde kontrollü ve tekrarlı deneylerle yeterince karşılaştırılmamıştır. Bırakma kuralı tek başına da belirleyici değildir: düğümler arasındaki ağ protokolü, yeni bırakılan rölenin ağa ne kadar hızlı katıldığını ve bir röle bozulduğunda bağlantının ne kadar sürede toparlandığını belirler. Ayrıca aynı kanalı paylaşan 802.11 düğümleri birbiriyle yarıştığı için çok atlamalı zincirlerde atlama sayısı arttıkça verim düşer (Li vd., 2001); yani röle eklemek sorunu kendiliğinden çözmez.

Bu problemde düşük maliyet bir tercih değil, gerekliliktir. Afet ve keşif görevlerinde bırakılan düğümler çoğu zaman geri toplanamaz, kaybolabilir ya da zarar görebilir. CERBERUS ekibinin düğümleri ticari bir mesh radyosu taşıyan, 245 g ağırlığında ve 120 × 60 × 50 mm boyutunda birimlerdir ve bir robot bunlardan en fazla dördünü taşıyabilmiştir (Tranzatto vd., 2022b). Birim fiyatı birkaç yüz lira olan Wi-Fi mikrodenetleyicileri ise hafif, harcanabilir ve çok sayıda taşınabilir düğümler yapmaya olanak verir (Seeed Studio, t.y.); ancak bu donanımla kurulan zincirlerin hangi mesafeyi hangi veri kalitesiyle sağlayabildiği yeterince bilinmemektedir.

Bu projede ESP32-C3 Wi-Fi mikrodenetleyicileriyle bir breadcrumbing sistemi kurulacak; üç bırakma kuralı (sabit mesafe, RSSI eşiği, RSSI eğilimine dayalı öngörü) ile üç ağ yaklaşımı (hazır ESP-WIFI-MESH, ham ESP-NOW zinciri ve bu projede geliştirilecek, bırakma sırasını bilen SBZ protokolü) üç farklı kapalı ortamda karşılaştırılacaktır (Espressif Systems, t.y.-a; Espressif Systems, t.y.-b). Projenin özgün yönleri; bırakma kurallarının harcanabilir, düşük maliyetli donanımda ve aynı deney düzeneğinde kontrollü ve tekrarlı deneylerle karşılaştırılması, robotun bırakma sırasını doğrudan ağ protokolüne aktaran SBZ protokolü, bırakma kuralı ile protokolün etkisinin birlikte ölçülmesi ve tüm ham verinin açık veri seti olarak paylaşılmasıdır.

Araştırma soruları: Projenin başarısı iki birincil soruyla ölçülecektir. (AS1) Aynı sabit röle zincirinde hangi ağ yaklaşımı paket teslimi, verim, gecikme ve arıza sonrası toparlanmayı en iyi dengeler? (AS2) Hangi bırakma kuralı en az röleyle en kesintisiz bağlantıyı sağlar ve bu sonuç ortama göre değişir mi? Bunlara ek olarak iki ikincil soru incelenecektir. (AS3) Bırakma kuralının etkisi protokole bağlı mıdır; öngörüye dayalı bırakma, yeni röleyi ağa yavaş katan bir protokolün açığını kapatabilir mi? (AS4) Atlama sayısı arttıkça zincirin taşıyabildiği en yüksek veri hızı nasıl düşer?

Hipotezler: (H1, AS1) SBZ'de yeni röleye bağlanma ve arıza sonrası toparlanma süreleri ESP-WIFI-MESH'tekinden kısadır; buna karşılık SBZ'nin uygulama düzeyindeki yeniden gönderimleri p95 gidiş-dönüş gecikmesini ham ESP-NOW zincirine göre artırır. (H2, AS2) RSSI eşiği ve eğilim kuralları, L dönüşlü koridorda ve beton bodrumda sabit mesafeye göre daha az röleyle daha kısa toplam kesinti sağlar; düz koridorda fark küçüktür. (H3, AS2) Eğilim kuralı, eşik kuralına göre daha kısa kesintiler üretir. (H4, AS3) Eğilim kuralının eşik kuralına göre sağladığı kesinti azalması ESP-WIFI-MESH'te SBZ'dekinden büyüktür; çünkü erken bırakma, yeni rölenin ağa yavaş katılmasını telafi eder. (H5, AS4) Altı atlamada en yüksek sürdürülebilir veri hızı, tüm protokollerde tek atlamadakinin yarısının altına düşer.

Çalışmanın kapsamı 2,4 GHz Wi-Fi, en fazla beş röle ve kampüs içindeki kapalı ortamlarla sınırlıdır; kamera yükü sentetik paket trafiğiyle temsil edilecektir. Rotalar 40–60 m ile sınırlı olduğundan, gerektiğinde düşürülen verici gücü daha uzun rotaların ölçeklenmiş bir modeli olarak kullanılacaktır. Robot kablolu kumandayla sürüleceği için kumanda bağlantısı röle zincirinden geçmeyecek, zincir yalnızca robotun veri akışını taşıyacaktır. Bu sınırlılıklar sonuçlarla birlikte raporlanacaktır.
```

### 1.2 Amaç ve Hedefler (en az 350 karakter)

```text
Bu projenin amacı, düşük maliyetli Wi-Fi mikrodenetleyicilerle robottan bırakılan röle zincirlerinin (breadcrumbing) kapalı ve engelli ortamlarda bağlantıyı ne kadar uzağa ve hangi kalitede taşıyabildiğini ölçmek; bırakma kuralının ve ağ protokolünün bu başarıya etkisini kontrollü deneylerle ortaya koymaktır.

Hedef 1 – Çalışan prototip: Beş röle düğümü, bırakıcı robot ve revolver tipi bırakma mekanizmasından oluşan sistemin kurulması. Doğrulama: üç protokolün beş rölelik zincirde veri iletmesi; mekanizmanın her bölmeden 20 olmak üzere 100 bırakmalık testte en az 95 bırakmada takılmadan ve düğümü düz bırakarak çalışması.

Hedef 2 – Kapasite eğrisi (AS4, ikincil): Düz koridorda 1, 3 ve 6 atlamada her protokolün, paket teslim oranı %95'in altına düşmeden taşıyabildiği en yüksek veri hızının ölçülmesi ve bu hızların 360p ve 480p görüntü akışı için gereken tipik hızlarla karşılaştırılması (3 protokol × 3 atlama sayısı × 5 tekrar = 45 tarama). H5 bu ölçümle test edilecektir.

Hedef 3 – Protokol karşılaştırması (AS1, birincil): Aynı sabit zincirde üç protokolün bağlanma, toparlanma ve geri katılma sürelerinin; kararlı durumda paket teslim oranı, goodput ve gidiş-dönüş gecikmesinin (p50, p95) üç ortamda ölçülmesi (3 protokol × 3 ortam × 5 tekrar = 45 koşu). H1 karma etkili model, etki büyüklüğü ve %95 güven aralığıyla test edilecektir.

Hedef 4 – Bırakma kuralı karşılaştırması (AS2 birincil, AS3 ikincil): Üç bırakma kuralının iki protokolle ve üç ortamda robotla uygulanması (2 × 3 × 3 × 5 = 90 koşu); kullanılan röle sayısı, kesinti sayısı ve toplam kesinti süresi, görev başarısı, ulaşılan mesafe ve aynı ortamda gözlenen en iyi yerleşime göre fazla röle sayısı bakımından karşılaştırılması. H2, H3 ve protokol × kural etkileşimine ilişkin H4 test edilecektir.

Hedef 5 – Açık ve tekrarlanabilir çıktı: SBZ protokolünün, deney yazılımlarının ve 3D baskı dosyalarının açık kaynak olarak; yaklaşık 180 koşuluk ham ölçüm verisinin ve deney protokolünün başkalarının tekrarlayabileceği biçimde yayımlanması. Doğrulama: kod ve veri setinin proje sonunda herkese açık bir depoda erişilebilir olması.
```

### 2. Yöntem (en az 350 karakter)

```text
2.1 Araştırma tasarımı
Çalışma, kampüs içindeki gerçek kapalı ortamlarda yürütülecek deneysel ve karşılaştırmalı bir saha araştırmasıdır. Bağımsız değişkenler ağ protokolü (3 düzey), bırakma kuralı (3 düzey), ortam (3 düzey) ve kapasite testinde atlama sayısı (1, 3, 6) ile veri yüküdür. Bağımlı değişkenler paket teslimi, verim, gecikme, bağlantı sürekliliği ve yerleştirme başarısıdır (2.8). Kart modeli, anten ve anten yönü, düğüm yüksekliği, kanal, verici gücü, fiziksel katman (PHY) veri hızı, paket boyutu, ortak veri yükü, robot hızı ve router durumu sabit tutulacaktır. Uyarlamalı kurallar her protokolde röleleri farklı konumlara bırakacağından protokol ve kural etkileri birbirine karışabilir. Bunu önlemek için önce protokoller sabit röle konumlarında (Aşama 1), ardından bırakma kuralları robotla (Aşama 2) karşılaştırılacaktır.

2.2 Donanım
Tüm ağ uçlarında aynı kart kullanılacaktır: harici U.FL antenli ve dahili LiPo şarj devreli XIAO ESP32-C3 (Seeed Studio, t.y.). Sistemde 8 kart bulunur: bilgisayara USB ile bağlı merkez düğüm, beş röle, robot ağ kartı ve bir yedek. Böylece en fazla beş röleyle altı kablosuz atlamalı bir zincir kurulur. Her röle, 1000 mAh korumalı LiPo pil, aç/kapa anahtarı ve yaklaşık 60 × 40 × 15 mm boyutunda 3D baskı bir kutudan oluşur. Bırakıcı robot, iki enkoderli mikro DC motorla diferansiyel sürüşlü, 3D baskı şasili ve 3S Li-ion pilli bir araçtır; operatör robotu 2,4 GHz bandını kullanmayan kablolu bir kumandayla sürer. Robotta iki kart vardır: rölelerle aynı olan ağ kartı sentetik trafiği üretir ve bırakma kararını verir; Wi-Fi ve Bluetooth'u hiç açılmayan ESP32-S3 sürüş kartı ise motorları, enkoderleri ve bırakma mekanizmasını yönetir ve ağ kartıyla UART üzerinden haberleşir. Bu ayrım, robot ucundaki radyonun rölelerle aynı kalmasını ve motor kontrolünün ağ ölçümlerinin zamanlamasını etkilememesini sağlar. Bırakma mekanizması, NEMA 17 step motorla dönen, 6 bölmeli (5 röle + 1 boş) revolver tipi bir döner tabladır; altında tek açıklıklı sabit bir taban bulunur. Her bırakmada tabla 60° döner ve sıradaki röle açıklıktan zemine düz düşer; böylece anten yönelimi her bırakmada aynı kalır. Tabla her koşudan önce hizalama işaretiyle başlangıç konumuna getirilir; hedef açılar bu konuma göre mutlak adım sayısıyla hesaplanarak hata birikmesi önlenir.

2.3 Karşılaştırılacak ağ yaklaşımları
Üç yaklaşım aynı radyoyu kullanır; farkları ağ kurma ve veri aktarma yöntemindedir. ESP-NOW varsayılan olarak 1 Mbit/s hızında gönderirken ESP-WIFI-MESH daha yüksek 802.11n hızlarını kullandığından, fiziksel katman hızı üç yaklaşımda aynı değere sabitlenecektir. Sabitlemenin her yöntemde mümkün olduğu İP1'de doğrulanacak, kullanılan değer her koşuda kaydedilecektir. P1 ESP-WIFI-MESH, ebeveyn seçimini ve ağ onarımını kendisi yapan hazır mesh protokolüdür (Espressif Systems, t.y.-b); merkez sabit kök olarak atanacaktır. Bu protokolde kök normalde bir router'a bağlanır; router'sız çalışma ilk iş paketinde denenecek, gerekirse deneye özel bir router tüm protokollerin bütün koşularında açık tutulacaktır. P2 ham ESP-NOW zinciri, ESP-NOW bağlantısız çerçeveleriyle (Espressif Systems, t.y.-a) kurulan sade bir temel yöntemdir: robot her röleyi bıraktığında ona yalnızca üst düğümünü (k−1) bildirir, ağ keşfi yapılmaz ve her röle paketi bu üst düğüme iletir. ESP-NOW'un MAC katmanındaki onay ve yeniden deneme davranışı dışında uygulama düzeyinde yeniden gönderim ve düğüm atlama yoktur. P3 SBZ (Sıra Bilen Zincir) bu projede geliştirilecek protokoldür ve P2'ye üç özellik ekler: (i) robot bıraktığı röleye üst düğümünün yanı sıra onun bir üstündeki düğümü (k−2) de bildirir; (ii) atlama başına sınırlı sayıda uygulama düzeyi yeniden gönderim yapılır; (iii) düğüm atlama: üst düğüme art arda N gönderim başarısız olursa paket k−2 düğümüne gönderilir ve eski düğüm belirli aralıklarla yeniden denenir. P2 ve P3 aynı kod tabanından ayarla elde edileceğinden P2–P3 farkı yeniden gönderim ve düğüm atlamanın etkisini doğrudan ayrıştırır. P1–P2 farkı ise tek bir özelliğe indirgenemez; kendini kuran mesh yaklaşımı ile bırakma sırasını bilen keşifsiz zincir yaklaşımı arasındaki toplam farkı (keşif, ağaç yönetimi ve protokol yükü) gösterir. Tüm protokollerde uygulama verisi 160 bayttır ve aynı başlık alanları (koşu no, sıra no, kaynak, hedef, atlama sayısı, ömür) kullanılır. Tamburdaki röleler açık ama bekleme modundadır ve ağa katılmaz; bırakma anında robotun gönderdiği "aktif ol" komutuyla seçilen protokolü başlatır. Böylece henüz bırakılmamış röleler ağ topolojisini bozmaz.

2.4 Bırakma kuralları
S1 sabit mesafe: son bırakmadan sonra enkoder X metre gösterdiğinde röle bırakılır; mesafe temelli karşılaştırma yöntemidir (Abu-Aisheh vd., 2022). S2 RSSI eşiği: son röleden gelen yoklama paketlerinin 1 saniyelik ortalama RSSI değeri 3 saniye boyunca eşiğin altında kalırsa röle bırakılır (Pezeshkian vd., 2012). S3 eğilim: RSSI üstel ağırlıklı hareketli ortalamayla yumuşatılır, son 3–5 saniyedeki eğimden eşiğe ulaşma süresi tahmin edilir; bu süre T'nin altına inerse ya da paket kaybı artmaya başlarsa eşiğe gelmeden röle bırakılır. Başlangıç değerleri (X ≈ 12 m, eşik ≈ −75 dBm, T ≈ 3 s) pilotta önceden yazılmış kurallarla kesinleştirilecektir. Tüm kurallarda iki bırakma arasında asgari bir mesafe bulunur, tek bir ölçümle karar verilmez ve RSSI, son bırakılan rölenin çerçevelerinden her protokolde aynı yöntemle okunur. Kuralların röle kullanımındaki verimliliği ayrıca, aynı ortamda hedefe bağlantıyla ulaşan koşularda gözlenen en az röle sayısına (en iyi gözlenen yerleşim) göre fazla röle sayısıyla değerlendirilecektir.

2.5 Ortamlar ve veri yükü
Üç ortam kullanılacaktır: O1 düz koridor (görüş hattı açık), O2 L dönüşlü koridor (köşede görüş hattı aniden kopar) ve O3 betonu yoğun bodrum ya da çok kapılı koridor (sürekli yüksek zayıflama). Rota uzunluğu 40–60 m hedeflenmektedir; sinyal beklenenden güçlü çıkarsa verici gücü tüm protokoller için eşit olarak düşürülecektir. Her ortamın rotası, engelleri, kanal durumu ve ortamdaki Wi-Fi ağları kaydedilecektir. Gerçek kamera kullanılmayacak; görüntü akışı 160 baytlık paketlerle üretilen, hızı ayarlanabilir sentetik trafikle temsil edilecektir (başlangıç değeri 128 kbit/s). Tüm protokoller ve kurallar aynı ortak yükle test edilecek; bu yük, Aşama 1A'da en zayıf protokolün altı atlamadaki en yüksek sürdürülebilir hızının yaklaşık %70'i olarak önceden belirlenen kuralla seçilecektir. İnsan vücudu 2,4 GHz sinyali zayıflattığından operatör robotun önünde ya da yanında yürüyecek, robot ile son röle arasına girmeyecektir.

2.6 Deney aşamaları
Aşama 0 – Kalibrasyon ve pilot (robotsuz): Her ortamda bir düğüm sabitken ikinci düğüm zemin seviyesinde metre metre uzaklaştırılarak mesafe–RSSI ve RSSI–paket teslim eğrileri çıkarılacaktır (ortam başına 2 tur). Eşik, X, ortak yük, kanal ve verici gücü bu eğrilerden önceden yazılmış kurallarla seçilecektir. Pilot verileri hipotez testlerinde kullanılmayacaktır.
Aşama 1 – Statik zincir (robotsuz): Röleler, pilotta belirlenen ve komşu çiftlerin benzer sinyal seviyesinde olduğu konumlara lazer metreyle yerleştirilecek; aralık, düğüm atlamanın sınanabilmesi için her rölenin k−2 düğümüyle de bağlantı kurabileceği biçimde seçilecektir. Değişen tek şey protokol olacaktır. 1A Kapasite (yalnızca O1): 1, 3 ve 6 atlamada yük 32 kbit/s'den başlayarak 30 saniyede bir yaklaşık 1,4 katına çıkarılır. Taramanın üst sınırı, tek atlamada da paket teslim oranının %95'in altına düşeceği hıza göre pilotta belirlenir; böylece eğri tavana takılmaz. Paket teslim oranı %95'in altına düşmeden önceki son yük en yüksek sürdürülebilir hız kabul edilir. Atlama sayısının denetlenebilmesi için bu testte ESP-WIFI-MESH'te üst düğümler sabitlenecektir (3 protokol × 3 atlama sayısı × 5 tekrar = 45 tarama). 1B Birleşik test: Her 10 dakikalık koşuda olaylar röle yazılımına programlanır. 60. saniyede beşinci röle bekleme modundan çıkar (bağlanma süresi); 60–360 s arasında kararlı durum ölçülür; 360. saniyede üçüncü röle Wi-Fi'ını kapatarak arıza benzetir (toparlanma süresi) ve 480. saniyede yeniden açar (geri katılma süresi) (3 protokol × 3 ortam × 5 tekrar = 45 koşu). Böylece koşular elle müdahale olmadan, tek kişiyle yürütülebilir.
Aşama 2 – Robotla dinamik deney: Robot beş röleyle merkezin yanından başlar. Operatör rotayı yazılımla sabitlenmiş hızla (~0,3 m/s) sürer; robot ağ kartı ortak yükte veri gönderirken seçili kural bırakma kararını verir. Karar çıkınca robot durur, tabla döner, röle düşer ve aktif edilir, bırakma konumu kaydedilir. Hedefe bağlantıyla ulaşılırsa koşu başarılı, beş röle kullanıldıktan sonra bağlantı koparsa başarısız sayılır ve ulaşılan mesafe kaydedilir. Bu aşamada hazır yöntem (P1) ile önerilen protokol (P3) üç kuralla, üç ortamda beşer kez denenecektir (90 koşu). P2, SBZ'nin ek özellikleri kapalı hâli olduğundan katkısı Aşama 1'de ayrıştırılmış olacaktır. Projede toplam yaklaşık 180 koşu yapılacaktır.

2.7 Ayarların dondurulması
Pilotta belirlenen tüm ayarlar, ana deneylerden önce tarihli bir ayar belgesine yazılacak ve ana deneyler boyunca değiştirilmeyecektir. Ana deneyde bir yöntemin kötü sonuç vermesi ayar değişikliği gerekçesi değil, bulgu sayılacaktır. Zorunlu bir değişiklik gerekirse ilgili bloktaki tüm koşullara uygulanacak, blok yeniden yapılacak ve raporlanacaktır.

2.8 Ölçütler
Her paket benzersiz bir sıra numarası ve zaman damgası taşır; merkez tüm paketleri kaydeder, koşu özeti betikle hesaplanır. Paket teslim oranı, merkeze ulaşan benzersiz paketlerin gönderilenlere oranıdır; tanım, RFC 7680 kayıp ölçütünün uygulama katmanına uyarlamasıdır (Almes vd., 2016). Goodput, merkeze ulaşan uygulama verisinin süreye oranıdır. Kartların saatleri eş zamanlı olmadığından tek yönlü gecikme iddia edilmeyecek; yankı paketleriyle gidiş-dönüş süresi (RTT; p50 ve p95) ve ardışık RTT farklarından gecikme değişkenliği (Demichelis ve Chimento, 2002) ölçülecektir. 500 ms'den uzun süre paket gelmeyen aralıklar kesinti sayılacak, kesinti sayısı ve toplam süresi hesaplanacaktır. Aşama 1B'de bağlanma, toparlanma ve geri katılma süreleri; Aşama 2'de kullanılan röle sayısı, bırakma konumları, görev başarısı, ulaşılan en uzak mesafe, röle başına mesafe ve en iyi gözlenen yerleşime göre fazla röle sayısı raporlanacaktır. Her hipotezin birincil ölçütü önceden belirlenmiştir: H1 için bağlanma ve toparlanma süreleri ile p95 RTT, H2 ve H3 için toplam kesinti süresi, H4 için eğilim ve eşik kuralları arasındaki toplam kesinti farkı, H5 için en yüksek sürdürülebilir hız. Yeniden gönderim sayısı, ebeveyn değişimi ve gerçekleşen topoloji sonuçları açıklamak için ikincil olarak kaydedilecektir.

2.9 Analiz
Aşama 1'de protokol × ortam, Aşama 2'de protokol × kural × ortam etkileri incelenecektir. Koşular farklı günlere yayıldığından gün, karma etkili modelde rastgele etki olarak ele alınacaktır. Gün etkisinin koşullarla karışmaması için her deney gününde bir ortamdaki tüm koşulların birer tekrarı rastgele sırayla yürütülecek, böylece her gün bir tam blok oluşturacaktır. Ölçütün türüne uygun genelleştirilmiş karma modeller kullanılacaktır: süreler için log dönüşümü, röle ve kesinti sayıları için Poisson, görev başarısı için binom dağılımı. Birincil ölçütlerdeki karşılaştırmalarda Holm düzeltmesi uygulanacak, diğer karşılaştırmalar keşif amaçlı raporlanacaktır. Her sonuç p değerinin yanında etki büyüklüğü ve %95 güven aralığıyla verilecektir. Kapasite sonuçları atlama sayısına göre çizilecek ve çok atlamalı ağlarda beklenen verim düşüşüyle (Li vd., 2001) karşılaştırılacaktır. Protokol ve kural etkileşimi her ortam için etkileşim grafiğiyle gösterilecektir.

2.10 Tekrarlanabilirlik ve iş paketleriyle ilişki
Tüm kartlar aynı ESP-IDF sürümüyle programlanacak; kanal, verici gücü, PHY hızı, router durumu, anten yönü, pil durumu ve ortam bilgileri her koşuda kaydedilecek; ham kayıtlar ve betikler korunacaktır. Ön testler, ölçüm altyapısı ve robotun temel sürüşü İP1'de; protokoller ve bırakma mekanizması İP2'de; bırakma kuralları ve pilot İP3'te; Aşama 1 İP4'te; Aşama 2 İP5'te; analiz İP6'da yürütülecektir. Projede henüz ön çalışma sonucu bulunmamaktadır; ilk ölçümler İP1'de alınacaktır.
```

### 3.1 Çalışma Takvimi

Her satır için kırmızı **+ Add** düğmesine basın. Süreler 2 + 2 + 1 + 2 + 3 + 2 = **12 ay**, katkılar toplamı **%100**. Literatür taraması, malzeme alımı ve rapor yazımı ayrı satır olarak yazılmadı; kurallar bunu istemiyor.

**İP1**
- Proje Süresi (Ay): `1–2`
- Faaliyetler:
```text
Donanım ön testi ve ortak ölçüm altyapısı: İki kartla ESP-NOW ve ESP-WIFI-MESH ön testleri (router'sız sabit kök denemesi dâhil) ve ESP-IDF sürümünün sabitlenmesi; fiziksel katman hızının üç yöntemde aynı değere sabitlenebildiğinin doğrulanması; deney ortamları için erişim izninin alınması; ortak paket biçimi, merkezde paket kaydı, RTT ölçümü ve koşu özetini üreten betiklerin yazılması; RSSI'nin tüm protokollerde aynı yöntemle okunabildiğinin doğrulanması; 3D baskı robot şasisinin enkoderli motorlar, motor sürücü, sürüş kartı, güç sistemi ve kablolu kumandayla kurulup ilk sürüş testlerinin ve enkoder kalibrasyonunun yapılması.
```
- Kim(ler): `Aleyna (ESP-NOW ön testleri ve ortak paket biçimi), Hatice (ESP-WIFI-MESH ön testi), Hacer (robot şasisi ve sürüş testleri), Mustafa (paket kaydı ve koşu betikleri)`
- Başarı ölçütü ve katkı:
```text
İki kart arasında her iki yöntemle veri aktarıldı; router gereksinimi ve ortak PHY hızı belirlendi; paket kaydı ve koşu özeti dosyası çalışıyor; robot kablolu kumandayla sürülüyor ve enkoder mesafesi kaydediliyor. Katkı: %10
```

**İP2**
- Proje Süresi (Ay): `3–4`
- Faaliyetler:
```text
Ağ protokolleri ve bırakma mekanizması: SBZ protokolünün (ham ESP-NOW zinciri ayarı dâhil) ve ESP-WIFI-MESH uygulamasının yazılması; bekleme modu ve "aktif ol" komutunun eklenmesi; revolver tipi bırakma mekanizmasının ve düğüm kutularının 3D tasarımı ve baskısı; mekanizmanın step motor ve sürücüsüyle robota monte edilmesi ve kabul testi.
```
- Kim(ler): `Aleyna (SBZ protokolü), Hatice (ESP-WIFI-MESH uygulaması ve bekleme modu), Hacer (mekanizmanın elektroniği, montajı ve kabul testi), Mustafa (mekanizma ve düğüm kutularının 3D tasarımı ve baskısı)`
- Başarı ölçütü ve katkı:
```text
Üç protokol beş rölelik zincirde veri iletiyor; bekleme modu ve "aktif ol" komutu üç protokolde çalışıyor; mekanizma her bölmeden 20 olmak üzere 100 bırakmanın en az 95'inde takılmadan çalışıyor. Katkı: %20
```

**İP3**
- Proje Süresi (Ay): `5`
- Faaliyetler:
```text
Yerleştirme kuralları, kalibrasyon ve pilot: Sabit mesafe, RSSI eşiği ve eğilim kurallarının robot ağ kartına uygulanması; üç ortamda mesafe–RSSI ve RSSI–paket teslim eğrilerinin çıkarılması; ortak yük, eşik, X, kanal ve verici gücü ayarlarının ve kapasite taramasının üst sınırının kurallarla belirlenip tarihli ayar belgesinde dondurulması.
```
- Kim(ler): `Hatice (kurallar ve sinyal haritası), Hacer (kuralların robota entegrasyonu), Aleyna (ayar belgesi)`
- Başarı ölçütü ve katkı:
```text
Üç ortamın eğrileri çıkarıldı ve ayarlar donduruldu; üç kural robotta tekrarlanabilir bırakma kararı üretiyor. Katkı: %15
```

**İP4**
- Proje Süresi (Ay): `6–7`
- Faaliyetler:
```text
Aşama 1 statik zincir deneyleri: Düz koridorda 1, 3 ve 6 atlamada kapasite taramaları (45) ve üç ortamda bağlanma, kararlı durum, arıza ve geri katılma olaylarını içeren otomatik birleşik testler (45).
```
- Kim(ler): `Aleyna, Hatice, Mustafa`
- Başarı ölçütü ve katkı:
```text
90 koşu tamamlandı; ham kayıtlar ve koşu özetleri kontrol edildi; en yüksek sürdürülebilir hızlar ile bağlanma ve toparlanma süreleri hesaplandı. Katkı: %20
```

**İP5**
- Proje Süresi (Ay): `8–10`
- Faaliyetler:
```text
Aşama 2 robotlu dinamik deneyler: İki protokol (ESP-WIFI-MESH, SBZ) × üç bırakma kuralı × üç ortam × beş tekrar.
```
- Kim(ler): `Hacer, Hatice (robot sürüşü ve bırakma), Aleyna, Mustafa (merkez kaydı ve ağ izleme)`
- Başarı ölçütü ve katkı:
```text
90 koşu tamamlandı; her koşuda bırakma konumları, kesintiler, kullanılan röle sayısı ve görev sonucu kaydedildi. Katkı: %25
```

**İP6**
- Proje Süresi (Ay): `11–12`
- Faaliyetler:
```text
İstatistiksel analiz ve açık veri: Karma etkili modellerle hipotez testleri, etki büyüklükleri ve güven aralıkları; kuralların en iyi gözlenen yerleşimle karşılaştırılması; veri seti, kod, 3D baskı dosyaları ve deney protokolünün belgelenerek herkese açık depoda yayımlanması.
```
- Kim(ler): `Aleyna (analiz, kod ve veri setinin yayımı), Hatice (analiz), Hacer (donanım belgeleri), Mustafa (3D baskı dosyaları)`
- Başarı ölçütü ve katkı:
```text
H1–H5 test edildi, etki büyüklükleri raporlandı; veri seti, kod ve tasarım dosyaları herkese açık depoda erişilebilir. Katkı: %10
```

### 3.2 Risk Yönetimi

Her satır için **+ Add** düğmesine basın.

| En Önemli Riskler | Alınacak Tedbirler (B Planı) |
|---|---|
| ESP-WIFI-MESH'in router olmadan, sabit kökle kararlı çalışmaması | Deneye özel bir router merkezin yanına konur ve adillik için üç protokolün bütün koşularında açık tutulur. Mesh yine kararlı çalışmazsa aynı rolü üstlenen ESP-Mesh-Lite'a geçilir (Espressif Systems, t.y.-c); karşılaştırmanın yapısı değişmez. |
| Bırakma mekanizmasının veya robotun çalışmaması ya da gecikmesi (3D baskı erişiminin aksaması dâhil) | 3D yazıcıya erişilemezse parçalar başka bir atölyede basılır; filament bütçede yer aldığından maliyet değişmez. Mekanizma yine de çalışmazsa robot durur ve düğüm aynı noktaya elle bırakılır; bırakma kararı ve kayıt değişmediği için araştırma sorusu etkilenmez. Robot tamamen arızalanırsa ağ kartı aynı yükseklikte elle itilen bir arabaya taşınır. Statik deneyler (İP4) robottan bağımsız olduğu için bekletilmez. |
| Ölçümlerde ortam kaynaklı yüksek değişkenlik (çevredeki Wi-Fi ağları, insan hareketi) | En az kullanılan kanal seçilir; her koşul 5 kez tekrarlanır; gün etkisi karma etkili modelle hesaba katılır; operatör konumu kuralı uygulanır; koşu saati ve ortamdaki ağlar kaydedilir. |
| Kampüs koridorlarının kısa kalması veya sinyalin röle gerektirmeyecek kadar güçlü çıkması | Verici gücü tüm protokoller için eşit olarak düşürülür; ayar pilotta belirlenip dondurulur. |
| Pilotta bir protokolün çok atlamada kararlı çalışmaması | Ayarlar pilotta kurallarla belirlenir; ana deneyde kötü sonuç bulgu olarak raporlanır. Protokol hiç çalışmazsa önceden belirlenen yedek yönteme geçilir; bu da olmazsa karşılaştırma kalan yöntemlerle sürdürülür ve sınırlılık olarak raporlanır. |
| Bırakma sırasında röle kutusunun veya kartın zarar görmesi (proje boyunca yaklaşık 450 bırakma) | Bırakma yüksekliği düşük tutulur, kutu darbeye dayanıklı basılır ve her koşudan sonra röleler kontrol edilir. Kırılan kutu bütçedeki filamentle yeniden basılır; kart arızalanırsa yedek kart devreye alınır. |
| Deney ortamlarına (koridor, bodrum) erişim izninin alınamaması ya da ortamın kullanıma kapanması | Erişim izni İP1'de fakülte yönetiminden yazılı olarak alınır; her ortam türü için kampüste bir yedek bina önceden belirlenir. |
| Bir ekip üyesinin projeden ayrılması veya robotlu koşuların (İP5) gecikmesi | Her görev için bir yedek sorumlu belirlenir; kod, kayıtlar ve belgeler ortak depoda tutulur. Koşular haftalık hedef sayıyla izlenir; analiz betikleri İP4'te hazır olduğundan İP6'nın ilk ayı gerekirse tampon olarak kullanılır. |

### 3.3 Araştırma Olanakları

> ⚠️ **Köşeli parantezli yerleri gerçek bilgilerle doldurun.** Elinizde olmayan bir şeyi yazmayın; rehber bunu açıkça yasaklıyor.
>
> **3D yazıcı:** Üniversitede gerçekten bir yazıcı varsa (bölüm, kulüp, kütüphane) birinci satırı onunla doldurun. Yoksa arkadaşınızın yazıcısının modelini yazın. Satır iki durumda da doğru kalır.

| Kuruluşta Bulunan Altyapı/Ekipman Türü – Modeli | Projede Kullanım Amacı |
|---|---|
| 3D yazıcı – [marka/model] | Bırakma mekanizması, düğüm kutuları ve montaj parçalarının basılması |
| [Bölüm] elektronik laboratuvarı (lehim istasyonu, güç kaynağı, multimetre) | Pil bağlantıları, robot elektroniğinin montajı ve testleri |
| [Bina] düz ve L dönüşlü koridorları, [bina] bodrum katı | Deney ortamları (O1, O2, O3) |
| Ekip üyelerinin dizüstü bilgisayarları; açık kaynak ESP-IDF, Python ve 3D tasarım yazılımı | Yazılım geliştirme, mekanik tasarım, merkez düğüm kaydı ve veri analizi |

### 4. Yaygın Etki

**Bilimsel/Akademik Çıktılar**
```text
Protokol ve bırakma kuralı karşılaştırmasının sonuçlarının ulusal bir kongrede bildiri olarak sunulmak üzere raporlanması; sonuçlar yeterli görülürse kablosuz ağlar veya robotik alanında hakemli bir dergiye makale taslağı hazırlanması. Yaklaşık 180 koşuluk ham ölçüm verisi ve deney protokolü, düşük maliyetli donanımda breadcrumbing üzerine tekrarlanabilir bir karşılaştırma tabanı olarak açık erişimle paylaşılacaktır.
```

**Ekonomik/Ticari/Sosyal Çıktılar**
```text
Düğüm başına yaklaşık 650 TL malzeme maliyetiyle çalışan bir breadcrumbing prototipi (5 röle düğümü, bırakıcı robot ve revolver tipi bırakma mekanizması); açık kaynak SBZ protokolü ve deney yazılımları; düğüm kutusu ve mekanizmanın 3D baskı dosyaları. Bu çıktılar; arama-kurtarma, maden ve tünel gibi altyapısız ortamlarda çalışan düşük bütçeli robot ekiplerine, öğrenci topluluklarına ve robot yarışması ekiplerine, hangi koşulda hangi bırakma kuralının ve ağ protokolünün tercih edilmesi gerektiğine dair ölçülmüş bir dayanak sunacaktır.
```

**Yeni Proje(ler) Oluşturulmasına Yönelik Çıktılar**
```text
Bulgulara göre karşılaştırmanın ESP-NOW kontrollü yayma ve ESP-Mesh-Lite gibi ek protokollerle genişletilmesi; robot hızının ve kontrollü girişim düzeylerinin öngörüye dayalı bırakma kuralına etkisinin incelenmesi; sistemin arama-kurtarma eğitim sahaları gibi gerçek enkaz benzetim alanlarında denenmesi; Wi-Fi uzun menzil (LR) kipi, enerji verimliliği ve otonom navigasyonla birleşik bırakma kararı konularında bitirme projesi veya yeni bir araştırma projesi önerisinin çerçevesinin çıkarılması.
```

### 5. Belirtmek İstediğiniz Diğer Konular

Zorunlu değil. Bütçe ekranı yalnızca kategori toplamlarını aldığı için değerlendiricinin harcama dökümünü görebileceği tek yer burası.

```text
Bütçe gerekçesi (KDV dâhil, Kasım 2026 fiyatları; bazı kalemler tahminidir):
• Ağ düğümleri – 4.276,61 TL: 8 adet XIAO ESP32-C3 (merkez, 5 röle, robot ağ kartı, 1 yedek), 5 korumalı LiPo pil, anahtarlar ve kablo.
• Bırakıcı robot – 3.506,96 TL: 2 enkoderli mikro DC motor ve tekerler, sarhoş teker, sürüş kartı (ESP32-S3), motor sürücü, 3S Li-ion pil, BMS ve sigorta, 2 düşürücü regülatör, şarj cihazı, kablolu kumanda. Şasi 3D baskıyla üretilecektir.
• Bırakma mekanizması ve 3D baskı – 1.536,27 TL: step motor ve sürücü, rulman, PLA filament (şasi, tabla ve düğüm kutuları).
• Ölçüm altyapısı – 2.332,05 TL: deneye özel 2,4 GHz router ve lazer mesafe ölçer (demirbaş), 2 yedek LiPo pil.
• Sarf – 348,11 TL: zemin işaretleme bandı, konnektör, kondansatör, pil kabloları, yedek sigortalar.
Toplam: 12.000,00 TL (makine-teçhizat 1.858 TL, sarf malzemesi 10.142 TL).
```

### 6. Ekler

Boş bırakın.

---

## EKRAN 07 — Kaynaklar

Kaynaklar alfabetik sırada. Metindeki her atfın burada, buradaki her kaynağın da metinde karşılığı var. Metin içi atıflar (Yazar, yıl) biçiminde olduğu için **Number the Bibliography düğmesine basmayın.**

```text
Abu-Aisheh, R., Bronzino, F., Salaün, L. ve Watteyne, T. (2022). CARA: Connectivity-aware relay algorithm for multi-robot expeditions. Sensors, 22(23), 9042. DOI: 10.3390/s22239042

Almes, G., Kalidindi, S., Zekauskas, M. ve Morton, A. (2016). A one-way loss metric for IP performance metrics (IPPM) (RFC 7680, STD 82). Internet Engineering Task Force. DOI: 10.17487/RFC7680

Chung, T. H., Orekhov, V. ve Maio, A. (2023). Into the robotic depths: Analysis and insights from the DARPA Subterranean Challenge. Annual Review of Control, Robotics, and Autonomous Systems, 6, 477–502. DOI: 10.1146/annurev-control-062722-100728

Demichelis, C. ve Chimento, P. (2002). IP packet delay variation metric for IP performance metrics (IPPM) (RFC 3393). Internet Engineering Task Force. DOI: 10.17487/RFC3393

Espressif Systems. (t.y.-a). ESP-NOW – ESP32-C3, ESP-IDF Programming Guide. Erişim adresi: https://docs.espressif.com/projects/esp-idf/en/v5.5.6/esp32c3/api-reference/network/esp_now.html (Erişim tarihi: 6 Kasım 2026)

Espressif Systems. (t.y.-b). ESP-WIFI-MESH – ESP32-C3, ESP-IDF API Guide. Erişim adresi: https://docs.espressif.com/projects/esp-idf/en/release-v5.4/esp32c3/api-guides/esp-wifi-mesh.html (Erişim tarihi: 6 Kasım 2026)

Espressif Systems. (t.y.-c). ESP-Mesh-Lite User Guide. Erişim adresi: https://github.com/espressif/esp-mesh-lite/blob/master/components/mesh_lite/User_Guide.md (Erişim tarihi: 6 Kasım 2026)

Gao, Y., Chen, H., Li, Y., Lyu, C. ve Liu, Y. (2017). Autonomous Wi-Fi relay placement with mobile robots. IEEE/ASME Transactions on Mechatronics, 22(6), 2532–2542. DOI: 10.1109/TMECH.2017.2751149

Imaizumi, T., Murakami, H. ve Uchimura, Y. (2013). Deployment control of wireless multihop-relay mobile robots based on Voronoi partition. Electrical Engineering in Japan, 184(4), 42–51. DOI: 10.1002/eej.22413

Kim, D., Kim, S. ve Suh, J.-H. (2026). GP-driven adaptive tube MPC for communication-preserving navigation of mobile relay robots in indoor disaster environments. Sensors, 26(13), 3981. DOI: 10.3390/s26133981

Kim, K.-H., Shin, K. G. ve Niculescu, D. (2013). Mobile autonomous router system for dynamic (re)formation of wireless relay networks. IEEE Transactions on Mobile Computing, 12(9), 1828–1841. DOI: 10.1109/TMC.2012.160

Li, J., Blake, C., De Couto, D. S. J., Lee, H. I. ve Morris, R. (2001). Capacity of ad hoc wireless networks. Proceedings of the 7th Annual International Conference on Mobile Computing and Networking (MobiCom '01), 61–69. ACM. DOI: 10.1145/381677.381684

Pezeshkian, N., Neff, J. D. ve Hart, A. (2012). Link quality estimator for a mobile robot. Proceedings of the 9th International Conference on Informatics in Control, Automation and Robotics (ICINCO), 87–94. SciTePress. DOI: 10.5220/0004038400870094

Pezeshkian, N., Nguyen, H. G. ve Burmeister, A. (2007). Unmanned ground vehicle radio relay deployment system for non-line-of-sight operations. 13th IASTED International Conference on Robotics and Applications, Würzburg, Almanya. DOI: 10.21236/ADA475525

Seeed Studio. (t.y.). Getting started with Seeed Studio XIAO ESP32C3. Erişim adresi: https://wiki.seeedstudio.com/XIAO_ESP32C3_Getting_Started/ (Erişim tarihi: 6 Kasım 2026)

Tranzatto, M., Miki, T., Dharmadhikari, M., Bernreiter, L. vd. (2022a). CERBERUS in the DARPA Subterranean Challenge. Science Robotics, 7(66), eabp9742. DOI: 10.1126/scirobotics.abp9742

Tranzatto, M., Dharmadhikari, M., Bernreiter, L., Camurri, M. vd. (2022b). Team CERBERUS wins the DARPA Subterranean Challenge: Technical overview and lessons learned. arXiv ön baskısı, arXiv:2207.04914. DOI: 10.48550/arXiv.2207.04914
```

**Atıf kontrolü (17 kaynak, hepsi metinde):**

| Kaynak | Geçtiği yer |
|---|---|
| Abu-Aisheh vd., 2022 · Pezeshkian vd., 2012 | 1.1, Yöntem 2.4 |
| Almes vd., 2016 · Demichelis ve Chimento, 2002 | Yöntem 2.8 |
| Chung vd., 2023 · Gao vd., 2017 · Imaizumi vd., 2013 · Kim vd., 2013 · Kim vd., 2026 · Pezeshkian vd., 2007 · Tranzatto vd., 2022a | 1.1 |
| Tranzatto vd., 2022b | 1.1 (iki kez) |
| Espressif Systems, t.y.-a / -b | 1.1, Yöntem 2.3 |
| Espressif Systems, t.y.-c | 3.2 Risk |
| Li vd., 2001 | 1.1, Yöntem 2.9 |
| Seeed Studio, t.y. | 1.1, Yöntem 2.2 |

---

## EKRAN 08 — ÜYZ Taahhütnamesi

Kutuları yalnızca doğruysa işaretleyin. Bu başvurunun hazırlanmasında bir sohbet asistanı (Claude) önemli ölçüde kullanıldı. Rehber bu durumda şeffaf beyanı zorunlu tutuyor. Sistem bir açıklama kutusu sorarsa:

```text
Proje önerisinin hazırlanmasında üretken yapay zekâ aracı (Claude, Anthropic) deney tasarımı seçeneklerinin tartışılması, metin taslaklarının oluşturulması ve dil düzenlemesi amacıyla kullanılmıştır. Üretilen tüm metinler ekip tarafından okunmuş, düzenlenmiş ve onaylanmıştır. Kaynakçadaki tüm kaynakların varlığı ve künye bilgileri DOI ve yayıncı kayıtları üzerinden ekip tarafından tek tek doğrulanmıştır. Bilimsel içeriğin doğruluğu ve özgünlüğünün sorumluluğu proje ekibine ve danışmana aittir.
```

> Beyan doğru olsun diye kaynakların her birini **ekipten en az bir kişi kendisi açıp kontrol etsin**.

## EKRAN 09 — Başvuru Ek Belgeleri

Etik kurul belgesi gerekmiyor: insan katılımcı, anket ya da kişisel veri yok. Yine de danışmana bir kez sorun.

---

## Sınır kontrolü

| Alan | Sınır | Bu metin |
|---|---|---|
| Başlık | ≤ 495 karakter | 167 karakter ✓ |
| Özet | 25–450 kelime | 404 kelime ✓ |
| Anahtar kelime | 3–5, virgülle ayrılmış | 5 ✓ |
| 1.1 | ≥ 350 karakter | 6.854 karakter ✓ |
| 1.2 | ≥ 350 karakter | 2.112 karakter ✓ |
| 2. Yöntem | ≥ 350 karakter | 12.109 karakter ✓ |
| Kaynakça | ≤ 32.000 karakter | 3.620 karakter ✓ |
| İş paketi süreleri | ≤ 12 ay | 12 ay ✓ |
| Katkı toplamı | %100 | %100 ✓ |

Bir kutunun **üst** karakter sınırı çıkarsa (rehberde yalnızca alt sınır görünüyor) Yöntem'de önce 2.10'u, sonra 2.7'yi kısaltın.

## Göndermeden önce

1. **Aleyna:** TYBS'de *Institution Official Information* tablosunda kuruluş yetkilisinin tanımlı olduğunu hemen kontrol et. Tanımlı değilse danışman onay veremez.
2. **Hacer, Hatice, Mustafa:** ARBİS kayıtlarınızı güncelleyin. Aleyna sizi **Öğrenci Bilgileri → Ekle** ile eklesin; yalnızca arama yapmak yetmez.
3. **3.3'ü** gerçek bilgilerle doldurun (3D yazıcı, laboratuvar, binalar).
4. **NovaMax motorun kilitlenme akımını** ve pil yuvasının akım sınırını satıcıdan doğrulayın.
5. Faaliyet alanını ve öncelikli alanı danışmanla seçin.
6. **12. Kalkınma Planı'nda "afet" aratın.** İlgili bir hedef bulursanız 1.1'deki Türkiye paragrafına bir cümleyle ekleyin (bilgi notu bu ilişkiyi soruyor). Metni görmeden madde numarası yazmayın.
7. **İsteğe bağlı ön ölçüm:** İki kartla bir akşam koridorda ESP-NOW menzil ve RSSI ölçümü yapılırsa 2.10'daki "henüz ön çalışma sonucu bulunmamaktadır" cümlesi gerçek bir sonuçla değiştirilir ("Ön denemede düz koridorda X m'de RSSI −75 dBm ölçülmüştür" gibi). Yapılabilirlik puanına iyi gelir.
8. Kaydet → **Preview Research Proposal** → **Preview Application** ile kontrol edin. Sonra Aleyna son onayı versin. Danışmana en geç **10 Kasım**'da haber verin.
