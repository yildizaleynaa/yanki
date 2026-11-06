# Ekip ve Görevler

**Son güncelleme:** 6 Kasım 2026

## Kişiler

| Kişi | Rol | Ana sorumluluk | Yedeği |
|---|---|---|---|
| **Aleyna** | Proje yürütücüsü | Koordinasyon; **SBZ protokolü** (ham ESP-NOW zinciri dâhil); ayar belgesi; istatistiksel analiz; kod ve veri setinin yayımlanması | Hatice (analiz) |
| **Hatice** | Ortak | **ESP-WIFI-MESH** uygulaması ve bekleme modu; **bırakma kuralları** (S1–S3); sinyal haritası ve pilot; ideal yerleşim (S★); analiz | Aleyna (ağ yazılımı) |
| **Hacer** | Ortak | **Bırakıcı robot** (2WD şasi tasarımı, ESP32-S3 sürüş kartı, motorlar ve enkoderler, güç sistemi, kumanda); mekanizmanın elektroniği, montajı ve kabul testi | Mustafa (mekanizma) |
| **Mustafa** | Ortak | **Ölçüm altyapısı** (merkez düğüm kaydı, RTT, koşu betikleri); bırakma mekanizması ve düğüm kutularının **3D tasarımı ve baskısı** | Hacer (montaj) |

"Yedeği" sütunu, biri hastalanırsa ya da sınav dönemindeyse işin kime geçeceğini gösterir. Yedek kişi asıl sahibin dosyalarını takip eder.

## İş paketi bazında kim ne yapıyor

| İP | Ay | Aleyna | Hatice | Hacer | Mustafa |
|---|---|---|---|---|---|
| **İP1** Ön test ve ölçüm altyapısı | 1–2 | ESP-NOW ön testleri, ortak paket biçimi | ESP-WIFI-MESH ön testi (router'sız kök) | Robot şasisi, sürüş testleri | Paket kaydı ve koşu betikleri |
| **İP2** Protokoller ve mekanizma | 3–4 | SBZ protokolü | ESP-WIFI-MESH uygulaması, bekleme modu | Mekanizma elektroniği, montaj, kabul testi | Mekanizma ve düğüm kutularının 3D tasarımı ve baskısı |
| **İP3** Kurallar ve pilot | 5 | Ayar belgesi | Kurallar, sinyal haritası, S★ | Kuralların robota entegrasyonu | Pilotta kayıt |
| **İP4** Statik deneyler | 6–7 | Koşu yürütme | Koşu yürütme | — | Koşu yürütme, kayıt kontrolü |
| **İP5** Robotlu deneyler | 8–10 | Merkez kaydı | Robot sürüşü ve bırakma | Robot sürüşü ve bırakma | Ağ izleme |
| **İP6** Analiz ve yayın | 11–12 | Analiz; kod ve veri yayını | Analiz | Donanım belgeleri | 3D baskı dosyaları |

## Arayüzler: kim kime ne teslim eder

Ekipteki işler birbirine bağlı. Bu sınırlar baştan net olursa beklemeler azalır.

| Veren | Alan | Ne? | Ne zaman? |
|---|---|---|---|
| Mustafa | Aleyna, Hatice | Merkez kayıt betiği ve paket biçimine uyan test aracı | İP1 sonu |
| Aleyna | Hatice | Ortak paket başlığı ve "aktif ol" komutunun tanımı ([protokoller_ve_stratejiler.md](protokoller_ve_stratejiler.md)) | İP1 sonu |
| Hacer ↔ Mustafa | — | Şasi ve tabla birlikte tasarlanır: motor, teker, pil ve kart yerleşimi; tablanın bağlantı noktaları | İP1 sonu |
| Mustafa | Hacer | Basılmış şasi (İP1), tabla, taban ve düğüm kutuları (İP2 ortası) | İP1 / İP2 |
| Hacer | Hatice | Robotun UART üzerinden mesafe göndermesi ve `BIRAK` komutunu alması | İP2 sonu |
| Hatice | Aleyna | Pilot eğrileri ve önerilen ayarlar | İP3 sonu |

## Karar verme

- **Teknik kararı işin sahibi verir.** Örneğin SBZ'nin yeniden gönderim sayısına Aleyna, tabla çapına Mustafa karar verir.
- **Birden fazla kişiyi etkileyen kararlar** (paket biçimi, deney ayarları, takvim kayması) kısa bir toplantıda alınır. Son sözü yürütücü söyler.
- **Deney ayarları** dondurulduktan sonra yalnızca dört kişinin onayıyla ve [deney_plani.md](deney_plani.md)'deki "zorunlu değişiklik" kuralıyla değiştirilir.
- Alınan her önemli karar [kararlar.md](kararlar.md)'ye yazılır.

## Çalışma düzeni (öneri)

- Haftada bir 30 dakikalık toplantı: geçen hafta ne bitti, bu hafta ne yapılacak, kim neyi bekliyor.
- Kod ve belgeler aynı depoda tutulur. Herkes kendi alanının dosyasını günceller.
- Sahaya en az **iki kişi** çıkar: Aşama 2'de biri robotu sürer, diğeri merkezde kaydı izler. Aşama 1 koşuları tek kişiyle de yapılabilir.
- Satın almalarda fatura **yürütücü veya ortaklardan biri adına** kesilir. Ayrıntı: [maliyet.md](maliyet.md).
