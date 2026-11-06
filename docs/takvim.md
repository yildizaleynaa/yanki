# Takvim ve Riskler

**Son güncelleme:** 6 Kasım 2026

Proje süresi **12 ay**. Başlangıç tarihi destek sonucuna bağlı olduğu için aylar "Ay 1, Ay 2…" diye numaralandı. Kim ne yapıyor: [ekip.md](ekip.md).

## 1. İş paketleri

```text
Ay        1    2    3    4    5    6    7    8    9   10   11   12
İP1     ████████                                                     Ön test + ölçüm altyapısı
İP2               ████████                                           Protokoller + mekanizma
İP3                         ████                                     Kurallar + pilot
İP4                              ████████                            Statik deneyler (90)
İP5                                        ████████████              Robotlu deneyler (90)
İP6                                                      ████████    Analiz + yayın
          ▲         ▲         ▲    ▲         ▲              ▲    ▲
         KT1       KT2       KT3  KT4       KT5            KT6  KT7
```

| İP | Ay | İş | Bitti sayılması için |
|---|---|---|---|
| **İP1** | 1–2 | ESP-NOW ve ESP-WIFI-MESH ön testleri, ortak paket biçimi, merkez kaydı ve betikler, robotun temel sürüşü | İki kartla iki yöntemde veri aktarılıyor; router gereksinimi belli; CSV kaydı çalışıyor; robot kumandayla sürülüyor ve enkoder mesafesi kaydediliyor |
| **İP2** | 3–4 | SBZ ve ESP-WIFI-MESH uygulaması, bekleme modu; bırakma mekanizması ve düğüm kutularının tasarımı, baskısı ve montajı | Üç protokol 5 rölelik zincirde veri iletiyor; mekanizma 100 bırakmada takılmıyor |
| **İP3** | 5 | Bırakma kuralları, üç ortamda kalibrasyon ve pilot, S★ | Eğriler çıkarıldı; [deney_ayarlari.md](deney_ayarlari.md) donduruldu; üç kural tekrarlanabilir karar veriyor |
| **İP4** | 6–7 | Aşama 1: 45 kapasite taraması + 45 birleşik test | 90 koşu tamamlandı, ham kayıtlar kontrol edildi |
| **İP5** | 8–10 | Aşama 2: 90 robotlu koşu | 90 koşu tamamlandı |
| **İP6** | 11–12 | İstatistiksel analiz, S★ karşılaştırması, açık veri ve kod | Hipotezler test edildi; veri ve kod herkese açık |

**Bağımsızlık:** Robot ve mekanizma (İP2) ile statik deneyler (İP4) birbirinden bağımsızdır. Robot gecikirse statik deneyler bekletilmez.

## 2. Kilometre taşları

| KT | Ne zaman | Ne olmuş olmalı? | Olmadıysa |
|---|---|---|---|
| **KT1** | Ay 1 sonu | 2 kart geldi; ESP-NOW ile iki kart arasında paket gidip geliyor | Tedarikçi değiştir, kalan kart siparişini beklet |
| **KT2** | Ay 2 sonu | Router kararı verildi; kayıt betiği çalışıyor; robot sürülüyor | Mesh için router al ya da Mesh-Lite'a geç |
| **KT3** | Ay 4 sonu | 5 rölelik zincir 3 protokolde çalışıyor; mekanizma kabul testini geçti | Mekanizma geciktiyse elle bırakma B planıyla İP3'e geç |
| **KT4** | Ay 5 sonu | Ayarlar donduruldu | Dondurmadan ana deneye başlanmaz |
| **KT5** | Ay 7 sonu | Aşama 1 bitti | Aşama 2'yi 1B bitmeden başlatma |
| **KT6** | Ay 10 sonu | Aşama 2 bitti | Eksik blokları 11. ayın ilk haftasında tamamla |
| **KT7** | Ay 12 sonu | Analiz, açık veri seti ve kod yayımlandı | — |

**Sınav dönemleri:** Ders takvimindeki vize ve final haftaları bu çizelgeye işaretlenmeli. Saha deneyleri o haftalara planlanmaz.

## 3. Riskler ve B planları

