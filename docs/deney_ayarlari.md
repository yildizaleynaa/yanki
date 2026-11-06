# Deney Ayarları (dondurulacak belge)

**Durum:** ⬜ Şablon. Aşama 0 (kalibrasyon ve pilot) bitince doldurulur. Sade tasarıma göre güncellendi ([kararlar.md](kararlar.md) K-42).  
**Dondurma tarihi:** —  
**Onaylayanlar:** — (dört ekip üyesi)

> Bu belge ana deneylerden **önce** doldurulur, tarihlenir ve dört kişi tarafından onaylanır. Dondurulduktan sonra değiştirilmez. Zorunlu bir değişiklik gerekirse aşağıdaki "Değişiklik kaydı" bölümüne yazılır ve [deney_plani.md](deney_plani.md) 8. bölümdeki kural uygulanır. Ana deneyde kötü sonuç almak değişiklik gerekçesi **değildir**.
>
> **Kural ayarları yalnızca K'de (düz koridor) belirlenir.** O1 ve O2'de hiçbir kural ayarı değiştirilmez.

## 1. Yazılım ve donanım

| Ayar | Değer | Not |
|---|---|---|
| ESP-IDF sürümü | | Tüm kartlarda aynı |
| Röle yazılımı sürümü (git hash) | | |
| Robot sürüş kartı yazılımı (git hash) | | |
| Kayıt betiği sürümü (git hash) | | |
| Anten tipi ve yönü | | Kutunun üst iç yüzü, aynı yön |
| Router | Kullanılıyor / kullanılmıyor | Kullanılıyorsa model, konum, kanal |
| **PHY hızı ve yöntemi** | | ESP-NOW: `esp_now_set_peer_rate_config`; mesh: `esp_wifi_config_80211_tx_rate` ya da iki protokolde aynı kip (K-43). Değer ve yöntem buraya yazılır. |

## 2. Kalibrasyonda (K) kuralla belirlenen ayarlar

| Ayar | Başlangıç değeri | Belirleme kuralı | **Seçilen değer** | Dayandığı veri |
|---|---|---|---|---|
| Kanal | 1 / 6 / 11 | Üç ortamda en az kullanılan kanal | | |
| Verici gücü | Tam güç | K'de tek bağlantının rotayı kapsayamayacağı değer; iki protokol için eşit | | |
| Ortak yük | 128 kbit/s | K'de 5 rölelik zincirde, **zayıf kalan protokolün** en yüksek sürdürülebilir hızının ~%70'i; tarama basamaklarından birine aşağı yuvarlanır | | |
| RSSI eşiği (S2, S3) | −75 dBm | K'deki RSSI–teslim eğrisinde uçurumun üzerinde güvenlik payı bırakacak değer | | |
| Sabit mesafe X (S1) | ~12 m | K'de sinyalin eşiğe düştüğü mesafenin biraz altında; **tek X**, test ortamlarına göre değişmez | | |
| Asgari bırakma aralığı d_min | 3 m | K'de art arda bırakmayı önleyen en küçük değer | | |
| S3: α, W, T, kayıp eşiği | 0,2 · 3–5 s · 3 s · %30 | K'deki deneme koşularında en kararlı çalışan değerler | | |
| SBZ: R | 2 | K'de kayıpları toparlayan ama gecikmeyi belirgin artırmayan en küçük değer | | |
| Robot hızı | 0,3 m/s | Düz gidiş ve düzgün bırakma sağlanan değer | | |
| Enkoder katsayısı (mm/darbe) | — | Zemin işaretli 5 m düz yolda 5 sürüşün ortalaması ([donanim.md](donanim.md) 3.5) | | |
| RSSI okuma yöntemi | ESP-NOW alım bilgisi | Ön testte iki protokolde çalışan yöntem | | |

## 3. Sabit tanımlar (kalibrasyonda değişmez)

| Tanım | Değer |
|---|---|
| Uygulama verisi | 160 bayt |
| Kesinti | 500 ms'den uzun süre paket gelmemesi |
| Bırakma anı penceresi | Bırakmadan 5 s önce – 10 s sonra |
| En yüksek sürdürülebilir hız | Teslim oranı ≥ %95 iken taşınan en yüksek yük |
| Kapasite taraması | 32 kbit/s'den başlayarak her 30 s'de ~1,4 kat; ilk 5 s geçiş |
| Yankı oranı | Her 10 DATA paketinden 1'i |
| Yoklama hızı | 10 Hz |
| Tekrar sayısı | Statik koşullarda 5, robotlu koşullarda 8 |
| Aşama 1 zaman çizelgesi | 0 / 60 / 360 s |
| Aşama 2 blok | Bir gün, bir ortam, 6 koşulun her birinden 1 koşu, rastgele sıra |

## 4. Kalibrasyon ve rota kontrolü

| Ortam | Rota uzunluğu (m) | Uçurum noktası (dBm) | Tek bağlantı rotayı kapsıyor mu? | Not |
|---|---|---|---|---|
| K | | | Hayır olmalı | Kural ayarları buradan |
| O1 | | — | Hayır olmalı | Ayar yapılmaz |
| O2 | | — | Hayır olmalı | Ayar yapılmaz |

## 5. Aşama 1 röle konumları

| Ortam | Röle 1 (m) | Röle 2 (m) | Röle 3 (m) | Röle 4 (m) | Röle 5 (m) | Robot (m) | Robot → Röle 4 bağlantısı yok mu? |
|---|---|---|---|---|---|---|---|
| O1 | | | | | | | |
| O2 | | | | | | | |

## 6. Değişiklik kaydı

| Tarih | Değişen ayar | Eski → yeni | Gerekçe | Tekrarlanan blok | Onaylayanlar |
|---|---|---|---|---|---|
| | | | | | |
