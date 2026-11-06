# Kaynakça ve Literatür Notları

**Son güncelleme:** 6 Kasım 2026  
**Doğrulama:** A1–A11 ve C1'in DOI'leri 4 Kasım 2026'da Crossref'te kontrol edildi. A1'in yazarları ve A12'deki alıntılar 6 Kasım 2026'da kaynağından doğrulandı.

> **Kaynak türleri:** A = hakemli akademik yayın · B = üretici teknik belgesi · C = ölçüm standardı. Satıcı sayfaları akademik kaynak değildir; onlar [maliyet.md](maliyet.md)'de.

## A. Akademik literatür

### A1. Bağlantı kopmadan röle bırakma kararı ★ en ilgili

Pezeshkian, N., Neff, J. D., & Hart, A. (2012). *Link quality estimator for a mobile robot*. Proceedings of the 9th International Conference on Informatics in Control, Automation and Robotics (ICINCO), 87–94. SciTePress. https://doi.org/10.5220/0004038400870094

- **Ne yapıyor?** Görüş hattı olmayan ortamlarda, bağlantı kopmadan önce röle bırakmak için RSSI ve video veri hızından bağlantı kalitesi tahmini.
- **Bizim için:** S2 (eşik) ve S3 (eğilim) kurallarının dayanağı; RSSI'nin tek başına yetmeyebileceği tartışması.
- [Yayın sayfası ve açık PDF](https://www.scitepress.org/Papers/2012/40384/)

### A2. Wi-Fi rölelerini robotlarla yerleştirme

Gao, Y., Chen, H., Li, Y., Lyu, C., & Liu, Y. (2017). *Autonomous Wi-Fi relay placement with mobile robots*. IEEE/ASME Transactions on Mechatronics, 22(6), 2532–2542. https://doi.org/10.1109/TMECH.2017.2751149

- **Ne yapıyor?** Wi-Fi sinyal dağılımını modelleyip hareketli röle robotlarını uygun konumlara yerleştiriyor.
- **Bizim için:** Kapalı ortamda Wi-Fi röle yerleştirmenin arka planı; sabit mesafenin her ortamda en iyi olmayabileceği. SLAM ve hareket planlaması bizim kapsamımız dışında.

### A3. Sabit mesafeye karşı bağlantı farkındalıklı yerleştirme

Abu-Aisheh, R., Bronzino, F., Salaün, L., & Watteyne, T. (2022). *CARA: Connectivity-aware relay algorithm for multi-robot expeditions*. Sensors, 22(23), 9042. https://doi.org/10.3390/s22239042

- **Ne yapıyor?** Röleyi ne zaman ve nereye koyacağına bağlantı ölçümüyle karar veren algoritmayı mesafe temelli yaklaşımla karşılaştırıyor.
- **Bizim için:** S1'in (sabit mesafe) kıyas yöntemi olarak seçilmesinin dayanağı.
- [Açık erişim](https://pmc.ncbi.nlm.nih.gov/articles/PMC9737801/)

### A4. Doğrusal çok atlamalı röle ağı

Kim, K.-H., Shin, K. G., & Niculescu, D. (2013). *Mobile autonomous router system for dynamic (re)formation of wireless relay networks*. IEEE Transactions on Mobile Computing, 12(9), 1828–1841. https://doi.org/10.1109/TMC.2012.160

- **Ne yapıyor?** Bağlantı kalitesi ölçümleriyle röleleri konumlandırarak doğrusal çok atlamalı ağ kuruyor (MARS).
- **Bizim için:** Robot–röleler–merkez zincirinin arka planı.
- [Yazar nüshası (PDF)](https://rtcl.eecs.umich.edu/rtclweb/assets/publications/2013/kkim_tmc2013.pdf)

### A5. RSSI ile çok atlamalı röle konumlandırma

Imaizumi, T., Murakami, H., & Uchimura, Y. (2013). *Deployment control of wireless multihop-relay mobile robots based on Voronoi partition*. Electrical Engineering in Japan, 184(4), 42–51. https://doi.org/10.1002/eej.22413

- **Ne yapıyor?** Sinyali zayıflatan binalarda bağlantıyı koruyan çok atlamalı röle robotu yerleşimi.
- **Bizim için:** Engelli kapalı ortamlarda çok atlamanın gerekliliği. Bizde röleler hareket etmez, tek robottan bırakılır.

### A6. Güncel: RSSI'yi öngörerek bağlantıyı koruma

Kim, D., Kim, S., & Suh, J.-H. (2026). *GP-driven adaptive tube MPC for communication-preserving navigation of mobile relay robots in indoor disaster environments*. Sensors, 26(13), 3981. https://doi.org/10.3390/s26133981

- **Ne yapıyor?** İç mekânda RSSI'yi Gauss süreç regresyonuyla tahmin edip model öngörülü kontrolle röle robotunu konumlandırıyor.
- **Bizim için:** S3'ün (eğilim) ileri yöntemlerle ilişkisi. Bizim hafif EWMA/eğim yöntemimiz bunun aynısı değil; öyle sunulmamalı.
- [Açık erişim](https://www.mdpi.com/1424-8220/26/13/3981)

### A7. Robottan röle bırakma sistemi — fikrin öncülü

Pezeshkian, N., Nguyen, H. G., & Burmeister, A. (2007). *Unmanned ground vehicle radio relay deployment system for non-line-of-sight operations*. 13th IASTED International Conference on Robotics and Applications, Würzburg. https://doi.org/10.21236/ADA475525

- **Ne yapıyor?** İnsansız kara aracının arkasına otomatik olarak radyo rölesi bırakan sistem.
- **Bizim için:** "Robot arkasına röle bırakır" fikrinin öncülü. Farkımız: ucuz donanım ve kuralların kontrollü karşılaştırması.
- DOI, DTIC rapor kaydına ait; bildiri bilgileri ikincil kaynaklardan.

### A8. DARPA SubT'yi kazanan ekip

Tranzatto, M., Miki, T., Dharmadhikari, M., Bernreiter, L., et al. (2022). *CERBERUS in the DARPA Subterranean Challenge*. Science Robotics, 7(66), eabp9742. https://doi.org/10.1126/scirobotics.abp9742

- **Bizim için:** Breadcrumbing'in yarışmayı kazanan sistemde kullanıldığının kaynağı. "İnsan karar verdi" bulgusu için **A12'ye** atıf yapılır.

### A9. CERBERUS: tünel ve kentsel etaplar

Tranzatto, M., Mascarich, F., Bernreiter, L., Godinho, C., et al. (2022). *CERBERUS: Autonomous legged and aerial robotic exploration in the tunnel and urban circuits of the DARPA Subterranean Challenge*. Field Robotics, 2, 274–324. https://doi.org/10.55417/fr.2022011

- **Bizim için:** Önceki etaplarda düğüm donanımı ve bırakma uygulaması.

### A10. DARPA SubT genel değerlendirmesi

Chung, T. H., Orekhov, V., & Maio, A. (2023). *Into the robotic depths: Analysis and insights from the DARPA Subterranean Challenge*. Annual Review of Control, Robotics, and Autonomous Systems, 6, 477–502. https://doi.org/10.1146/annurev-control-062722-100728

- **Bizim için:** Haberleşmenin yeraltı keşfinde temel zorluk olduğunun düzenleyici gözünden kaynağı.

### A11. Çok atlamada kapasite düşüşü

Li, J., Blake, C., De Couto, D. S. J., Lee, H. I., & Morris, R. (2001). *Capacity of ad hoc wireless networks*. MobiCom '01, 61–69. ACM. https://doi.org/10.1145/381677.381684

- **Ne yapıyor?** Aynı kanalı paylaşan 802.11 düğümleri birbiriyle yarıştığı için zincirde atlama arttıkça verimin düştüğünü gösteriyor.
- **Bizim için:** H2'nin dayanağı.

### A12. CERBERUS teknik raporu — bırakma kararını insan verdi ★

Tranzatto, M., Dharmadhikari, M., Bernreiter, L., Camurri, M., et al. (2022). *Team CERBERUS wins the DARPA Subterranean Challenge: Technical overview and lessons learned*. arXiv:2207.04914. https://doi.org/10.48550/arXiv.2207.04914

Tam metinden doğrulanan bilgiler (6 Kasım 2026):

- **Bırakma kararı (Deployable breadcrumb nodes bölümü):** *"We did not automate the deployment of these beacons. During the Final Event, the Human Supervisor made the expert decision about where and when to deploy the breadcrumb nodes as wireless signal propagation is often unpredictable in subterranean scenarios."*
- **Başka ekip (ilgili çalışmalar):** Team Explorer'ın kara robotları düğümleri *"according to a cost function using distance, line-of-sight and RSSI conditions"* bırakmış.
- **Düğüm:** Ticari Rajant BreadCrumb DX2 radyosu; 245 g; 120 × 60 × 50 mm; elektromıknatısla bırakma; robot başına 4 düğüm.

> **Dikkat:** Otomatik bırakma kuralları literatürde **var** (A1, A7, Team Explorer). Doğru iddia "otomatik bırakmayı ilk biz yapıyoruz" değil, **"bırakma kuralları ucuz donanımda kontrollü ve tekrarlı deneylerle karşılaştırılmamış"** olmalıdır.

## B. Üretici teknik belgeleri

| No | Belge | Kullanım |
|---|---|---|
| B1 | [Seeed Studio — XIAO ESP32-C3 başlangıç rehberi](https://wiki.seeedstudio.com/XIAO_ESP32C3_Getting_Started/) | Kart, anten, pil bağlantısı, pinler |
| B2 | [Espressif — Wi-Fi sürücüsü (ESP32-C3)](https://docs.espressif.com/projects/esp-idf/en/v5.5.6/esp32c3/api-guides/wifi.html) | Kanal, verici gücü, Wi-Fi aç/kapa (1B arıza), RSSI. Not: AP+STA iki ayrı radyo demek değildir. |
| B3 | [Espressif — ESP-NOW (ESP32-C3)](https://docs.espressif.com/projects/esp-idf/en/v5.5.6/esp32c3/api-reference/network/esp_now.html) | Eş tanımlama, gönderme/alma callback'leri, 250 B sınırı. ESP-NOW tek başına çok atlama yapmaz. |
| B4 | [Espressif — ESP-WIFI-MESH API rehberi](https://docs.espressif.com/projects/esp-idf/en/release-v5.4/esp32c3/api-guides/esp-wifi-mesh.html) | Kök, ebeveyn, katmanlar, ağ onarımı |
| B5 | [Espressif — ESP-WIFI-MESH programlama](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/api-reference/network/esp-wifi-mesh.html) | `esp_mesh_*` arayüzleri, sabit kök. Mesh açıkken Wi-Fi'ı ayrıca yönetmek çakışma yaratır. |
| B6 | [Espressif — ESP-FAQ: Wi-Fi Mesh](https://github.com/espressif/esp-faq/blob/master/docs/en/application-solution/wifi-mesh-development-framework.rst) | Router'lı ve router'sız senaryolar. Sürüm davranışını kanıtlamaz; ön test şart. |
| B7 | [Espressif — ESP-Mesh-Lite kılavuzu](https://github.com/espressif/esp-mesh-lite/blob/master/components/mesh_lite/User_Guide.md) | B planı. SoftAP+STA tabanlı; C3 desteği ESP-IDF v5.0+. |

## C. Ölçüm standartları

| No | Kaynak | Kullanım |
|---|---|---|
| C1 | Almes, G., Kalidindi, S., Zekauskas, M., & Morton, A. (2016). *A one-way loss metric for IP performance metrics (IPPM)*. RFC 7680. https://doi.org/10.17487/RFC7680 | Paket kaybı tanımı. ESP-NOW için `seq` üzerinden **uyarlanır**; RFC'ye tam uyum iddia edilmez. |
| C2 | Demichelis, C., & Chimento, P. (2002). *IP packet delay variation metric for IPPM*. RFC 3393. https://doi.org/10.17487/RFC3393 | Jitter tanımı. Saatler eş zamanlı olmadığı için tek yönlü gecikme yerine RTT. |

## D. Hangi konuda hangi kaynak?

| Konu | Kaynaklar |
|---|---|
| Problem ve motivasyon | A10, A8, A12 |
| Literatürdeki boşluk | A7, A1, A12 |
| Ucuz donanımın gerekçesi | A12 (ticari düğüm: 245 g, robot başına 4), B1 |
| Atlama sayısı ve verim | A11 |
| Sabit mesafe | A3 |
| RSSI eşiği | A1, A5 |
| Eğilim / öngörü | A1, A6 |
| Doğrusal röle zinciri | A2, A4, A5 |
| ESP-WIFI-MESH | B4, B5, B6 |
| ESP-NOW ve SBZ | B3, B2 |
| B planı | B7 |
| Metrikler | C1, C2 |

## Yazarken dikkat

- Bu kaynaklar üç protokol ile üç kuralın **aynı deneyde** daha önce karşılaştırıldığını göstermez; biz de öyle iddia etmeyiz.
- ESP-WIFI-MESH, ham ESP-NOW zinciri ve SBZ aynı radyonun farklı ağ yöntemleridir; farklı radyo teknolojileri gibi sunulmaz.
- S3'ün EWMA/eğim yöntemi bizim tasarımımızdır; literatürdeki GPR/MPC yöntemleriyle eşitlenmez.
- Kaynakçadaki her yayına metinde gerçekten atıf yapılır; sayı artırmak için kaynak eklenmez.
