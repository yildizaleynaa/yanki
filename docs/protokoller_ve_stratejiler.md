# Ağ Protokolleri ve Bırakma Kuralları

**Son güncelleme:** 6 Kasım 2026  
**Sorumlular:** Aleyna (SBZ ve ham ESP-NOW zinciri), Hatice (ESP-WIFI-MESH ve bırakma kuralları)  
**Durum:** Tasarım. Burada "başlangıç değeri" yazan sayılar pilotta kesinleşir ve [deney_ayarlari.md](deney_ayarlari.md)'ye yazılır.

---

# Bölüm A — Ağ protokolleri

## A1. Genel bakış

Üç yaklaşım aynı radyoyu kullanır; farkları ağ kurma ve veri aktarma yöntemindedir. İki kod tabanı yazılır: ESP-WIFI-MESH entegrasyonu ve ESP-NOW üzerinde kendi kodumuz. P3 ve P5 aynı kodun iki ayarıdır.

| Kod | Protokol | Rolü | Kodu |
|---|---|---|---|
| **P1** | ESP-WIFI-MESH | Hazır, kendini kuran ve onaran mesh | Entegrasyon ve ayar |
| **P3** | Ham ESP-NOW zinciri | Sade temel: sabit sıra, uygulama düzeyinde tekrar yok, atlama yok | SBZ kodu, özellikler kapalı |
| **P5** | **SBZ (Sıra Bilen Zincir)** | Bizim protokolümüz | Asıl geliştirme |

**Bu ayrımın değeri:** P1 → P3 farkı **keşifsiz bağlanmanın**, P3 → P5 farkı ise **yeniden gönderim ve atlamanın** etkisini gösterir. Böylece SBZ'nin hangi özelliğinin ne kazandırdığı ayrıştırılır.

## A2. Ortak kurallar (üç protokolde aynı)

| Konu | Kural |
|---|---|
| Kanal | Tek sabit kanal (1, 6 veya 11; pilotta en az kullanılan seçilir) |
| Verici gücü | Tüm kartlarda aynı; gerekirse pilotta eşit olarak düşürülür |
| Uygulama verisi | **160 bayt**: ilk 16 bayt robot telemetrisi (son RTT, RSSI, yol, bırakma sayısı; bkz. [olcum_ve_analiz.md](olcum_ve_analiz.md) 3.1), kalanı kamera yükünü temsil eden sentetik doldurma |
| Paket başlığı | Bölüm A3'teki ortak başlık. P1'de mesh yükünün içine konur. |
| Tekrarlı paket | Merkezde `(src, seq)` ile elenir |
| Bekleme modu | Tamburdaki röle ağa katılmaz; "aktif ol" komutunu bekler (A6) |
| Düğüm kimliği | 0 = merkez, 1–5 = röleler, 6 = robot ağ kartı, 7 = yedek. 8 kartın MAC adresleri ortak bir yapılandırma tablosunda tutulur. |

## A3. Ortak paket başlığı (P3, P5; P1'de yük içinde)

Tüm alanlar little-endian. Başlık 16 bayt, veri 160 bayt; toplam **176 bayt**. Bu, ESP-NOW v1'in 250 bayt sınırının altındadır.

| Bayt | Alan | Tür | Açıklama |
|---:|---|---|---|
| 0 | `type` | u8 | Paket türü (aşağıda) |
| 1–2 | `run_id` | u16 | Koşu numarası |
| 3–6 | `seq` | u32 | Kaynağa göre artan sıra numarası |
| 7 | `src` | u8 | Kaynak düğüm kimliği |
| 8 | `dst` | u8 | Hedef düğüm kimliği (0 = merkez) |
| 9 | `hop` | u8 | Geçilen atlama sayısı; her iletimde +1 |
| 10 | `ttl` | u8 | Kalan ömür; her iletimde −1, 0 olunca atılır (başlangıç 8) |
| 11 | `flags` | u8 | bit0: uygulama düzeyinde tekrar edildi · bit1: atlama (bypass) kullanıldı |
| 12–15 | `t_src` | u32 | Kaynağın gönderim anındaki yerel zamanı (ms). RTT için yankıda geri döner. |

