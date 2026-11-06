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
| K-12 | 5 Kas 2026 | Üç protokol: ESP-WIFI-MESH, ham ESP-NOW zinciri, SBZ | P1 hazır mesh'i temsil eder. P3 ve P5 aynı kodun iki ayarı olduğu için SBZ'nin her özelliğinin katkısı ayrıştırılır. Üçü iki kod tabanıyla yapılır. | 5–6 protokol (emek fazla, her biri yüzeysel kalır) |
| K-13 | 6 Kas 2026 | **Kendi SoftAP+STA/UDP zincirimiz çıkarıldı** | ESP-Mesh-Lite aynı fikrin (SoftAP+STA, üst düğümden IP) Espressif tarafından bakımı yapılan hâli. Yeniden yazmak en riskli işti (IP, yönlendirme, tek radyoda kanal zinciri) ve "neden hazırını kullanmadınız?" sorusuna açıktı. Sıra bilme fikri ESP-NOW'a daha iyi oturuyor (SBZ). | Mesh-Lite'ı yalnızca Aşama 1'e eklemek (önerildi, ekip istemedi) |
| K-14 | 6 Kas 2026 | ESP-Mesh-Lite yalnızca B planı | P1 çalışmazsa yerine geçer. | Dördüncü protokol olarak eklemek |
| K-15 | 6 Kas 2026 | Röleler tamburda **bekleme modunda** | Aksi hâlde ESP-WIFI-MESH'te röleler robotun üstündeyken ağa katılır ve topolojiyi bozar. | Röleleri kapalı taşıyıp düşünce açmak (manyetik anahtar gerekir, karmaşık) |
| K-16 | 6 Kas 2026 | Router kullanılırsa **üç protokolde de açık** | Router'ın yayın paketleri kanalı meşgul eder; yalnızca bir protokolde açık olursa karşılaştırma adil olmaz. | Yalnızca P1'de açık |
| K-17 | 6 Kas 2026 | Paket: 16 B başlık + 160 B veri | ESP-NOW v1'in 250 B sınırının altında; tüm protokollerde aynı boyut. | Daha büyük çerçeve (ESP-NOW v2, uyumluluk riski) |

## Bırakma kuralları

| No | Tarih | Karar | Gerekçe | Değerlendirilen alternatifler |
|---|---|---|---|---|
| K-18 | 5 Kas 2026 | Sabit mesafe (S1) **kalıyor** | Uyarlamalı kuralların "daha iyi" olduğunu söyleyebilmek için bir kıyas noktası şart. Literatürde de temel yöntem bu. "Basit" olması zayıflık değil, kıyas için gereklilik. | S1'i çıkarıp yalnızca S2 ve S3 (karşılaştırma dayanaksız kalırdı) |
| K-19 | 6 Kas 2026 | S1'de ana karşılaştırmada **tek X** (O1'den) | Gerçek bir sahada ortam önceden bilinmez; tek X bunu temsil eder. Ortama göre X ayrıca kıyas olarak denenebilir. | Her ortama ayrı X (sabit mesafeye haksız avantaj) |
| K-20 | 6 Kas 2026 | S★ açgözlü yerleşim yürüyüşüyle hesaplanır | Duraklayarak ölçen biri için en az röle sayısını verir; kurallar buna göre "kaç fazla röle" ile değerlendirilir. | Yayılım modeliyle hesap (L koridorda güvenilmez) |

## Deney tasarımı

| No | Tarih | Karar | Gerekçe | Değerlendirilen alternatifler |
|---|---|---|---|---|
| K-21 | 5 Kas 2026 | Üç aşama: pilot → statik zincir → robotlu | Uyarlamalı kurallar her protokolde röleleri farklı yere bırakır; protokol ve kural etkisi karışır. Önce protokoller sabit konumda karşılaştırılır. | Tek aşamada her şey (karışıklık) |
| K-22 | 6 Kas 2026 | Her koşulda **5 tekrar** (~180 koşu) | 3 tekrar istatistik için az; koşuların çoğu kısa ve otomatik, ek emek sınırlı. | 3 tekrar |
| K-23 | 6 Kas 2026 | Ayarlar pilottan sonra **dondurulur** | Sonuca bakıp ayar değiştirmek sonuçları geçersiz kılar. Kötü sonuç bulgu sayılır. | Ana deneyde ayar ayarlamak |
| K-24 | 6 Kas 2026 | Ortak yük kuralla seçilir (en zayıf protokolün 6 atlamadaki kapasitesinin ~%70'i) | Bir protokolde rahat, diğerinde taşınamayan yük adil olmaz; kural önceden yazılınca seçim keyfî olmaz. | Sabit 128 kbit/s |
| K-25 | 6 Kas 2026 | 1B olayları röle yazılımına programlanır | Elle düğme kapatmak zamanlamayı bozar ve iki kişi gerektirir; programlı olunca tek kişi yeter. | Elle kapatma |
| K-26 | 6 Kas 2026 | Tek yönlü gecikme değil RTT | Kart saatleri eş zamanlı değil. | Saat eşleme (karmaşık) |
| K-27 | 6 Kas 2026 | Aşama 2'de yalnızca P1 ve P5 | P3, SBZ'nin özellikleri kapalı hâli; katkısı Aşama 1'de ayrışır. Koşu sayısı 135'ten 90'a iner. | Üç protokol (135 koşu) |

## Ekip

| No | Tarih | Karar | Gerekçe |
|---|---|---|---|
| K-28 | 6 Kas 2026 | İş bölümü [ekip.md](ekip.md)'deki gibi; başvurudakiyle aynı | Tek iş bölümü, karışıklık yok; sonuç raporu başvuruyla tutarlı olur. |

## Yeni karar eklerken

```markdown
| K-xx | GG Ay YYYY | Karar | Gerekçe | Alternatifler |
```
