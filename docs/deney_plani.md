# Deney Planı

**Son güncelleme:** 6 Kasım 2026 · **Sürüm:** 3.0 (sade tasarım, [kararlar.md](kararlar.md) K-42)  
**İlke:** Önce kalibre et → sonra dondur → sonra ölç.

Metrik tanımları ve kayıt biçimi: [olcum_ve_analiz.md](olcum_ve_analiz.md). Ayarlar: [deney_ayarlari.md](deney_ayarlari.md). Protokoller ve kurallar: [protokoller_ve_stratejiler.md](protokoller_ve_stratejiler.md).

> **Sürüm 2.1'den farklar:** 3 yerine 2 protokol (ham ESP-NOW çıktı). Düz koridor artık yalnızca kalibrasyon ortamı. Kapasite eğrisi (1A), arıza testi ve S★ çıktı. Robotlu koşullarda tekrar sayısı 8'e çıktı. Toplam koşu ~180'den ~120'ye indi.

## 1. Ana fikir ve aşamalar

Bağlantı en çok **röle bırakma anında** kopar. Robot menzil sınırındayken yeni röle yere düşer ve ağa katılana kadar veri akışı kesilebilir. Bu kesintiyi iki şey belirler: rölenin **ne zaman** bırakıldığı (kural) ve ağa **ne kadar hızlı** katıldığı (protokol).

Uyarlamalı kurallar (S2, S3) röleleri her protokolde **farklı yerlere** bırakır. Protokolleri yalnızca robotla karşılaştırırsak "bu fark protokolden mi, rölelerin yerinden mi?" sorusunu cevaplayamayız. Bu yüzden üç aşama var:

| Aşama | Nerede? | Ne değişiyor? | Ne sabit? | Hangi soruyu cevaplıyor? |
|---|---|---|---|---|
| **0 — Kalibrasyon ve pilot** | K | Ayarlar aranır | — | Ayarlar ne olmalı? |
| **1 — Statik zincir** | O1, O2 | Yalnızca protokol | Röle konumları | AS1: Yeni röle hangi protokolde ne kadar hızlı katılır? |
| **2 — Robotla dinamik** | O1, O2 | Protokol ve kural | Ayarlar, rota | AS1 (bırakma anı), AS2: Hangi kural daha iyi? AS3: Kural ve protokol birbirini telafi ediyor mu? |

## 2. Ortamlar

| Kod | Ortam | Rolü | Ne sınıyor? | Yer (doldurulacak) |
|---|---|---|---|---|
| **K** | Düz koridor | **Yalnızca kalibrasyon** | Görüş hattı açık; kuralların bütün ayarları burada yapılır | [bina, kat] |
| **O1** | L dönüşlü koridor | Test | Köşede görüş hattının birden kopması; enkaz içindeki ani kırılmalar | [bina, kat] |
| **O2** | Betonu yoğun bodrum veya çok kapılı koridor | Test | Sürekli yüksek zayıflama; göçük altı yapılara en yakın ortam | [bina, kat] |

- Rota uzunluğu hedefi **40–60 m**.
- Verici gücü K'de, tek bir bağlantının rotayı kapsayamayacağı biçimde **iki protokol için eşit** belirlenir.
- **O1 ve O2'de kural ayarı yapılmaz.** Yalnızca rotanın tek bağlantıyla kapsanamadığı kontrol edilir.
- Her ortam için bir kez kaydedilir: rota çizimi, başlangıç ve bitiş noktaları, ölçülen mesafeler, kapılar ve engeller, rotadaki her 5 m'nin zemin işareti.
- Her deney gününde kaydedilir: kanal taraması (ortamdaki Wi-Fi ağları), saat, ortamdaki insan yoğunluğu.

## 3. Sabit tutulanlar

Kart modeli, anten ve anten yönü, düğüm yüksekliği (zemin), kanal, verici gücü, **PHY hızı**, paket boyutu, ortak yük, robot hızı, router durumu (açık/kapalı), ESP-IDF sürümü.

- **Operatör kuralı:** İnsan vücudu 2,4 GHz sinyali belirgin biçimde zayıflatır. Operatör her koşuda robotun **önünde veya yanında, sabit bir mesafede** yürür ve robot ile son röle arasına asla girmez. Statik testlerde kimse zincir hattının üstünden geçmez.
- **Pil:** Röleler her deney gününden önce tam şarj edilir. Koşular arasında pil seviyesi düşükse yedek pil takılır.

## 4. Aşama 0 — Kalibrasyon ve pilot (K ortamı)

**Amaç:** Ayarları kuralla belirlemek ve dondurmak.

