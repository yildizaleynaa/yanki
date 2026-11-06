# Robottan Röle Bırakma (Breadcrumbing) — Proje Belgeleri

**Son güncelleme:** 6 Kasım 2026

## Proje bir paragrafta

Kapalı ve engelli ortamlarda (koridor, bodrum, tünel) robotun kablosuz bağlantısı kısa mesafede kopar. Bunun bilinen bir çözümü, robotun ilerlerken arkasına **röle düğümleri bırakarak** çok atlamalı bir ağ kurmasıdır. Bu yaklaşıma *breadcrumbing* denir. Bu projede ucuz Wi-Fi kartlarıyla (XIAO ESP32-C3, düğüm başına ~650 TL) bu zinciri kuruyoruz ve iki soruyu ölçüyoruz:

1. **Röle ne zaman bırakılmalı?** Üç kural karşılaştırılıyor: sabit mesafe, sinyal eşiği, sinyal eğilimi.
2. **Röleler birbirine nasıl bağlanmalı?** Üç ağ yaklaşımı karşılaştırılıyor: hazır ESP-WIFI-MESH, ham ESP-NOW zinciri ve bizim protokolümüz **SBZ (Sıra Bilen Zincir)**.

Operatör robotu kablolu kumandayla sürer. Bırakma kararını robot kendisi verir ve arkasındaki revolver tipi tabladan düğümü yere bırakır. En fazla 5 röle kullanılır, deneyler 3 ortamda yapılır, her koşul 5 kez tekrarlanır.

## Dosyalar

| Dosya | Ne var? | Kim bakmalı? |
|---|---|---|
| [ekip.md](ekip.md) | Kim ne yapıyor, kararlar nasıl alınıyor | Herkes |
| [sistem.md](sistem.md) | Büyük resim: parçalar ve veri akışı | Herkes, önce bu |
| [donanim.md](donanim.md) | Kartlar, piller, robot elektroniği, pin ve güç şeması | Hacer, Mustafa |
| [birakma_mekanizmasi.md](birakma_mekanizmasi.md) | Revolver tabla tasarımı ve kabul testi | Mustafa, Hacer |
| [protokoller_ve_stratejiler.md](protokoller_ve_stratejiler.md) | Üç ağ yaklaşımı, SBZ'nin tam tanımı, üç bırakma kuralı | Aleyna, Hatice |
| [deney_plani.md](deney_plani.md) | Aşamalar, koşu prosedürleri, sahada kontrol listesi, günlük şablonu | Herkes |
| [deney_ayarlari.md](deney_ayarlari.md) | Pilottan sonra dondurulacak ayarlar (şimdilik boş şablon) | Aleyna |
| [olcum_ve_analiz.md](olcum_ve_analiz.md) | Metrik tanımları, kayıt biçimi, analiz ve hipotezler | Aleyna, Hatice, Mustafa |
| [maliyet.md](maliyet.md) | Malzeme listesi, fiyatlar, satın alma takibi | Aleyna |
| [takvim.md](takvim.md) | 12 aylık plan, kilometre taşları, riskler ve B planları | Herkes |
| [kararlar.md](kararlar.md) | Alınan kararlar ve nedenleri | Bir şeyi değiştirmeden önce |
| [kaynakca.md](kaynakca.md) | Literatür ve teknik belgeler | Yazım sırasında |
| [tubitak/](tubitak/) | 2209-A başvuru metinleri ve rehber | Başvuru süreci |

## Neredeyiz?

| Adım | Durum |
|---|---|
| Deney tasarımı | ✅ Tamamlandı (v2.1) |
| Bütçe | ✅ 12.000 TL, kalemler belli |
| TÜBİTAK 2209-A başvurusu | ⏳ Son gün 11 Kasım 2026, 17.30 |
| Malzeme alımı | ⬜ Başlamadı (önce 2 kartla ön test) |
| Yazılım ve donanım | ⬜ Başlamadı |

## Çalışma kuralları

- **Tek doğru kaynak bu klasördür.** Bir karar değişirse ilgili dosya güncellenir ve [kararlar.md](kararlar.md)'ye bir satır eklenir.
- **Deney ayarları pilottan sonra dondurulur** ([deney_ayarlari.md](deney_ayarlari.md)). Ana deneyler sırasında ayar değiştirilmez.
- **Her deney günü kayda geçer.** Şablon [deney_plani.md](deney_plani.md)'nin sonunda.
- Sahaya çıkmadan önce [deney_plani.md](deney_plani.md)'deki kontrol listesine bakılır.

---

## Sözlük

| Terim | Anlamı |
|---|---|
| **Breadcrumbing** | Robotun ilerlerken arkasına röle düğümleri bırakarak ağı uzatması (Hansel ile Gretel'in ekmek kırıntıları gibi) |
| **Röle (düğüm)** | Paketi alıp bir sonrakine ileten küçük cihaz. Bizde pil ve kutu içinde bir XIAO ESP32-C3 |
| **Merkez / kök** | Zincirin başı; bilgisayara USB ile bağlı kart. Tüm veriyi kaydeder |
| **Atlama (hop)** | Paketin bir düğümden diğerine bir kez gitmesi. 5 röle = 6 atlama |
| **RSSI** | Alınan sinyal gücü (dBm). −50 çok iyi, −75 zayıf, −90 kopmak üzere |
| **PDR** | Paket teslim oranı: merkeze ulaşan paket / gönderilen paket |
| **Goodput** | Merkeze gerçekten ulaşan faydalı veri hızı (kbit/s). Tekrarlar ve başlıklar sayılmaz |
| **RTT** | Gidiş-dönüş süresi: paketin gidip yankısının geri gelmesi (ms). p50 = ortanca, p95 = en kötü %5'in sınırı |
| **Jitter** | Gecikmenin paketten pakete dalgalanması |
| **Kesinti** | 500 ms'den uzun süre hiç paket gelmemesi |
| **ESP-NOW** | Espressif'in Wi-Fi radyosu üzerinde çalışan, bağlantı kurmadan doğrudan çerçeve gönderen yöntemi. Tek başına çok atlama yapmaz |
| **ESP-WIFI-MESH** | Espressif'in hazır mesh ağı: düğümler ebeveynlerini kendileri seçer, ağı kendileri onarır |
| **SBZ** | Sıra Bilen Zincir, bizim protokolümüz. Robot bıraktığı röleye "üstün şu, onun üstü şu" der; keşif beklenmez |
| **Bypass** | SBZ'de üst düğüm bozulunca paketi bir üstteki düğüme atlatmak |
| **Bekleme modu** | Tamburdaki rölenin açık olup ağa katılmaması; "aktif ol" komutunu beklemesi |
| **EWMA** | Üstel ağırlıklı hareketli ortalama: yeni ölçüme daha çok ağırlık veren yumuşatma |
| **S★** | İdeal yerleşim: rotayı en az röleyle kapsayan, sonradan hesaplanan konumlar. Kuralların ne kadar iyi olduğunu buna göre ölçeriz |
| **Pilot** | Ana deneyden önceki deneme ve ayar bulma aşaması. Verisi hipotez testinde kullanılmaz |
| **Dondurma** | Ayarların ana deneyden önce yazılıp bir daha değiştirilmemesi |
| **Karma etkili model** | Farklı günlerde yapılan koşuların "gün etkisini" hesaba katan istatistik modeli |
