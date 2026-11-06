# Karar Kaydı

**Son güncelleme:** 6 Kasım 2026

Her önemli karar buraya bir satır olarak eklenir. Amaç, altı ay sonra "bunu neden böyle yapmıştık?" sorusuna cevap bulabilmek ve bir kararı değiştirmeden önce gerekçesini görmek. **Eski kararlar silinmez**; değişen bir kararın yanına "yerini aldı: K-xx" yazılır.

## Kapsam

| No | Tarih | Karar | Gerekçe | Değerlendirilen alternatifler |
|---|---|---|---|---|
| K-01 | 4 Kas 2026 | Yalnızca 2,4 GHz Wi-Fi kullanılacak | Karşılaştırılan şey radyo değil, aynı radyo üzerindeki ağ yöntemleri. Tek radyo = adil karşılaştırma ve düşük maliyet. | Zigbee, BLE mesh, LoRa (ayrı bir proje konusu) |
| K-02 | 4 Kas 2026 | Tüm ağ uçlarında XIAO ESP32-C3 | Ucuz, küçük, harici antenli, pil şarj devresi kartta. Robot ucu da aynı kart olunca donanım farkı karşılaştırmayı bozmaz. | ESP32 DevKit (büyük, harici anten yok), ESP32-S3 (pahalı) |
| K-03 | 4 Kas 2026 | Gerçek kamera yok, sentetik trafik var | Hızı ayarlanabilir, tekrarlanabilir yük; video kodlama karmaşıklığı yok. | Gerçek kamera akışı (gelecek çalışma) |
| K-04 | 5 Kas 2026 | Enerji ölçümü kapsam dışı | Projenin ana sorusu "ucuz donanımla breadcrumbing ne kadar etkili?". Enerji ayrı ölçüm donanımı ve emek ister. | INA219 ile röle başına akım ölçümü (çıkarıldı) |
| K-05 | 4 Kas 2026 | En fazla 5 röle | Bütçe, tabla boyutu ve robotun taşıma kapasitesi. 6 atlama, kapasite düşüşünü görmeye yeter. | 3 röle (az), 8 röle (pahalı, tabla büyük) |

## Robot ve mekanizma

