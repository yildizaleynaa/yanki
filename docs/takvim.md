# Takvim ve Riskler

**Son güncelleme:** 6 Kasım 2026 (sade tasarım, [kararlar.md](kararlar.md) K-42)

Proje süresi **12 ay**. Başlangıç tarihi destek sonucuna bağlı olduğu için aylar "Ay 1, Ay 2…" diye numaralandı. Kim ne yapıyor: [ekip.md](ekip.md).

## 1. İş paketleri

```text
Ay        1    2    3    4    5    6    7    8    9   10   11   12
İP1     ████████                                                     Ön test + ölçüm altyapısı
İP2               ████████                                           Protokoller + mekanizma
İP3                         ████                                     Kurallar + kalibrasyon
İP4                              ████                                Statik deneyler (20)
İP5                                   ████████████████               Robotlu deneyler (96)
İP6                                                      ████████    Analiz + yayın
          ▲         ▲         ▲    ▲    ▲              ▲         ▲
         KT1       KT2       KT3  KT4  KT5            KT6       KT7
```

| İP | Ay | Katkı | İş | Bitti sayılması için |
|---|---|---:|---|---|
| **İP1** | 1–2 | %10 | ESP-NOW ve ESP-WIFI-MESH ön testleri, **PHY hızının iki yöntemde sabitlenmesi**, deney ortamları için erişim izni, ortak paket biçimi, merkez kaydı ve betikler, robotun temel sürüşü | İki kartla iki yöntemde veri aktarılıyor; router gereksinimi ve ortak PHY hızı belli; CSV kaydı çalışıyor; robot kumandayla sürülüyor ve enkoder mesafesi kaydediliyor |
| **İP2** | 3–4 | %20 | SBZ ve ESP-WIFI-MESH uygulaması, bekleme modu; bırakma mekanizması ve düğüm kutularının tasarımı, baskısı ve montajı | İki protokol 5 rölelik zincirde veri iletiyor; mekanizma 100 bırakmanın en az 95'inde takılmıyor |
| **İP3** | 5 | %15 | Bırakma kuralları; K'de kalibrasyon yürüyüşü ve kapasite taraması; O1 ve O2'de rota kontrolü | Eğriler çıkarıldı; [deney_ayarlari.md](deney_ayarlari.md) donduruldu; üç kural tekrarlanabilir karar veriyor |
| **İP4** | 6 | %10 | Aşama 1: 20 statik koşu; analiz betiklerinin bu veriyle sınanması | 20 koşu tamamlandı, ham kayıtlar kontrol edildi, analiz betikleri çalışıyor |
| **İP5** | 7–10 | %35 | Aşama 2: 96 robotlu koşu (16 tam blok) | 96 koşu tamamlandı |
| **İP6** | 11–12 | %10 | İstatistiksel analiz, açık veri ve kod | H1–H3 test edildi; veri ve kod herkese açık |

**Bağımsızlık:** Robot ve mekanizma (İP2) ile statik deneyler (İP4) birbirinden bağımsızdır. Robot gecikirse statik deneyler bekletilmez.

**İP5 temposu:** 16 blok / 4 ay ≈ haftada 1 blok (6 koşu, yarım gün). Sınav haftaları için pay var.

## 2. Kilometre taşları

