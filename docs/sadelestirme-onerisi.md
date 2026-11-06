# Sadeleştirme Önerisi (uygulanmadı)

**Tarih:** 6 Kasım 2026
**Durum:** ✅ **Uygulandı (6 Kasım 2026).** Önce rafa kaldırılmıştı (K-41). Aynı gün başvuru metni bu tasarımla yeniden yazıldı (K-42): [TYBS_Basvuru_Metinleri.md](tubitak/TYBS_Basvuru_Metinleri.md).

Uygulamada netleşen noktalar:

- **Ham ESP-NOW tamamen çıktı.** Statik aşamada bile tutulmadı; düğüm atlama çıkınca SBZ ile arasındaki tek fark yeniden gönderim kalıyordu.
- **Kalibrasyon ortamı K (düz koridor).** Kuralların bütün ayarları (X, eşik, T) burada belirlenir ve test ortamlarında değiştirilmez. Bu sadece X için değil, üç kural için de geçerli; böylece karşılaştırma adil kalır.
- **Ortak yük:** K'de beş rölelik zincirde yapılan kapasite taramasıyla belirlenir (protokol başına 3 tarama).
- **Statik koşu:** 6 dakika. 60. saniyede 5. röle katılır, 60–360 s arası kararlı durum ölçülür. Arıza olayı yok.
- **Robot bırakmadan sonra beklemez.** Röle düşer ve robot hemen yoluna devam eder. "Bırakma anı kesintisi" hikâyesi buna dayanıyor.
- **Takvim:** İP4 1 ay (6. ay), İP5 4 ay (7–10. aylar). Katkılar %10 / 20 / 15 / 10 / 35 / 10.

Aşağıdaki metin önerinin ilk hâlidir.

---

## Neden böyle bir öneri var?

Asıl sorumuz iki eksenliydi: **hangi protokol, hangi bırakma kuralı?** Mevcut tasarım bu soruyu cevaplıyor, ama üstüne birkaç katman eklendi:

| Öğe | Asıl soruya hizmet ediyor mu? |
|---|---|
| 3 protokol × sabit zincir (Aşama 1B) | Evet, çekirdek |
| 3 kural × robot (Aşama 2) | Evet, çekirdek |
| Kapasite eğrisi, 1/3/6 atlama (AS4) | Kısmen. Sonucu literatürde biliniyor (Li vd., 2001) |
| Protokol × kural etkileşimi (AS3) | Ek soru; 5 tekrarla zayıf kalıyor |
| S★ ideal yerleşim | Ek; tek referanslı sinyal haritasıyla hesaplanamıyor |
| 3 ortam | 2 ortam yeterli. Düz koridorda farkın küçük çıkmasını zaten bekliyoruz |
| Revolver mekanizması | Mühendislik yükü; araştırma sorusu için şart değil |

Tasarım, sorunun gerektirdiğinden yaklaşık 1,5 kat büyük. Aşamalı kurulduğu için kırılgan değil: robot gecikirse Aşama 1 tek başına sonuç verir. Ama sade bir sürüm daha net bir hikâye anlatır.

## Ana fikir: bağlantı bırakma anında kopar

Breadcrumbing'de en kritik an röle bırakma anıdır. Robot menzilin sınırındadır, yeni röle yere düşer ve ağa katılması gerekir. Katılım uzun sürerse robot ilerlemeye devam eder ve bağlantı kopar. Bu anı iki şey belirler:

- **Kural:** Röle ne zaman bırakılıyor? Erken mi, sınırda mı?
- **Protokol:** Bırakılan röle ağa ne kadar hızlı katılıyor?

Tek cümlelik soru:

> **Ucuz Wi-Fi düğümleriyle breadcrumbing'de kesintiyi bırakma kuralı mı belirler, protokol mü, yoksa ikisi birbirini telafi eder mi?**

Bu soru dağınık parçaları tek mantığa bağlıyor:

- Mesh bağlanmadan önce ağı taradığı için yeni röle yavaş katılır.
- SBZ tarama yapmaz, yeni röle hemen katılır.
- Eğilim kuralı röleyi sınıra gelmeden, erken bırakır. Erken bırakmak mesh'in yavaş katılımını telafi edebilir.

Bu son madde, etkileşimi yan soru olmaktan çıkarıp projenin en ilginç ve önceden bilinmeyen bulgusu yapar.