| `type` | Ad | Yön | İçerik |
|---|---|---|---|
| 0x01 | `DATA` | robot → merkez | 160 B sentetik veri |
| 0x02 | `ECHO` | merkez → robot | Başlık + 0 B; `seq` ve `t_src` yansıtılır |
| 0x03 | `PROBE` | son röle → robot | Yoklama; yalnızca RSSI ölçümü için |
| 0x04 | `ACTIVATE` | robot → bırakılan röle | "Aktif ol" komutu (A6) |
| 0x05 | `ACT_ACK` | röle → robot | Aktif oldum |
| 0x06 | `EVENT` | merkez → röleler | Aşama 1B olaylarını zamanlamak için koşu başlangıcı |

**Yankı oranı:** Her 10 `DATA` paketinden birine yankı verilir (100 paket/s'de 10 yankı/s). Aşağı yön trafiği böylece küçük kalır.

## A4. P1 — ESP-WIFI-MESH

- ESP-IDF `esp_mesh_*` arayüzleri kullanılır. Merkez **sabit kök** olarak atanır (`esp_mesh_fix_root`), diğer düğümler kök seçimine katılmaz.
- **Router sorunu:** ESP-WIFI-MESH normalde kökün bir router'a bağlanmasını bekler. Router'sız sabit kök çalışması İP1'deki ön testte denenir. Çalışmazsa deneye özel router merkezin yanına konur ve **üç protokolün bütün koşularında açık** tutulur. Veri router'dan geçmez; merkez veriyi USB ile bilgisayara aktarır.
- Ebeveynini mesh kendisi seçer. Gerçekleşen ebeveyn ilişkileri ve değişimleri kaydedilir.
- **Bekleme modundan geçiş:** Röle bekleme modunda yalnızca ESP-NOW ile `ACTIVATE` dinler. Komut gelince ESP-NOW'u kapatıp mesh'i başlatır. Bu geçişin süresi bağlanma süresine dahildir. Geçişin sorunsuz çalıştığı ön testte doğrulanır.
- **Yoklama:** Son röle `PROBE` paketlerini aynı kanalda ESP-NOW ile göndermeye devam eder. Mesh ile birlikte çalışmazsa robot, rölenin mesh çerçevelerinin RSSI'sini promiscuous modda okur. Hangisinin kullanılacağı ön testte belirlenir; üç protokolde aynı yöntem kullanılır.
- **B planı:** Mesh hiç kararlı çalışmazsa ESP-Mesh-Lite'a geçilir.

## A5. P3 — Ham ESP-NOW zinciri

- Sabit kanal, tanımlı eş MAC adresleri, `esp_now_send()` ve alım callback'leri.
- Röle k, `DATA` paketini **üst düğümüne (k−1)** gönderir; röle 1'in üstü merkezdir.
- `ECHO` paketini **alt düğümüne** gönderir. Alt düğüm, kendisine en son `DATA` gönderen düğümdür (ilk paketle öğrenilir).
- **Uygulama düzeyinde yeniden gönderim yok, atlama yok.** ESP-NOW'un kendi MAC katmanı onayı ve tekrarı açık kalır; bu ayar P3 ve P5'te aynıdır.

## A6. Bekleme modu ve "aktif ol" komutu (üç protokolde ortak)

Tamburdaki röleler açıktır ama **bekleme modundadır**: ağa katılmaz, paket iletmez, yalnızca robottan gelecek `ACTIVATE` komutunu dinler. Bu olmazsa ESP-WIFI-MESH'te röleler daha robotun üstündeyken ağa katılır ve topolojiyi bozar.

**`ACTIVATE` yükü (8 bayt):**

| Bayt | Alan | Açıklama |
|---:|---|---|
| 0 | `k` | Bu rölenin sıra numarası (1–5) |
| 1 | `parent` | Üst düğüm kimliği (k−1; k=1 ise 0 = merkez) |
| 2 | `grandparent` | Bir üstteki düğüm (k−2; yoksa 0xFF) |
| 3 | `proto` | 1 = P1, 3 = P3, 5 = P5 |
| 4 | `channel` | Kanal |
| 5 | `R` | SBZ yeniden gönderim sayısı |
| 6 | `N` | SBZ atlama eşiği |
| 7 | rezerv | 0 |

**Akış:** Robot `ACTIVATE` gönderir → röle `ACT_ACK` döner → röle protokolü başlatır. `ACT_ACK` 200 ms içinde gelmezse komut en fazla 5 kez tekrarlanır. Hiç gelmezse olay kaydedilir, robot yine devam eder.

Aşama 1B'deki "yeni röle açılır" olayı aynı mekanizmayı bir zamanlayıcıyla tetikler.

## A7. P5 — SBZ (Sıra Bilen Zincir)

P3'ün üzerine üç özellik ekler.

### A7.1. Keşifsiz bağlanma

Robot röleyi bıraktığı anda ona üst düğümünü (k−1) ve bir üstünü (k−2) `ACTIVATE` içinde bildirir. Röle ağ keşfi, kök seçimi ya da ebeveyn araması yapmaz; ilk paketi hemen iletebilir. Robot da kendi üst düğümünü k olarak günceller.

### A7.2. Atlama başına sınırlı yeniden gönderim

ESP-NOW gönderim callback'i başarısız dönerse (MAC katmanı tekrarlarına rağmen onay gelmediyse) paket uygulama düzeyinde **en fazla R kez** daha gönderilir.

### A7.3. Atlama (bypass)

Üst düğüme **art arda N** paket (tekrarlar dahil) başarısız olursa röle **ATLAMA** durumuna geçer ve paketleri k−2'ye gönderir. Her T_dene süresinde bir paket eski üst düğüme denenir; başarılı olursa **NORMAL** duruma dönülür.

### A7.4. Röle durum makinesi

```text
            açılış
              │
              ▼
        ┌───────────┐   ACTIVATE (proto=5)   ┌──────────┐
        │  BEKLEME  │ ─────────────────────► │  NORMAL  │ ◄─────────────┐
        └───────────┘                        └──────────┘               │
                                                  │ üst düğüme art arda │ T_dene'de bir
                                                  │ N başarısız         │ eski üste deneme
                                                  ▼                     │ başarılı
                                             ┌──────────┐               │
                                             │  ATLAMA  │ ──────────────┘
                                             │ (k−2'ye) │
                                             └──────────┘
```

### A7.5. Sözde kod (röle, P5)

```text
iletim(paket):
    paket.hop += 1; paket.ttl -= 1
    eğer paket.ttl == 0: at
    hedef = (durum == NORMAL) ? üst : üstün_üstü
    eğer durum == ATLAMA ve şimdi - son_deneme > T_dene:
        hedef = üst; son_deneme = şimdi
    tekrar R+1 kez:
        gönder(hedef, paket)
        eğer başarılı:
            eğer hedef == üst: ardışık_hata = 0; durum = NORMAL
            dön
        paket.flags |= TEKRAR
    eğer hedef == üst:
        ardışık_hata += 1
        eğer ardışık_hata >= N ve üstün_üstü varsa: durum = ATLAMA
```

### A7.6. Parametreler

| Parametre | Başlangıç değeri | Not |
|---|---|---|
| R (yeniden gönderim) | 2 | Çok büyük olursa gecikme artar |
| N (atlama eşiği) | 3 | Çok küçük olursa anlık kayıpta gereksiz atlama yapılır |
| T_dene | 1 s | Eski üst düğümün yeniden denenme aralığı |
| Tekrar eleme tamponu | Kaynak başına son 64 `seq` | |

## A8. Çıkarılan ve yedek yöntemler

- **Kendi SoftAP+STA/UDP zinciri çıkarıldı.** Gerekçesi [kararlar.md](kararlar.md)'de.
- **ESP-Mesh-Lite:** P1 çalışmazsa B planı (ESP32-C3 desteği ESP-IDF v5.0+).
- **ESP-NOW yayma (flooding), Mesh-Lite karşılaştırması:** Gelecek çalışma.

---

# Bölüm B — Bırakma kuralları

## B1. Genel bakış

| Kod | Kural | Ne zaman bırakır? | Başlangıç değeri |
|---|---|---|---|
| **S1** | Sabit mesafe | Son bırakmadan beri **X metre** gidilince | X ≈ 12 m |
| **S2** | RSSI eşiği | 1 s ortalama RSSI **3 s boyunca eşiğin altında** kalırsa | Eşik ≈ −75 dBm |
| **S3** | Eğilim | Yumuşatılmış RSSI'nin eğimine göre eşiğe ulaşma süresi **T'den kısa** ise ya da paket kaybı artıyorsa | Pencere 3–5 s, T ≈ 3 s |
| **S★** | İdeal yerleşim | Koşu değil; Aşama 0 verisinden hesaplanır | — |

## B2. Ortak kurallar

- **RSSI kaynağı:** Son bırakılan rölenin robota gönderdiği `PROBE` paketleri (10 Hz). Robot, alınan çerçevenin RSSI'sini (ESP-NOW alım bilgisi) kaydeder. Üç protokolde **aynı yöntem** kullanılır (A4'e bakın).
- İlk röle bırakılmadan önce RSSI merkezden gelen yoklamalardan ölçülür.
- **Asgari mesafe:** İki bırakma arasında en az `d_min` (başlangıç 3 m) olmalı. Art arda bırakmayı önler.
- **Tek ölçümle karar verilmez.** S2 ve S3 pencere kullanır.
- **En fazla 5 röle.** Röleler bittikten sonra bağlantı koparsa koşu başarısız sayılır, ulaşılan son nokta kaydedilir.
- Kural her **100 ms**'de bir değerlendirilir.