| KT | Ne zaman | Ne olmuş olmalı? | Olmadıysa |
|---|---|---|---|
| **KT1** | Ay 1 sonu | 2 kart geldi; ESP-NOW ile iki kart arasında paket gidip geliyor | Tedarikçi değiştir, kalan kart siparişini beklet |
| **KT2** | Ay 2 sonu | Router ve PHY hızı kararı verildi; kayıt betiği çalışıyor; robot sürülüyor | Mesh için router al ya da Mesh-Lite'a geç; PHY sabitlenemiyorsa iki protokolü 802.11b'ye sınırla (K-43) |
| **KT3** | Ay 4 sonu | 5 rölelik zincir 2 protokolde çalışıyor; mekanizma kabul testini geçti | Mekanizma geciktiyse elle bırakma B planıyla İP3'e geç |
| **KT4** | Ay 5 sonu | Ayarlar donduruldu | Dondurmadan ana deneye başlanmaz |
| **KT5** | Ay 6 sonu | Aşama 1 bitti; analiz betikleri çalışıyor | Aşama 2'ye başlanabilir ama Aşama 1 Ay 7 içinde bitirilir |
| **KT6** | Ay 10 sonu | Aşama 2 bitti | Eksik blokları 11. ayın ilk haftalarında tamamla (İP6'nın ilk ayı tampon) |
| **KT7** | Ay 12 sonu | Analiz, açık veri seti ve kod yayımlandı | — |

**Sınav dönemleri:** Ders takvimindeki vize ve final haftaları bu çizelgeye işaretlenmeli. Saha deneyleri o haftalara planlanmaz.

## 3. Riskler ve B planları

| Risk | Olasılık | Etki | Önlem / B planı | Sorumlu |
|---|---|---|---|---|
| ESP-WIFI-MESH'in router'sız sabit kökle çalışmaması | Orta | Orta | Deneye özel router; adillik için iki protokolde de açık tutulur. Mesh hiç kararlı değilse ESP-Mesh-Lite. | Hatice |
| Mesh'te PHY hızının sabitlenememesi | Orta | Orta | İki protokolün radyosu aynı kipe (ör. yalnızca 802.11b) sınırlanır; kalan fark sınırlılık olarak raporlanır (K-43). | Hatice |
| Mesh'in bekleme modundan geçişte (ESP-NOW → mesh) sorun çıkarması | Orta | Orta | Ön testte denenir; gerekirse röle `ACTIVATE` sonrası yeniden başlatılıp doğrudan mesh'te açılır. Geçiş süresi bağlanma süresine dahildir. | Hatice |
| Pilotta bir protokolün çok atlamada kararlı çalışmaması | Düşük | Yüksek | Mesh için ESP-Mesh-Lite'a geçilir; SBZ'deki hatalar ayarlar dondurulmadan önce İP3'te giderilir. Ana deneyde kötü sonuç bulgudur. | Aleyna |
| Ortak RSSI okuma yönteminin bir protokolde uygulanamaması | Orta | Orta | Ön testte alternatif (promiscuous mod) denenir; fark raporlanır. | Hatice |
| Ölçümlerde yüksek değişkenlik | Yüksek | Orta | Robotlu koşullarda 8, statik koşullarda 5 tekrar; tam blok düzeni; gün etkisini hesaba katan model; kanal seçimi; operatör kuralı; ortam kaydı. | Aleyna |
| Koridorların kısa kalması / sinyalin çok güçlü çıkması | Orta | Orta | Verici gücü K'de iki protokol için eşit düşürülür; O1 ve O2'de rotanın tek bağlantıyla kapsanamadığı kontrol edilir. | Hatice |
| Test ortamında 5 rölenin rotayı kapsamaya yetmemesi | Orta | Düşük | Bulgudur: koşu "başarısız" sayılır, ulaşılan mesafe raporlanır. Ayar değiştirilmez. | Aleyna |
| Bırakma mekanizmasının takılması | Orta | Düşük | Robot durur, düğüm elle bırakılır; karar ve kayıt değişmez. | Hacer |
| Bırakmada röle kutusunun veya kartın kırılması (~500 bırakma) | Orta | Orta | Düşük bırakma yüksekliği, darbeye dayanıklı kutu, her koşu sonrası kontrol; kutu filamentle yeniden basılır, kart için yedek var. | Mustafa |
| 3D yazıcıya erişimin aksaması | Düşük | Orta | Başka bir atölyede basılır; filament bütçede. | Mustafa |
| Deney ortamlarına erişim izninin alınamaması | Düşük | Yüksek | İzin İP1'de yazılı alınır; her ortam türü için yedek bina belirlenir. | Aleyna |
| Tablanın adım kaçırması veya kayması | Düşük | Orta | Ev konumu sensörü yok; her koşudan önce elle hizalama; mutlak adım hedefi; bekleme akımı hiç kesilmez; her turda işaret gözle kontrol edilir. Kayma görülürse koşu tekrarlanır. | Hacer |
| Robotun arızalanması veya ağırlaşması (mikro motorların yükte zorlanması) | Orta | Orta | Ağırlık bütçesi; kısa gövdeli NEMA 17; ağırlık merkezi tahrik tekerine yakın; yumuşak hızlanma. Arızada ağ kartı elle itilen arabaya taşınır. | Hacer |
| Enkoder mesafe hatası (dönüşlerde kayma) | Yüksek | Düşük | Zemin işaretleri ve lazer metreyle doğrulanır; sapma kaydedilir. | Hacer |
| Motor gürültüsünün kartları resetlemesi | Orta | Orta | Motor (XL4015) ve kart (LM2596) hatları ayrı, ortak toprak, motor kondansatörleri, yumuşak hızlanma. | Hacer |
| 3S Li-ion pilin yanlış bağlanması veya şarjı | Düşük | Yüksek | Balanslı BMS (B1, B2 dahil), sigorta, yalnızca 12,6 V CC/CV şarj cihazı, aynı model hücreler; şarj gözetimsiz bırakılmaz. | Hacer |
| Operatörün sinyali gölgelemesi | Yüksek | Orta | Operatör konumu kuralı ([deney_plani.md](deney_plani.md) 3. bölüm). | Herkes |
| Tedarik gecikmesi / fiyat artışı | Orta | Orta | Önce 2 kartla ön test; kısılabilecek kalemler belli ([maliyet.md](maliyet.md) 6. bölüm). | Aleyna |
| Ekip üyesinin bir dönem ayrılamaması | Orta | Orta | Her işin bir yedeği var ([ekip.md](ekip.md)); belgeler güncel tutulur; İP6'nın ilk ayı tampon. | Aleyna |
| Kamera trafiğine eşdeğer sonuç iddiası | — | — | Sonuçlar kamera yükü benzetimiyle sınırlı olarak yazılır. | Aleyna, Hatice |

## 4. Proje bitti sayılması için

- [ ] Sekiz XIAO ESP32-C3 aynı ESP-IDF sürümüyle programlanabiliyor.
- [ ] P1 ve P2 aynı PHY hızında, 5 rölelik zincirde veri iletiyor; bekleme modu ve "aktif ol" komutu iki protokolde çalışıyor.
- [ ] Robot kablolu kumandayla sürülüyor; enkoder mesafesi kaydediliyor.
- [ ] Mekanizma 100 bırakmanın en az 95'inde düğümü düz bırakıyor; tabla kaymıyor.
- [ ] Üç kural tekrarlanabilir bırakma kararları veriyor.
- [ ] `deney_ayarlari.md` ana deneylerden önce tarihli olarak donduruldu.
- [ ] 20 statik ve 96 robotlu koşu tamamlandı; ham kayıtlar korunuyor.
- [ ] Sonuçlarda router durumu, kanal, verici gücü, PHY hızı, anten ve trafik koşulları belirtildi.
- [ ] Veri seti, kod ve 3D baskı dosyaları herkese açık bir depoda.