| No | Tarih | Karar | Gerekçe | Değerlendirilen alternatifler |
|---|---|---|---|---|
| K-06 | 4 Kas 2026 | Robot operatörle, **kablolu** kumandayla sürülür | Otonom navigasyon projenin sorusu değil. 2,4 GHz kumanda ölçülen kanala girişim yapar. | RC kumanda (girişim), otonom sürüş (kapsam dışı) |
| K-07 | 4 Kas 2026 | Robotta iki kart: radyolu ağ kartı + radyosuz sürüş kartı (Nano) — *yerini aldı: K-29* | Robot ucundaki radyo rölelerle aynı kalır; tek çekirdekli C3'te motor kontrolü ağ zamanlamasını bozmaz. | Tek ESP32 (zamanlama riski, radyo farkı) |
| K-08 | 5 Kas 2026 | Bırakma mekanizması: NEMA 17 ile dönen **6 bölmeli revolver tabla** | Tek motor, tek hareketli parça, kapak yok. Düğüm düz düşer, anten yönü korunur. 5 röle + 1 boş bölme. | Her düğüme bir servo (5 motor, karmaşık), dikey şarjör (sıkışma riski, düğüm dönebilir) |
| K-09 | 5 Kas 2026 | Step sürücü TMC2209 | İlk gerekçe 2S pilde A4988/DRV8825'in çalışmamasıydı. 3S'e geçilince (K-30) bu gerekçe kalktı; TMC2209 sessizliği ve bekleme akımı azaltma özelliği için korundu. | A4988, DRV8825 |
| K-10 | 5 Kas 2026 | 60° hedefleri mutlak adımla hesaplanır | 60° = 33,33 adım; göreli adımla yuvarlama hatası birikir. | Her bırakmada 33/34 adım dönüşümlü (hata birikir) |
| K-11 | 5 Kas 2026 | Mekanizma çalışmazsa elle bırakma (B planı) | Karar kuralı ve kayıt aynı kaldığı için araştırma sorusu etkilenmez. | — |
| K-29 | 6 Kas 2026 | Robot **2WD diferansiyel**: 2 enkoderli mikro DC motor (NovaMax 250 rpm) + sarhoş teker, 3D baskı şasi; sürüş kartı **ESP32-S3** (Wi-Fi/BT hiç açılmaz). Robot ağ kartı yine XIAO ESP32-C3. *K-07'nin yerini aldı.* | Motor enkoderleri ayrı IR sensörden daha hassas mesafe verir; 3D baskı şasi tablayla birlikte tasarlanabilir; daha hafif ve basit. S3 çift çekirdekli ve 3,3 V'luk; XIAO ile UART'ta seviye dönüştürücü gerekmez. Radyonun rölelerle aynı kalması için ağ işi XIAO'da kalır. | 4WD TT motor + Nano + IR enkoder (K-07); S3'ü ağ kartı olarak da kullanmak (robot radyosu rölelerden farklı olurdu) |
| K-30 | 6 Kas 2026 | Robot pili **3S** (3 × 18650, 10,8–12,6 V); motorlar XL4015 ile 6 V, kartlar LM2596 ile 5 V | Motor hattı pil seviyesinden bağımsız sabit 6 V olur, PWM sınırlamasına gerek kalmaz; step motor 12 V'ta daha rahat çalışır. | 2S doğrudan besleme + PWM sınırı |
| K-31 | 6 Kas 2026 | Tabla için **ev konumu sensörü yok**; her koşudan önce hizalama işaretiyle elle ayarlanır | Daha az parça ve pin; ESP32-S3'ün 3,3 V pinleriyle Hall modülünün 5 V çıkışı arasındaki uyum sorunu ortadan kalkar. 5 bırakmalık kısa koşularda elle hizalama yeterli; bekleme akımı kesilmediği sürece tabla kaymaz. | Hall sensörü + mıknatıs, mikro limit anahtarı |

## Ağ protokolleri

| No | Tarih | Karar | Gerekçe | Değerlendirilen alternatifler |
|---|---|---|---|---|
| K-12 | 5 Kas 2026 | Üç protokol: ESP-WIFI-MESH, ham ESP-NOW zinciri, SBZ — *yerini aldı: K-42* | P1 hazır mesh'i temsil eder. P3 ve P5 aynı kodun iki ayarı olduğu için SBZ'nin her özelliğinin katkısı ayrıştırılır. Üçü iki kod tabanıyla yapılır. | 5–6 protokol (emek fazla, her biri yüzeysel kalır) |
| K-13 | 6 Kas 2026 | **Kendi SoftAP+STA/UDP zincirimiz çıkarıldı** | ESP-Mesh-Lite aynı fikrin (SoftAP+STA, üst düğümden IP) Espressif tarafından bakımı yapılan hâli. Yeniden yazmak en riskli işti (IP, yönlendirme, tek radyoda kanal zinciri) ve "neden hazırını kullanmadınız?" sorusuna açıktı. Sıra bilme fikri ESP-NOW'a daha iyi oturuyor (SBZ). | Mesh-Lite'ı yalnızca Aşama 1'e eklemek (önerildi, ekip istemedi) |
| K-14 | 6 Kas 2026 | ESP-Mesh-Lite yalnızca B planı | P1 çalışmazsa yerine geçer. | Dördüncü protokol olarak eklemek |
| K-15 | 6 Kas 2026 | Röleler tamburda **bekleme modunda** | Aksi hâlde ESP-WIFI-MESH'te röleler robotun üstündeyken ağa katılır ve topolojiyi bozar. | Röleleri kapalı taşıyıp düşünce açmak (manyetik anahtar gerekir, karmaşık) |
| K-16 | 6 Kas 2026 | Router kullanılırsa **üç protokolde de açık** *(K-42 sonrası: iki protokolde de açık)* | Router'ın yayın paketleri kanalı meşgul eder; yalnızca bir protokolde açık olursa karşılaştırma adil olmaz. | Yalnızca P1'de açık |
| K-17 | 6 Kas 2026 | Paket: 16 B başlık + 160 B veri | ESP-NOW v1'in 250 B sınırının altında; tüm protokollerde aynı boyut. | Daha büyük çerçeve (ESP-NOW v2, uyumluluk riski) |

