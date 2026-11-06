# Deney Ayarları (dondurulacak belge)

**Durum:** ⬜ Şablon. Aşama 0 (pilot) bitince doldurulur.  
**Dondurma tarihi:** —  
**Onaylayanlar:** — (dört ekip üyesi)

> Bu belge ana deneylerden **önce** doldurulur, tarihlenir ve dört kişi tarafından onaylanır. Dondurulduktan sonra değiştirilmez. Zorunlu bir değişiklik gerekirse aşağıdaki "Değişiklik kaydı" bölümüne yazılır ve [deney_plani.md](deney_plani.md) 8. bölümdeki kural uygulanır. Ana deneyde kötü sonuç almak değişiklik gerekçesi **değildir**.

## 1. Yazılım ve donanım

| Ayar | Değer | Not |
|---|---|---|
| ESP-IDF sürümü | | Tüm kartlarda aynı |
| Röle yazılımı sürümü (git hash) | | |
| Robot sürüş kartı yazılımı (git hash) | | |
| Kayıt betiği sürümü (git hash) | | |
| Anten tipi ve yönü | | Kutunun üst iç yüzü, aynı yön |
| Router | Kullanılıyor / kullanılmıyor | Kullanılıyorsa model, konum, kanal |

## 2. Pilotta kuralla belirlenen ayarlar

| Ayar | Başlangıç değeri | Belirleme kuralı | **Seçilen değer** | Dayandığı veri |
|---|---|---|---|---|
| Kanal | 1 / 6 / 11 | Ortamlarda en az kullanılan kanal | | |
| Verici gücü | Tam güç | Zincir gerektirmeyecek kadar güçlü sinyalde tüm protokoller için eşit olarak düşürülür | | |
| Ortak yük | 128 kbit/s | 1A ön taramasında O1, 6 atlamada **en zayıf protokolün** en yüksek sürdürülebilir hızının ~%70'i; 32/64/128/256'ya aşağı yuvarlanır | | |
| RSSI eşiği (S2, S3) | −75 dBm | RSSI–teslim eğrisindeki uçurumun üzerinde güvenlik payı bırakacak değer | | |
| Sabit mesafe X (S1) | ~12 m | O1'de sinyalin eşiğe düştüğü mesafenin biraz altında | | |
| Asgari bırakma aralığı d_min | 3 m | Pilotta art arda bırakmayı önleyen en küçük değer | | |
| S3: α, W, T, kayıp eşiği | 0,2 · 3–5 s · 3 s · %30 | Pilotta en kararlı çalışan değerler | | |
| SBZ: R, N, T_dene | 2 · 3 · 1 s | Pilotta gereksiz atlama yapmayan, kayıpları toparlayan değerler | | |
| Robot hızı | 0,3 m/s | Düz gidiş ve düzgün bırakma sağlanan değer | | |
| Enkoder katsayısı (mm/darbe) | — | Zemin işaretli 5 m düz yolda 5 sürüşün ortalaması ([donanim.md](donanim.md) 3.5) | | |
| RSSI okuma yöntemi | ESP-NOW alım bilgisi | Ön testte üç protokolde çalışan yöntem | | |

## 3. Sabit tanımlar (pilotta değişmez)

| Tanım | Değer |
|---|---|
| Uygulama verisi | 160 bayt |
| Kesinti | 500 ms'den uzun süre paket gelmemesi |
| En yüksek sürdürülebilir hız | Teslim oranı ≥ %95 iken taşınan en yüksek yük |
| Yankı oranı | Her 10 DATA paketinden 1'i |
| Yoklama hızı | 10 Hz |
| Tekrar sayısı | Her koşulda 5 |
| 1B zaman çizelgesi | 0 / 60 / 360 / 480 / 600 s |

## 4. Aşama 1 röle konumları

| Ortam | Röle 1 (m) | Röle 2 (m) | Röle 3 (m) | Röle 4 (m) | Röle 5 (m) | Robot (m) | Röle 4 → Röle 2 RSSI |
|---|---|---|---|---|---|---|---|
| O1 | | | | | | | |
| O2 | | | | | | | |
| O3 | | | | | | | |

## 5. S★ (ideal yerleşim)

| Ortam | Rota uzunluğu (m) | S★ röle sayısı | Konumlar (m) |
|---|---|---|---|
| O1 | | | |
| O2 | | | |
| O3 | | | |

## 6. Değişiklik kaydı

| Tarih | Değişen ayar | Eski → yeni | Gerekçe | Tekrarlanan blok | Onaylayanlar |
|---|---|---|---|---|---|
| | | | | | |