1. **Kanal taraması:** Üç ortamda da 1, 6 ve 11. kanallardaki ağlar sayılır; en az kullanılan kanal seçilir.
2. **PHY hızı kontrolü:** İki protokolün aynı PHY hızında çalıştığı doğrulanır (İP1'de seçilen yöntemle).
3. **Kalibrasyon yürüyüşü** (K'de, 2 tur): Bir düğüm sabitken diğeri zemin seviyesinde metre metre uzaklaşır, her noktada 20–30 s bekler; RSSI ve teslim oranı kaydedilir. Ayrıntı: [protokoller_ve_stratejiler.md](protokoller_ve_stratejiler.md) B6.
4. **Kapasite taraması** (K'de, 5 rölelik zincir, protokol başına 3 tarama):
   - Yük 32 kbit/s'den başlayıp her **30 s**'de yaklaşık **1,4 katına** çıkar (32 → 45 → 64 → 90 → 128 → …).
   - Her basamağın ilk 5 s'si geçiş sayılır, hesaba katılmaz.
   - Teslim oranı %95'in altına düşmeden önceki son yük **en yüksek sürdürülebilir hızdır**.
   - **Ortak yük** = zayıf kalan protokolün bu hızının ~%70'i.
5. **Robotlu deneme koşuları** (K'de, birkaç tane): Üç kuralın tekrarlanabilir bırakma kararı ürettiği ve tablanın düzgün çalıştığı kontrol edilir.
6. **Test ortamı kontrolü** (O1, O2'de birer kez): Rotanın tek bağlantıyla kapsanamadığı doğrulanır; Aşama 1 röle konumları belirlenir (komşu çiftleri benzer RSSI'de, ~−70 dBm).
7. Ayarlar [deney_ayarlari.md](deney_ayarlari.md)'deki kurallarla belirlenir, belge tarihlenir ve **dondurulur**.

**Çıktılar:** mesafe–RSSI eğrisi, RSSI–teslim eğrisi ve "uçurum" noktası, ortak yük, eşik, X, T, kanal, verici gücü, Aşama 1 röle konumları.

**Pilot verileri hipotez testlerinde kullanılmaz.**

## 5. Aşama 1 — Statik zincir (robotsuz, O1 ve O2)

Röleler Aşama 0'da bulunan konumlara lazer metreyle yerleştirilir ve yer bantla işaretlenir. Değişen tek şey protokoldür.

**Kurulum:** Röle 1–4 açık ve yerinde; Röle 5 yerinde ama bekleme modunda. Robot kartı, Röle 5 olmadan Röle 4'e **ulaşamayacağı** konumda. Bu kurulumda bir kez kontrol edilir.

**Zaman çizelgesi:** Olaylar röle yazılımına programlanır; kimse elle müdahale etmez.

```text
 t=0 s              t=60 s                         t=360 s
 "BAŞLA"            Röle 5 bekleme modundan         bitir
 (merkez EVENT)     aktif olur → BAĞLANMA SÜRESİ
                    Röle 5 katıldıktan 360 s'ye kadar: KARARLI DURUM
                    → teslim oranı, goodput, RTT p50/p95, jitter
```

- Merkez t=0'da `EVENT` paketiyle koşuyu başlatır; röleler zamanlayıcılarını buna göre kurar.
- Süreler merkezdeki paket zaman damgalarından betikle hesaplanır; kronometre kullanılmaz.
- Röle 5 hiç katılmazsa bağlanma süresi **"katılmadı"** olarak kaydedilir (sansürlü). Bu bir hata değil, bulgudur.

**Koşu sayısı:** 2 protokol × 2 ortam × 5 tekrar = **20 koşu**.

### 5.1. Bir statik koşunun yapılışı (~8 dk, 1 kişi yeterli)

1. Röleleri bantlı yerlerine koy, anten yönünü kontrol et, aç.
2. `python kosu.py --asama 1 --ortam O2 --protokol P2 --tekrar 3`
3. 6 dk bekle; kimse hattın üstünden geçmesin.
4. Betiğin koşu özetini kontrol et (gönderilen ve alınan sayılar makul mü?).
5. Sıradaki koşu.

Bir ortamın 10 koşusu (2 protokol × 5 tekrar) bir iki günde biter. Her gün iki protokolün de koşuları rastgele sırayla yapılır.

## 6. Aşama 2 — Robotla dinamik deney (O1 ve O2)

| Faktör | Düzeyler |
|---|---|
| Protokol | P1 (ESP-WIFI-MESH), P2 (SBZ) |
| Kural | S1, S2, S3 |
| Ortam | O1, O2 |
| Tekrar | **8** |

**Koşu sayısı:** 2 × 3 × 2 × 8 = **96 koşu**.

**Blok düzeni:** Her deney günü tek ortamda yapılır ve o ortamın altı koşulunun (2 protokol × 3 kural) **her birinden birer koşu** içerir. Sıra rastgele belirlenir. Bu bir "tam blok"tur; böylece gün etkisi koşullarla karışmaz. Bir günde 1 ya da 2 blok yapılabilir (6–12 koşu). 16 blok gerekir (2 ortam × 8 tekrar), yani 8–16 deney günü.

### 6.1. Bir robotlu koşunun yapılışı (2 kişi)

1. Robot başlangıç çizgisinde, merkezin yanında. 5 röle tamburda, bekleme modunda. Tabla elle hizalama işaretine getirildi (boş bölme açıklıkta), ardından robot açıldı (`H` mesajı alındı). Piller dolu.
2. Merkezde kayıt başlatılır: `python kosu.py --asama 2 --ortam O2 --protokol P1 --kural S3 --tekrar 2`
3. Operatör robotu rota boyunca sürer. Hız yazılımla sabittir (~0,3 m/s); operatör yalnızca ileri ve dön komutu verir.
4. Robot ağ kartı ortak yükte sürekli veri gönderir; seçilen kural sürekli bırakma kararını değerlendirir.
5. Karar çıkınca: robot durur → tabla döner → röle düşer → "aktif ol" gönderilir → `ACT_ACK` gelir → bırakma konumu ve zamanı kaydedilir → **robot, rölenin ağa katılmasını beklemeden** yoluna devam eder.
6. Koşunun sonu:
   - hedefe bağlantıyla ulaşıldıysa **başarılı**,
   - 5 röle kullanıldıktan sonra bağlantı koptuysa **başarısız**; ulaşılan son nokta kaydedilir.
7. Bırakılan rölelerin yerleri zemine işaretlenir ve lazer metreyle ölçülür (enkoder doğrulaması). Röleler toplanır ve kırık var mı diye kontrol edilir.

## 7. Koşu sayıları

| Aşama | Hesap | Koşu | Yaklaşık süre |
|---|---|---:|---|
| 0 — Kalibrasyon | K'de 2 yürüyüş + 2 protokol × 3 kapasite taraması + deneme koşuları; O1 ve O2'de rota kontrolü | 6 tarama | 2–3 gün |
| 1 — Statik | 2 protokol × 2 ortam × 5 tekrar | 20 | ~8 dk |
| 2 — Robotlu | 2 protokol × 3 kural × 2 ortam × 8 tekrar | 96 | ~10–15 dk |
| **Toplam** | | **~120** | |

## 8. Ayarlar ve zorunlu değişiklik kuralı

- Ayarlar Aşama 0 sonunda [deney_ayarlari.md](deney_ayarlari.md)'ye yazılır, tarihlenir ve **ana deneyler boyunca değiştirilmez**.
- Ana deneyde bir protokolün ya da kuralın kötü sonuç vermesi ayar değiştirme gerekçesi değil, **bulgudur**. Sabit mesafenin bir test ortamında kötü çıkması tam da H2'nin sınadığı şeydir.
- Zorunlu bir değişiklik gerekirse (ör. kart arızası, ortamın kullanılamaz hâle gelmesi) değişiklik o bloktaki **tüm koşullara** uygulanır, blok baştan tekrarlanır ve gerekçesiyle [kararlar.md](kararlar.md)'ye yazılır.

## 9. Sahaya çıkmadan önce kontrol listesi

- [ ] Röleler ve yedek piller tam şarjlı
- [ ] Tüm kartlarda doğru yazılım sürümü ve doğru protokol yüklü
- [ ] Kanal, verici gücü ve PHY hızı ayar belgesiyle aynı
- [ ] Router durumu ayar belgesiyle aynı (açıksa yerinde ve açık)
- [ ] Antenler her kutuda aynı yönde
- [ ] Zemin işaretleri yerinde (röle konumları, başlangıç, bitiş, her 5 m)
- [ ] Lazer metre, bant, yedek kablo yanında
- [ ] Dizüstü bilgisayar şarjlı; kayıt betiği deneme koşusuyla test edildi
- [ ] (Aşama 2) Tabla hizalama işaretinde, 5 röle yerleştirildi, robot pili dolu
- [ ] (Aşama 2) Günün blok sırası rastgele belirlendi ve günlüğe yazıldı

## 10. Deney günlüğü şablonu

Her deney günü için `veri/gunluk/YYYY-AA-GG.md` dosyası açılır ve aşağıdaki şablon kopyalanır.

```markdown
# Deney günlüğü — YYYY-AA-GG

- **Ortam:** K / O1 / O2   **Bina, kat:**
- **Kim vardı:**
- **Başlangıç–bitiş saati:**
- **Kanal / ortamdaki ağ sayısı (1–6–11):**
- **Router:** açık / kapalı
- **PHY hızı:**
- **Yazılım sürümü (git kısa hash):**
- **Hava / ortam notu:** (kalabalık, kapılar açık mı, vb.)

| Sıra | run_id | Aşama | Blok | Protokol | Kural | Tekrar | Başladı | Sonuç | Not |
|---:|---|---|---:|---|---|---:|---|---|---|
| 1 | | | | | | | | | |

## Ters giden ne oldu?

## Sonraki gün için not
```