## Bırakma kuralları

| No | Tarih | Karar | Gerekçe | Değerlendirilen alternatifler |
|---|---|---|---|---|
| K-18 | 5 Kas 2026 | Sabit mesafe (S1) **kalıyor** | Uyarlamalı kuralların "daha iyi" olduğunu söyleyebilmek için bir kıyas noktası şart. Literatürde de temel yöntem bu. "Basit" olması zayıflık değil, kıyas için gereklilik. | S1'i çıkarıp yalnızca S2 ve S3 (karşılaştırma dayanaksız kalırdı) |
| K-19 | 6 Kas 2026 | S1'de ana karşılaştırmada **tek X** (O1'den) | Gerçek bir sahada ortam önceden bilinmez; tek X bunu temsil eder. Ortama göre X ayrıca kıyas olarak denenebilir. | Her ortama ayrı X (sabit mesafeye haksız avantaj) |
| K-20 | 6 Kas 2026 | S★ açgözlü yerleşim yürüyüşüyle hesaplanır — *yerini aldı: K-42* | Duraklayarak ölçen biri için en az röle sayısını verir; kurallar buna göre "kaç fazla röle" ile değerlendirilir. | Yayılım modeliyle hesap (L koridorda güvenilmez) |

## Deney tasarımı

| No | Tarih | Karar | Gerekçe | Değerlendirilen alternatifler |
|---|---|---|---|---|
| K-21 | 5 Kas 2026 | Üç aşama: pilot → statik zincir → robotlu | Uyarlamalı kurallar her protokolde röleleri farklı yere bırakır; protokol ve kural etkisi karışır. Önce protokoller sabit konumda karşılaştırılır. | Tek aşamada her şey (karışıklık) |
| K-22 | 6 Kas 2026 | Her koşulda **5 tekrar** (~180 koşu) — *yerini aldı: K-42* | 3 tekrar istatistik için az; koşuların çoğu kısa ve otomatik, ek emek sınırlı. | 3 tekrar |
| K-23 | 6 Kas 2026 | Ayarlar pilottan sonra **dondurulur** | Sonuca bakıp ayar değiştirmek sonuçları geçersiz kılar. Kötü sonuç bulgu sayılır. | Ana deneyde ayar ayarlamak |
| K-24 | 6 Kas 2026 | Ortak yük kuralla seçilir (en zayıf protokolün 6 atlamadaki kapasitesinin ~%70'i) | Bir protokolde rahat, diğerinde taşınamayan yük adil olmaz; kural önceden yazılınca seçim keyfî olmaz. | Sabit 128 kbit/s |
| K-25 | 6 Kas 2026 | 1B olayları röle yazılımına programlanır | Elle düğme kapatmak zamanlamayı bozar ve iki kişi gerektirir; programlı olunca tek kişi yeter. | Elle kapatma |
| K-26 | 6 Kas 2026 | Tek yönlü gecikme değil RTT | Kart saatleri eş zamanlı değil. | Saat eşleme (karmaşık) |
| K-27 | 6 Kas 2026 | Aşama 2'de yalnızca P1 ve P5 — *yerini aldı: K-42* | P3, SBZ'nin özellikleri kapalı hâli; katkısı Aşama 1'de ayrışır. Koşu sayısı 135'ten 90'a iner. | Üç protokol (135 koşu) |

## Ekip

| No | Tarih | Karar | Gerekçe |
|---|---|---|---|
| K-28 | 6 Kas 2026 | İş bölümü [ekip.md](ekip.md)'deki gibi; başvurudakiyle aynı | Tek iş bölümü, karışıklık yok; sonuç raporu başvuruyla tutarlı olur. |

## Başvuru metni düzeltmeleri

| No | Tarih | Karar | Gerekçe | Değerlendirilen alternatifler |
|---|---|---|---|---|
| K-32 | 6 Kas 2026 | PHY veri hızı üç protokolde aynı değere sabitlenir ve her koşuda kaydedilir — *yerini aldı: K-43* | ESP-NOW varsayılanı 1 Mbit/s, mesh 802.11n hızları kullanır; sabitlenmezse menzil ve kapasite farkı protokolden değil hızdan gelir. | Varsayılan hızlarda bırakmak (karıştırıcı değişken) |
| K-33 | 6 Kas 2026 | Kapasite taraması ~1,4 kat adımlarla artar; üst sınır pilotta, tek atlamada da PDR düşecek şekilde seçilir. 1A'da mesh üst düğümleri sabitlenir. Görüntü kıyası 360p/480p ile sınırlı — *yerini aldı: K-42* | 32→1024 ikiye katlama kaba ve tavana takılıyordu; 720p bu aralığa girmiyordu. Sabit üst düğüm olmadan mesh'te atlama sayısı denetlenemez. | İkiye katlama (6 seviye), sabit 1024 kbit/s tavan |
| K-34 | 6 Kas 2026 | S★ (ideal yerleşim) çıkarıldı; yerine "aynı ortamda hedefe bağlantıyla ulaşan koşularda gözlenen en az röle sayısı" referansı — *yerini aldı: K-42* | Tek referans noktalı sinyal haritası, rota üzerindeki iki nokta arasındaki bağlantıyı vermez; S★ bu veriyle hesaplanamaz. | Çok referanslı sinyal haritası (ek iş) |
| K-35 | 6 Kas 2026 | TYBS'de protokoller P1/P2/P3 olarak yeniden adlandırıldı (P3 → P2, P5 → P3). Ham ESP-NOW zinciri de üst düğümü (k−1) bırakmada öğrenir; SBZ'nin farkı k−2 bilgisi, yeniden gönderim ve düğüm atlama — *yerini aldı: K-42* | Eski tanımda keşifsiz bağlanma hem P3'te hem P5'in eklediği özellik olarak görünüyordu. Etiketlerdeki boşluklar eski sürümden kalmaydı. | Eski numaralar |
| K-36 | 6 Kas 2026 | AS1–AS2 birincil, AS3–AS4 ikincil. Hipotezler AS sırasına göre H1–H5; H4 (etkileşim: eğilim kuralının üstünlüğü mesh'te daha büyük) eklendi; H1'e p95 RTT ödünleşimi eklendi; H5 "6 atlamada tek atlamanın yarısının altı" — *yerini aldı: K-42* | AS3'ün hipotezi yoktu; eski H1 ve H2 tasarım ya da literatür gereği zaten doğru çıkacaktı. Önceliklendirme zaman daralırsa ikincilleri bırakmayı meşru kılar. | Eski H1–H4 |
| K-37 | 6 Kas 2026 | Analiz: genelleştirilmiş karma model (süre log, sayım Poisson, başarı binom), her hipoteze bir birincil ölçüt, Holm düzeltmesi, her gün bir tam blok | 5 tekrarla normallik sınamak anlamsızdı; sayım ve evet/hayır ölçütleri doğrusal modele uymaz; gün etkisi koşullarla karışabiliyordu. | Parametrik olmayan testler |
| K-38 | 6 Kas 2026 | 1B'de röle aralığı k−2 bağlantısına izin verecek şekilde seçilir — *yerini aldı: K-42* | Aksi hâlde SBZ'nin düğüm atlama özelliği sınanamaz. | Yalnızca komşu sinyal seviyesine göre aralık |
| K-39 | 6 Kas 2026 | Mekanizma kabul ölçütü: 100 bırakmanın en az 95'i başarılı | "Hiç takılmadan" ölçütünü tutturmak neredeyse imkânsızdı. | 100/100 |
| K-40 | 6 Kas 2026 | Öncelikli alan: Evet; birincil Afet yönetimi teknolojileri, ikincil Robotik-mekatronik. 1.1'e Türkiye bağlamı paragrafı eklendi | Problemin kendisi afet alanına ait. "Yeni nesil haberleşme" 5G/6G çağrışımı yapar, 2,4 GHz Wi-Fi için zorlama görünür. Bilgi notu ilişkinin 1.1'de açıklanmasını istiyor. | Hayır; Yeni nesil haberleşme (40) |
| K-41 | 6 Kas 2026 | Sade tasarım ([sadelestirme-onerisi.md](sadelestirme-onerisi.md)) **uygulanmadı**; başvuru mevcut tasarımla yapılıyor — *yerini aldı: K-42* | Son güne 5 gün kala yeniden yapılandırma yeni tutarsızlık riski taşıyor; mevcut tasarım aşamalı ve kırılgan değil. | 2 protokol × 3 kural × 2 ortam × 8 tekrar |
| K-42 | 6 Kas 2026 | **Sade tasarım uygulandı** ([sadelestirme-onerisi.md](sadelestirme-onerisi.md)): P1 ESP-WIFI-MESH ve P2 SBZ (keşifsiz bağlanma + sınırlı yeniden gönderim; k−2 ve düğüm atlama yok); 3 kural; düz koridor yalnızca kalibrasyon (K), bütün kural ayarları orada yapılır; test ortamları O1 L koridor ve O2 bodrum; statik 2 × 2 × 5 = 20, robotlu 2 × 3 × 2 × 8 = 96, toplam ~120 koşu; ana ölçüt bırakma anı kesintisi (−5 s / +10 s); H1 katılma süresi, H2 kural (ortam bilinmeden), H3 etkileşim. Ham ESP-NOW, kapasite eğrisi, arıza testi, S★ ve en iyi gözlenen yerleşim çıktı. İP4 1 ay, İP5 4 ay. *K-27, K-33–K-36, K-38 ve K-41'in yerini aldı.* | Asıl soru (hangi protokol, hangi kural) tek bir hikâyeye bağlanıyor; etkileşim hipotezi sınanabilir hâle geliyor ve 8 tekrar bu testi güçlendiriyor; önceki tutarsızlıkların çoğu (P2/P3 ayrıştırması, k−2 gerilimi, kapasite tavanı, S★) ortadan kalkıyor; İP2'nin yükü azalıyor. | v2.1 tasarımı + düzeltmeler (K-32–K-40); ham ESP-NOW'u yalnızca statik aşamada tutmak |
| K-43 | 6 Kas 2026 | PHY hızı iki protokolde aynı değere sabitlenir: ESP-NOW'da `esp_now_set_peer_rate_config`, mesh'te `esp_wifi_config_80211_tx_rate` İP1'de denenir; mesh trafiğine uygulanamazsa iki protokolün radyosu aynı kipe (ör. yalnızca 802.11b) sınırlanır ve kalan fark sınırlılık olarak raporlanır. SBZ'nin yeniden gönderimi ESP-NOW gönderim callback'i başarısız döndüğünde yapılır (ayrı uygulama onayı yok). *K-32'nin yerini aldı.* | Espressif belgesine göre ESP-NOW varsayılanı 1 Mbps ve eş başına ayarlanabiliyor; mesh için doğrudan bir yöntem belgede yok. Callback yalnızca MAC teslimini bildiriyor. | Hızı sabitlemeden bırakmak; atlama başına uygulama onay paketi (ek trafik) |

## Yeni karar eklerken

```markdown
| K-xx | GG Ay YYYY | Karar | Gerekçe | Alternatifler |
```