## B3. S1 — Sabit mesafe

```text
her 100 ms:
    eğer (yol - son_bırakma_yolu) >= X: BIRAK
```

**X nasıl seçilir?** Ana karşılaştırmada **tek X** kullanılır: pilotta O1'de sinyalin eşiğe düştüğü mesafenin biraz altında bir değer. O2'de ek kıyas olarak ortama göre ayarlanmış bir X de denenebilir (ana analize girmez). Sabit mesafenin neden şart olduğu: [kararlar.md](kararlar.md).

## B4. S2 — RSSI eşiği

```text
her 100 ms:
    r1 = son 1 s'deki PROBE RSSI'lerinin ortalaması (paket yoksa −100 kabul)
    eğer r1 < eşik: altında_süre += 0,1 s; değilse: altında_süre = 0
    eğer altında_süre >= 3 s ve (yol - son_bırakma_yolu) >= d_min: BIRAK
```

## B5. S3 — Eğilim

```text
her 100 ms:
    r_ewma = α · r_son + (1 − α) · r_ewma            (α başlangıç 0,2)
    eğim = son W saniyedeki r_ewma'ya doğrusal regresyonun eğimi  (dBm/s)
    eğer eğim < 0: t_eşik = (r_ewma − eşik) / (−eğim)  değilse: t_eşik = ∞
    kayıp = son 1 s'de beklenen ama gelmeyen PROBE oranı
    eğer (t_eşik < T veya kayıp > kayıp_eşiği veya r_ewma < eşik)
         ve (yol - son_bırakma_yolu) >= d_min: BIRAK
```

