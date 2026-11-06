# Deney Planı

**Son güncelleme:** 6 Kasım 2026 · **Sürüm:** 2.1  
**İlke:** Önce keşfet → sonra dondur → sonra ölç.

Metrik tanımları ve kayıt biçimi: [olcum_ve_analiz.md](olcum_ve_analiz.md). Ayarlar: [deney_ayarlari.md](deney_ayarlari.md). Protokoller ve kurallar: [protokoller_ve_stratejiler.md](protokoller_ve_stratejiler.md).

## 1. Neden üç aşama?

Uyarlamalı kurallar (S2, S3) röleleri her protokolde **farklı yerlere** bırakır. Protokolleri doğrudan robotla karşılaştırırsak "bu fark protokolden mi, rölelerin yerinden mi?" sorusunu cevaplayamayız. Bu yüzden:

| Aşama | Ne değişiyor? | Ne sabit? | Hangi soruyu cevaplıyor? |
|---|---|---|---|
| **0 — Kalibrasyon ve pilot** | Her şey denenebilir | — | Ayarlar ne olmalı? S★ nedir? |
| **1 — Statik zincir** | Yalnızca protokol | Röle konumları | AS1: Hangi protokol daha iyi? AS4: Atlama arttıkça kapasite nasıl düşer? |
| **2 — Robotla dinamik** | Protokol ve kural | Ayarlar, rota | AS2: Hangi kural daha iyi? AS3: Protokol ve kural birlikte fazladan kazandırıyor mu? |

## 2. Ortamlar

| Kod | Ortam | Ne sınıyor? | Yer (doldurulacak) |
|---|---|---|---|
| **O1** | Düz koridor | Görüş hattı açık; temel senaryo | [bina, kat] |
| **O2** | L dönüşlü koridor | Köşede görüş hattının birden kopması; sabit mesafenin zayıf noktası | [bina, kat] |
| **O3** | Betonu yoğun bodrum veya çok kapılı koridor | Sürekli yüksek zayıflama; tünel ve enkaza en yakın ortam | [bina, kat] |

- Rota uzunluğu hedefi **40–60 m**. Sinyal beklenenden güçlü çıkarsa verici gücü **tüm protokoller için eşit** düşürülür.
- Her ortam için bir kez kaydedilir: rota çizimi, başlangıç ve bitiş noktaları, ölçülen mesafeler, kapılar ve engeller, rotadaki her 5 m'nin zemin işareti.
- Her deney gününde kaydedilir: kanal taraması (ortamdaki Wi-Fi ağları), saat, ortamdaki insan yoğunluğu.

## 3. Sabit tutulanlar

Kart modeli, anten ve anten yönü, düğüm yüksekliği (zemin), kanal, verici gücü, paket boyutu, ortak yük, robot hızı, router durumu (açık/kapalı), ESP-IDF sürümü.

- **Operatör kuralı:** İnsan vücudu 2,4 GHz sinyali belirgin biçimde zayıflatır. Operatör her koşuda robotun **önünde veya yanında, sabit bir mesafede** yürür ve robot ile son röle arasına asla girmez. Statik testlerde kimse zincir hattının üstünden geçmez.
- **Pil:** Röleler her deney gününden önce tam şarj edilir. Koşular arasında pil seviyesi düşükse yedek pil takılır.

## 4. Aşama 0 — Kalibrasyon ve pilot (robotsuz)

**Amaç:** Ortamları tanımak, ayarları kuralla belirlemek, S★'ı hesaplamak.