| Risk | Olasılık | Etki | Önlem / B planı | Sorumlu |
|---|---|---|---|---|
| ESP-WIFI-MESH'in router'sız sabit kökle çalışmaması | Orta | Orta | Deneye özel router; adillik için tüm protokollerde açık tutulur. Mesh hiç kararlı değilse ESP-Mesh-Lite. | Hatice |
| Mesh'in bekleme modundan geçişte (ESP-NOW → mesh) sorun çıkarması | Orta | Orta | Ön testte denenir; gerekirse röle `ACTIVATE` sonrası yeniden başlatılıp doğrudan mesh'te açılır. Geçiş süresi bağlanma süresine dahildir. | Hatice |
| Pilotta bir protokolün çok atlamada kararlı çalışmaması | Düşük | Yüksek | Yedek yönteme geçilir; olmazsa karşılaştırma kalan yöntemlerle sürdürülür ve sınırlılık olarak raporlanır. | Aleyna |
| Ortak RSSI okuma yönteminin bir protokolde uygulanamaması | Orta | Orta | Ön testte alternatif (promiscuous mod) denenir; fark raporlanır. | Hatice |
| Ölçümlerde yüksek değişkenlik | Yüksek | Orta | 5 tekrar, gün etkisini hesaba katan model, kanal seçimi, operatör kuralı, ortam kaydı. | Aleyna |
| Koridorların kısa kalması / sinyalin çok güçlü çıkması | Orta | Orta | Verici gücü tüm protokoller için eşit düşürülür. | Hatice |
| 1B'de Röle 4'ün Röle 2'ye hiç ulaşamaması | Orta | Orta | Kurulumda ölçülür; konumlar k−2 bağlantısı kullanılabilir olacak şekilde seçilir. | Aleyna |
| Bırakma mekanizmasının takılması | Orta | Düşük | Robot durur, düğüm elle bırakılır; karar ve kayıt değişmez. | Hacer |
| 3D yazıcıya erişimin aksaması | Düşük | Orta | Başka bir atölyede basılır; filament bütçede. | Mustafa |
| Tablanın adım kaçırması veya kayması | Düşük | Orta | Ev konumu sensörü yok; her koşudan önce elle hizalama; mutlak adım hedefi; bekleme akımı hiç kesilmez; her turda işaret gözle kontrol edilir. Kayma görülürse koşu tekrarlanır. | Hacer |
| Robotun arızalanması veya ağırlaşması (mikro motorların yükte zorlanması) | Orta | Orta | Ağırlık bütçesi; kısa gövdeli NEMA 17; ağırlık merkezi tahrik tekerine yakın; yumuşak hızlanma. Arızada ağ kartı elle itilen arabaya taşınır. | Hacer |
| Enkoder mesafe hatası (dönüşlerde kayma) | Yüksek | Düşük | Zemin işaretleri ve lazer metreyle doğrulanır; sapma kaydedilir. | Hacer |
| Motor gürültüsünün kartları resetlemesi | Orta | Orta | Motor (XL4015) ve kart (LM2596) hatları ayrı, ortak toprak, motor kondansatörleri, yumuşak hızlanma. | Hacer |
| 3S Li-ion pilin yanlış bağlanması veya şarjı | Düşük | Yüksek | Balanslı BMS (B1, B2 dahil), sigorta, yalnızca 12,6 V CC/CV şarj cihazı, aynı model hücreler; şarj gözetimsiz bırakılmaz. | Hacer |
| Operatörün sinyali gölgelemesi | Yüksek | Orta | Operatör konumu kuralı ([deney_plani.md](deney_plani.md) 3. bölüm). | Herkes |
| Tedarik gecikmesi / fiyat artışı | Orta | Orta | Önce 2 kartla ön test; kısılabilecek kalemler belli ([maliyet.md](maliyet.md) 6. bölüm). | Aleyna |
| Ekip üyesinin bir dönem ayrılamaması | Orta | Orta | Her işin bir yedeği var ([ekip.md](ekip.md)); belgeler güncel tutulur. | Aleyna |
| Kamera trafiğine eşdeğer sonuç iddiası | — | — | Sonuçlar kamera yükü benzetimiyle sınırlı olarak yazılır. | Aleyna, Hatice |

## 4. Proje bitti sayılması için

- [ ] Yedi XIAO ESP32-C3 aynı ESP-IDF sürümüyle programlanabiliyor.
- [ ] P1, P3 ve P5 en az 3 atlamada veri iletiyor; bekleme modu ve "aktif ol" komutu üç protokolde çalışıyor.
- [ ] Robot kablolu kumandayla sürülüyor; enkoder mesafesi kaydediliyor.
- [ ] Mekanizma her bölmeden 20 bırakmanın 20'sinde düğümü düz bırakıyor; tabla kaymıyor.
- [ ] Üç kural tekrarlanabilir bırakma kararları veriyor.
- [ ] `deney_ayarlari.md` ana deneylerden önce tarihli olarak donduruldu.
- [ ] 45 kapasite taraması, 45 birleşik test ve 90 robotlu koşu tamamlandı; ham kayıtlar korunuyor.
- [ ] Sonuçlarda router durumu, kanal, verici gücü, anten ve trafik koşulları belirtildi.
- [ ] Veri seti, kod ve 3D baskı dosyaları herkese açık bir depoda.