| Parametre | Başlangıç değeri |
|---|---|
| α | 0,2 |
| W (eğim penceresi) | 3–5 s |
| T | 3 s |
| kayıp_eşiği | %30 |

Son satırdaki `r_ewma < eşik` koşulu bir güvenlik ağıdır: eğilim yakalanamasa bile S3, S2'den daha geç bırakmaz.

## B6. S★ — İdeal yerleşim

Aşama 0'da her ortamda **açgözlü yerleşim yürüyüşü** yapılır:

1. Bir düğüm başlangıçta sabit durur, ikinci düğüm rota boyunca metre metre uzaklaşır; her noktada 20–30 s RSSI ve teslim oranı ölçülür.
2. Bağlantının hâlâ iyi olduğu (RSSI ≥ eşik + güvenlik payı ve teslim ≥ %95) **en uzak nokta** bulunur; sabit düğüm oraya taşınır.
3. Rota bitene kadar tekrarlanır.

Bu yürüyüşte kullanılan düğüm sayısı ve konumları **S★**'dır: o gün, o ortamda, duraklayarak ölçen biri için rotayı kapsayan en az röle sayısı. Kuralların başarısı "S★'a göre kaç fazla röle kullandı" ile de değerlendirilir.

Aynı yürüyüş, mesafe–RSSI ve RSSI–teslim eğrilerini de verir (Aşama 0'ın diğer çıktısı).