## Sade tasarım

| | Mevcut (v2.1) | Sade öneri |
|---|---|---|
| Protokol | 3 (Mesh, ham ESP-NOW, SBZ) | **2** (Mesh, SBZ) |
| SBZ özellikleri | keşifsiz bağlanma + yeniden gönderim + düğüm atlama | keşifsiz bağlanma + yeniden gönderim |
| Kural | 3 | **3** (aynı) |
| Ortam | 3 | **2** test ortamı (L koridor, bodrum); düz koridor yalnızca kalibrasyonda |
| Tekrar | 5 | **8** |
| Statik aşama | kapasite taraması (45) + birleşik test (45) | yalnızca bağlanma ve kararlı durum: 2 × 2 × 5 = **20** koşu + ortak yükü belirleyen tek tarama |
| Robotlu aşama | 2 × 3 × 3 × 5 = 90 | 2 × 3 × 2 × 8 = **96** |
| Ek analizler | S★, kapasite eğrisi, arıza testi | yok |
| Toplam | ~180 koşu, 4 AS, 4–5 H | ~120 koşu, **3 H** |

### Hipotezler

- **H1 (protokol):** SBZ'de yeni rölenin ağa katılma süresi ve bırakma anındaki kesinti, mesh'tekinden kısadır.
- **H2 (kural):** Sabit mesafe kuralının X değeri tek bir kalibrasyon ortamında ayarlanır ve iki test ortamında da aynı değerle kullanılır. Bu, ortamın önceden bilinmediği gerçek görevi taklit eder. RSSI'ye dayalı kurallar iki ortamda da sabit mesafeden daha az kesinti veya daha az röle sağlar.
- **H3 (etkileşim):** Eğilim kuralının eşik kuralına üstünlüğü mesh'te SBZ'dekinden büyüktür, çünkü erken bırakma yavaş katılımı telafi eder.

### Ana ölçüt

Her bırakmanın çevresindeki kesinti süresi (örneğin bırakmadan 5 s öncesi ile 10 s sonrası arası). Yanında toplam kesinti, kullanılan röle sayısı ve ulaşılan mesafe raporlanır. Hepsi mevcut kayıt düzeniyle zaten ölçülüyor.

## Kazanç ve kayıp

**Kazandıklarımız:**

- **Tek cümlelik bir proje.** "Neyi neden ölçüyorsunuz" sorusunun cevabı hemen görülür.
- **Daha az tutarsızlık riski.** Ham ESP-NOW çıkınca protokoller arası ayrıştırma iddiası, S★, kapasite tavanı ve k−2 erişimi sorunları da kalkar.
- **Daha fazla istatistiksel güç.** Koşul sayısı azalır, tekrar 5'ten 8'e çıkar. Etkileşim testi anlamlı hale gelir.
- **Takvimde boşluk.** En sıkışık iş paketi İP2'ydi; bir protokol ve bir SBZ özelliği eksilince yük hafifler.

**Kaybettiklerimiz:**

- Kapasite eğrisi ve arıza toparlanma ölçümü. İkisi de ilginç ama asıl soru değil.
- Ham ESP-NOW zincirinin sağladığı ayrıştırma. Mesh ile SBZ birçok yönden farklıdır; bu durumda karşılaştırmaya dürüstçe "hazır çözüm ile sırayı bilen zincir" demek gerekir.

**Değişmeyenler:** robot, revolver mekanizması, ekip rolleri, bütçe, iş paketlerinin yapısı.

**Açık kalan soru:** Ham ESP-NOW zinciri yalnızca statik aşamada, bir ayar bayrağıyla neredeyse bedavaya tutulabilir mi?

## Ne zaman gündeme gelir?

- **Proje sırasında zaman daralırsa:** Önce S★ ve kapasite eğrisi bırakılır, sonra O1 robotlu koşuları. Başvuruda bunlar zaten ikincil soru olarak işaretlendi, bu yüzden bırakmak meşru.
- **Sonuç raporunda ya da bildiride:** "Bırakma anı kesintisi" çerçevesi, mevcut veriyle de anlatılabilir. Ana ölçüt olarak eklenmesi için yeni deney gerekmez; kayıtlardan hesaplanır.
- **Sonraki bir proje için:** Bu tasarım daha az koşuyla daha keskin bir soru sorar.