1. **Kanal taraması:** Her ortamda 1, 6 ve 11. kanallardaki ağlar sayılır.
2. **Açgözlü yerleşim yürüyüşü** (ortam başına 2 tur): Bir düğüm sabitken diğeri zemin seviyesinde metre metre uzaklaşır, her noktada 20–30 s bekler; RSSI ve teslim oranı kaydedilir. Bağlantının iyi olduğu en uzak noktada sabit düğüm oraya taşınır. Ayrıntı: [protokoller_ve_stratejiler.md](protokoller_ve_stratejiler.md) B6.
3. **Çıktılar:**
   - mesafe–RSSI eğrisi,
   - RSSI–teslim oranı eğrisi; teslimin birden düştüğü "uçurum" noktası,
   - S★ (röle sayısı ve konumları),
   - Aşama 1 röle konumları (komşu çiftleri ~−70 dBm'de).
4. **Protokol kontrolü:** Üç protokolün en az 3 atlamada veri ilettiği doğrulanır.
5. **1A ön taraması:** Ortak yükü belirlemek için O1'de 6 atlamada bir kapasite taraması.
6. Ayarlar [deney_ayarlari.md](deney_ayarlari.md)'deki kurallarla belirlenir, belge tarihlenir ve **dondurulur**.

**Pilot verileri hipotez testlerinde kullanılmaz.**

## 5. Aşama 1 — Statik zincir (robotsuz)

Röleler Aşama 0'da bulunan konumlara lazer metreyle yerleştirilir ve yer bantla işaretlenir. Değişen tek şey protokoldür.

### 5.1. 1A — Kapasite eğrisi (AS4)

- **Yalnızca O1'de**, 1, 3 ve 6 atlamada.
  - 1 atlama: röle yok, robot kartı Röle 1'in konumunda.
  - 3 atlama: Röle 1 ve 2 açık, robot kartı Röle 3'ün konumunda.
  - 6 atlama: 5 röle açık, robot kartı 6. konumda.
- Yük her **30 s**'de ikiye katlanır: 32 → 64 → 128 → 256 → 512 → 1024 kbit/s (bir tarama ~3 dk).
- Her basamağın ilk 5 s'si geçiş sayılır, hesaba katılmaz.
- Teslim oranı %95'in altına düşmeden önceki son yük **en yüksek sürdürülebilir hızdır**.
- Merkezdeki betik taramayı kendisi yürütür.
- Sonuç 360p (~0,5 Mbit/s), 480p (~1 Mbit/s) ve 720p (~2 Mbit/s) eşdeğer hızlarla karşılaştırılır.

**Koşu sayısı:** 3 protokol × 3 atlama sayısı × 5 tekrar = **45 tarama**.

### 5.2. 1B — Birleşik test (AS1)

**Kurulum:** Röle 1–4 açık ve yerinde; Röle 5 yerinde ama bekleme modunda. Robot kartı, Röle 5 olmadan Röle 4'e **ulaşamayacağı** konumda. Bu kurulumda bir kez kontrol edilir.

**Atlama kontrolü:** Röle 3 kapanınca Röle 4'ün Röle 2'ye doğrudan ulaşıp ulaşamadığı kurulumda ölçülür ve kaydedilir. Ulaşamıyorsa hiçbir protokol toparlanamaz ve test anlamsızlaşır. Bu yüzden konumlar, k−2 bağlantısı zayıf ama kullanılabilir olacak şekilde seçilir.

**Zaman çizelgesi:** Tüm olaylar röle yazılımına programlanır; kimse elle düğme kapatmaz.

```text
 t=0 s          t=60 s               t=360 s               t=480 s          t=600 s
 "BAŞLA"         Röle 5 bekleme       Röle 3 Wi-Fi'ı         Röle 3 Wi-Fi'ı    bitir
 (merkez)        modundan aktif olur  kapatır (arıza)        tekrar açar
                 → BAĞLANMA SÜRESİ    → TOPARLANMA SÜRESİ    → GERİ KATILMA
 60–360 s arası: KARARLI DURUM → teslim oranı, goodput, RTT, jitter
```

- Arıza `esp_wifi_stop()` / `esp_wifi_start()` ile benzetilir; ağ açısından fişi çekip takmakla eşdeğerdir.
- Merkez t=0'da `EVENT` paketiyle koşuyu başlatır; röleler zamanlayıcılarını buna göre kurar.
- Süreler merkezdeki paket zaman damgalarından betikle hesaplanır; kronometre kullanılmaz.
- P3'te atlama olmadığı için toparlanma beklenmez. Bu bir hata değil, ölçülen bir sonuçtur ("toparlanmadı" olarak kaydedilir).

**Koşu sayısı:** 3 protokol × 3 ortam × 5 tekrar = **45 koşu**.

### 5.3. Bir statik koşunun yapılışı (~12 dk, 1 kişi yeterli)

1. Röleleri bantlı yerlerine koy, anten yönünü kontrol et, aç.
2. `python kosu.py --asama 1B --ortam O2 --protokol SBZ --tekrar 3`
3. 10 dk bekle; kimse hattın üstünden geçmesin.
4. Betiğin koşu özetini kontrol et (gönderilen ve alınan sayılar makul mü?).
5. Sıradaki koşu.

Koşular **protokole göre günlere gruplanır**, böylece kartlar sık yeniden programlanmaz. Gün içindeki sıra rastgele belirlenir.

## 6. Aşama 2 — Robotla dinamik deney (AS2, AS3)

| Faktör | Düzeyler |
|---|---|
| Protokol | P1 (ESP-WIFI-MESH), P5 (SBZ) |
| Kural | S1, S2, S3 (+ S★ hesapla) |
| Ortam | O1, O2, O3 |
| Tekrar | 5 |

P3, SBZ'nin özellikleri kapalı hâli olduğu için bu aşamada yoktur; katkısı Aşama 1'de ayrıştırılır.

**Koşu sayısı:** 2 × 3 × 3 × 5 = **90 koşu**.

### 6.1. Bir robotlu koşunun yapılışı (2 kişi)

1. Robot başlangıç çizgisinde, merkezin yanında. 5 röle tamburda, bekleme modunda. Tabla elle hizalama işaretine getirildi (boş bölme açıklıkta), ardından robot açıldı (`H` mesajı alındı). Piller dolu.
2. Merkezde kayıt başlatılır: `python kosu.py --asama 2 --ortam O3 --protokol P1 --kural S3 --tekrar 2`
3. Operatör robotu rota boyunca sürer. Hız yazılımla sabittir (~0,3 m/s); operatör yalnızca ileri ve dön komutu verir.
4. Robot ağ kartı ortak yükte sürekli veri gönderir; seçilen kural sürekli bırakma kararını değerlendirir.
5. Karar çıkınca: robot durur → tabla döner → röle düşer → "aktif ol" gönderilir → konum kaydedilir → robot devam eder.
6. Koşunun sonu:
   - hedefe bağlantıyla ulaşıldıysa **başarılı**,
   - 5 röle kullanıldıktan sonra bağlantı koptuysa **başarısız**; ulaşılan son nokta kaydedilir.
7. Bırakılan rölelerin yerleri zemine işaretlenir ve lazer metreyle ölçülür (enkoder doğrulaması). Röleler toplanır.

## 7. Koşu sayıları

| Aşama | Hesap | Koşu | Yaklaşık süre |
|---|---|---:|---|
| 0 — Kalibrasyon | 3 ortam × 2 tur | 6 yürüyüş | ortam başına yarım gün |
| 1A — Kapasite | 3 protokol × 3 atlama × 5 tekrar | 45 tarama | ~3 dk + kurulum |
| 1B — Birleşik test | 3 protokol × 3 ortam × 5 tekrar | 45 | ~12 dk |
| 2 — Robotlu | 2 protokol × 3 kural × 3 ortam × 5 tekrar | 90 | ~10–15 dk |
| **Toplam** | | **~180** | |

## 8. Ayarlar ve zorunlu değişiklik kuralı

- Ayarlar Aşama 0 sonunda [deney_ayarlari.md](deney_ayarlari.md)'ye yazılır, tarihlenir ve **ana deneyler boyunca değiştirilmez**.
- Ana deneyde bir protokolün ya da kuralın kötü sonuç vermesi ayar değiştirme gerekçesi değil, **bulgudur**.
- Zorunlu bir değişiklik gerekirse (ör. kart arızası, ortamın kullanılamaz hâle gelmesi) değişiklik o bloktaki **tüm koşullara** uygulanır, blok baştan tekrarlanır ve gerekçesiyle [kararlar.md](kararlar.md)'ye yazılır.

## 9. Sahaya çıkmadan önce kontrol listesi

- [ ] Röleler ve yedek piller tam şarjlı
- [ ] Tüm kartlarda doğru yazılım sürümü ve doğru protokol yüklü
- [ ] Kanal ve verici gücü ayar belgesiyle aynı
- [ ] Router durumu ayar belgesiyle aynı (açıksa yerinde ve açık)
- [ ] Antenler her kutuda aynı yönde
- [ ] Zemin işaretleri yerinde (röle konumları, başlangıç, bitiş, her 5 m)
- [ ] Lazer metre, bant, yedek kablo yanında
- [ ] Dizüstü bilgisayar şarjlı; kayıt betiği deneme koşusuyla test edildi
- [ ] (Aşama 2) Tabla hizalama işaretinde, 5 röle yerleştirildi, robot pili dolu
- [ ] Gün içindeki koşu sırası rastgele belirlendi ve günlüğe yazıldı

## 10. Deney günlüğü şablonu

Her deney günü için `veri/gunluk/YYYY-AA-GG.md` dosyası açılır ve aşağıdaki şablon kopyalanır.

```markdown
# Deney günlüğü — YYYY-AA-GG

- **Ortam:** O1 / O2 / O3   **Bina, kat:**
- **Kim vardı:**
- **Başlangıç–bitiş saati:**
- **Kanal / ortamdaki ağ sayısı (1–6–11):**
- **Router:** açık / kapalı
- **Yazılım sürümü (git kısa hash):**
- **Hava / ortam notu:** (kalabalık, kapılar açık mı, vb.)

| Sıra | run_id | Aşama | Protokol | Kural | Tekrar | Başladı | Sonuç | Not |
|---:|---|---|---|---|---:|---|---|---|
| 1 | | | | | | | | |

## Ters giden ne oldu?

## Sonraki gün için not
```
