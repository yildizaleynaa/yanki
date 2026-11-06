# Ağ Protokolleri ve Bırakma Kuralları

**Son güncelleme:** 6 Kasım 2026 (sade tasarım, [kararlar.md](kararlar.md) K-42)  
**Sorumlular:** Aleyna (SBZ), Hatice (ESP-WIFI-MESH ve bırakma kuralları)  
**Durum:** Tasarım. "Başlangıç değeri" yazan sayılar kalibrasyonda kesinleşir ve [deney_ayarlari.md](deney_ayarlari.md)'ye yazılır.

> **Eski numaralar:** Bu belgenin önceki sürümünde P1 ESP-WIFI-MESH, P3 ham ESP-NOW zinciri ve P5 SBZ vardı. Ham ESP-NOW zinciri ve SBZ'nin düğüm atlama özelliği çıkarıldı; SBZ artık **P2**.

---

# Bölüm A — Ağ protokolleri

## A1. Genel bakış

İki yaklaşım aynı radyoyu kullanır; farkları ağ kurma ve veri aktarma yöntemindedir. İki kod tabanı yazılır: ESP-WIFI-MESH entegrasyonu ve ESP-NOW üzerinde kendi kodumuz.

| Kod | Protokol | Rolü | Kodu |
|---|---|---|---|
| **P1** | ESP-WIFI-MESH | Hazır, kendini kuran ve onaran mesh. Yeni düğüm ağa katılmadan önce aday ebeveynleri tarar. | Entegrasyon ve ayar |
| **P2** | **SBZ (Sıra Bilen Zincir)** | Bizim protokolümüz. Robot bırakma sırasını bildirir, tarama yapılmaz. | Asıl geliştirme |

**Karşılaştırmanın anlamı:** P1 ile P2 keşif, ağaç yönetimi ve protokol yükü bakımından birden çok yönde farklıdır. Bu yüzden ölçülen şey tek bir özelliğin değil, "hazır ve kendini kuran çözüm" ile "bırakma sırasını bilen zincir" arasındaki **toplam farktır**. Başvuruda da böyle yazılıyor; tek bir özelliği ayrıştırdığımızı iddia etmiyoruz.

**Projenin ana sorusu bırakma anıdır:** Yeni röle ağa ne kadar hızlı katılırsa bırakma anındaki kesinti o kadar kısa olur. Hipotezler: [olcum_ve_analiz.md](olcum_ve_analiz.md).

## A2. Ortak kurallar (iki protokolde aynı)

| Konu | Kural |
|---|---|
| Kanal | Tek sabit kanal (1, 6 veya 11; kalibrasyonda en az kullanılan seçilir) |
| Verici gücü | Tüm kartlarda aynı; gerekirse kalibrasyonda eşit olarak düşürülür |
| **PHY hızı** | İki protokolde aynı değere sabitlenir. ESP-NOW'un varsayılanı 1 Mbps'tir; hız her eş için `esp_now_set_peer_rate_config()` ile ayarlanır (`esp_wifi_start()` ve `esp_now_add_peer()` sonrasında). Mesh'te `esp_wifi_config_80211_tx_rate()` İP1'de denenir. Mesh trafiğine uygulanamazsa iki protokolün radyosu aynı kipe (ör. yalnızca 802.11b, `esp_wifi_set_protocol`) sınırlanır ve kalan fark sınırlılık olarak raporlanır ([kararlar.md](kararlar.md) K-43). |
| Uygulama verisi | **160 bayt**: ilk 16 bayt robot telemetrisi (son RTT, RSSI, yol, bırakma sayısı; bkz. [olcum_ve_analiz.md](olcum_ve_analiz.md) 3.1), kalanı kamera yükünü temsil eden sentetik doldurma |
| Paket başlığı | Bölüm A3'teki ortak başlık. P1'de mesh yükünün içine konur. |
| Tekrarlı paket | Merkezde `(src, seq)` ile elenir |
| Bekleme modu | Tamburdaki röle ağa katılmaz; "aktif ol" komutunu bekler (A6) |
| Düğüm kimliği | 0 = merkez, 1–5 = röleler, 6 = robot ağ kartı, 7 = yedek. 8 kartın MAC adresleri ortak bir yapılandırma tablosunda tutulur. |

## A3. Ortak paket başlığı (P2'de doğrudan, P1'de yük içinde)

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
| 11 | `flags` | u8 | bit0: uygulama düzeyinde yeniden gönderildi · diğer bitler rezerv |
| 12–15 | `t_src` | u32 | Kaynağın gönderim anındaki yerel zamanı (ms). RTT için yankıda geri döner. |

| `type` | Ad | Yön | İçerik |
|---|---|---|---|
| 0x01 | `DATA` | robot → merkez | 160 B sentetik veri |
| 0x02 | `ECHO` | merkez → robot | Başlık + 0 B; `seq` ve `t_src` yansıtılır |
| 0x03 | `PROBE` | son röle → robot | Yoklama; yalnızca RSSI ölçümü için |
| 0x04 | `ACTIVATE` | robot → bırakılan röle | "Aktif ol" komutu (A6) |
| 0x05 | `ACT_ACK` | röle → robot | Aktif oldum |
| 0x06 | `EVENT` | merkez → röleler | Aşama 1'de koşu başlangıcı; röleler zamanlayıcılarını buna göre kurar |

**Yankı oranı:** Her 10 `DATA` paketinden birine yankı verilir (100 paket/s'de 10 yankı/s). Aşağı yön trafiği böylece küçük kalır.

## A4. P1 — ESP-WIFI-MESH

- ESP-IDF `esp_mesh_*` arayüzleri kullanılır. Merkez **sabit kök** olarak atanır (`esp_mesh_fix_root`), diğer düğümler kök seçimine katılmaz.
- **Router sorunu:** ESP-WIFI-MESH normalde kökün bir router'a bağlanmasını bekler. Router'sız sabit kök çalışması İP1'deki ön testte denenir. Çalışmazsa deneye özel router merkezin yanına konur ve **iki protokolün bütün koşularında açık** tutulur. Veri router'dan geçmez; merkez veriyi USB ile bilgisayara aktarır.
- Ebeveynini mesh kendisi seçer. Gerçekleşen ebeveyn ilişkileri ve değişimleri kaydedilir. Mesh'in kısa yol olarak bir röleyi atlaması meşru bir protokol davranışıdır; engellenmez, kaydedilir.
- **Bekleme modundan geçiş:** Röle bekleme modunda yalnızca ESP-NOW ile `ACTIVATE` dinler. Komut gelince ESP-NOW'u kapatıp mesh'i başlatır. Bu geçişin süresi bağlanma süresine dahildir; mesh'in gerçek görevde de ödeyeceği bir bedeldir. Geçişin sorunsuz çalıştığı ön testte doğrulanır.
- **Yoklama:** Son röle `PROBE` paketlerini aynı kanalda ESP-NOW ile göndermeye devam eder. Mesh ile birlikte çalışmazsa robot, rölenin mesh çerçevelerinin RSSI'sini promiscuous modda okur. Hangisinin kullanılacağı ön testte belirlenir; iki protokolde aynı yöntem kullanılır.
- **B planı:** Mesh hiç kararlı çalışmazsa ESP-Mesh-Lite'a geçilir.

## A5. P2 — SBZ (Sıra Bilen Zincir)

ESP-NOW üzerinde, bırakma sırasını bilen sade bir zincir. İki özelliğe dayanır.

### A5.1. Keşifsiz bağlanma

- Sabit kanal, tanımlı eş MAC adresleri, `esp_now_send()` ve alım callback'leri kullanılır.
- Robot röleyi bıraktığı anda ona üst düğümünü (k−1) `ACTIVATE` içinde bildirir. Röle ağ keşfi, kök seçimi ya da ebeveyn araması yapmaz; ilk paketi hemen iletebilir. Robot da kendi üst düğümünü k olarak günceller.
- Röle k, `DATA` paketini **üst düğümüne (k−1)** gönderir; röle 1'in üstü merkezdir.
- `ECHO` paketini **alt düğümüne** gönderir. Alt düğüm, kendisine en son `DATA` gönderen düğümdür (ilk paketle öğrenilir).

### A5.2. Atlama başına sınırlı yeniden gönderim

ESP-NOW gönderim callback'i başarısız dönerse, yani gönderim MAC katmanında teslim edilemediyse, paket uygulama düzeyinde **en fazla R kez** daha gönderilir. ESP-NOW belgesi uygulama katmanına ulaşmayı garanti etmez ve gerekirse uygulama düzeyinde yeniden gönderimi önerir.

### A5.3. Röle durum makinesi

```text
          açılış
            │
            ▼
      ┌───────────┐   ACTIVATE (proto=2)   ┌──────────┐
      │  BEKLEME  │ ─────────────────────► │  NORMAL  │  (üst = k−1'e ilet)
      └───────────┘                        └──────────┘
```

Düğüm atlama (k−2'ye geçiş) yoktur. Bir röle bozulursa zincir o noktada kopar. Bu durum kapsam dışıdır; gelecek çalışma olarak raporlanır.

### A5.4. Sözde kod (röle, P2)

```text
iletim(paket):
    paket.hop += 1; paket.ttl -= 1
    eğer paket.ttl == 0: at
    tekrar R+1 kez:
        gönder(üst, paket)
        eğer başarılı: dön
        paket.flags |= TEKRAR
    kayıp_sayacı += 1
```

### A5.5. Parametreler

| Parametre | Başlangıç değeri | Not |
|---|---|---|
| R (yeniden gönderim) | 2 | Çok büyük olursa gecikme ve kanal yükü artar |
| Tekrar eleme tamponu | Kaynak başına son 64 `seq` | |

## A6. Bekleme modu ve "aktif ol" komutu (iki protokolde ortak)

Tamburdaki röleler açıktır ama **bekleme modundadır**: ağa katılmaz, paket iletmez, yalnızca robottan gelecek `ACTIVATE` komutunu dinler. Bu olmazsa ESP-WIFI-MESH'te röleler daha robotun üstündeyken ağa katılır ve topolojiyi bozar.

**`ACTIVATE` yükü (8 bayt):**

| Bayt | Alan | Açıklama |
|---:|---|---|
| 0 | `k` | Bu rölenin sıra numarası (1–5) |
| 1 | `parent` | Üst düğüm kimliği (k−1; k=1 ise 0 = merkez) |
| 2 | `proto` | 1 = P1 (mesh), 2 = P2 (SBZ) |
| 3 | `channel` | Kanal |
| 4 | `R` | SBZ yeniden gönderim sayısı |
| 5–7 | rezerv | 0 |

**Akış:** Robot `ACTIVATE` gönderir → röle `ACT_ACK` döner → röle protokolü başlatır. `ACT_ACK` 200 ms içinde gelmezse komut en fazla 5 kez tekrarlanır. Hiç gelmezse olay kaydedilir, robot yine devam eder.

`ACT_ACK` yalnızca komutun alındığını gösterir; rölenin ağa katıldığını göstermez. **Robot `ACT_ACK`'ten sonra rölenin ağa katılmasını beklemeden yoluna devam eder.** Bırakma anı kesintisi bu yüzden anlamlı bir ölçüttür.

Aşama 1'deki "beşinci röle aktif olur" olayı aynı mekanizmayı bir zamanlayıcıyla tetikler.

## A7. Çıkarılan ve yedek yöntemler

- **Ham ESP-NOW zinciri (eski P3) çıkarıldı** (K-42). SBZ'den düğüm atlama da çıkınca ikisi arasındaki tek fark yeniden gönderim kalıyordu.
- **SBZ'nin düğüm atlama (k−2) özelliği çıkarıldı** (K-42). Arıza sonrası toparlanma kapsam dışı; gelecek çalışma.
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

**Ortam bilinmeden ayar:** Üç kuralın **bütün** ayarları (X, eşik, T ve diğerleri) yalnızca kalibrasyon ortamı **K**'de (düz koridor) belirlenir. Test ortamları O1 ve O2'de değiştirilmez. Böylece ortamın önceden bilinmediği gerçek görev taklit edilir. Yalnızca X'i değil, bütün kuralların ayarını K'de yapmak karşılaştırmayı adil tutar.

## B2. Ortak kurallar

- **RSSI kaynağı:** Son bırakılan rölenin robota gönderdiği `PROBE` paketleri (10 Hz). Robot, alınan çerçevenin RSSI'sini (ESP-NOW alım bilgisi) kaydeder. İki protokolde **aynı yöntem** kullanılır (A4'e bakın).
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

**X nasıl seçilir?** **Tek X** kullanılır: K'de sinyalin eşiğe düştüğü mesafenin biraz altında bir değer. Test ortamlarına göre ayrı X denenmez. H2'nin mantığı da bu: K'ye göre ayarlanan X, köşeli koridorda geç, açık alanda erken bırakabilir. Sabit mesafenin neden şart olduğu: [kararlar.md](kararlar.md).

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

**H3'ün mantığı:** S3 röleyi sınıra gelmeden, erken bırakır. Erken bırakılan röle, robot menzilden çıkmadan ağa katılmak için daha fazla zaman bulur. Bu, yavaş katılan mesh'te (P1) hızlı katılan SBZ'den (P2) daha çok fark yaratmalıdır.

## B6. Kalibrasyon yürüyüşü (K ortamı)

Aşama 0'da yalnızca K'de yapılır (2 tur):

1. Bir düğüm başlangıçta sabit durur, ikinci düğüm zemin seviyesinde rota boyunca metre metre uzaklaşır; her noktada 20–30 s RSSI ve teslim oranı ölçülür.
2. Çıktılar: mesafe–RSSI eğrisi, RSSI–teslim eğrisi ve teslimin birden düştüğü "uçurum" noktası.
3. Eşik, X ve T bu eğrilerden [deney_ayarlari.md](deney_ayarlari.md)'deki kurallarla seçilir.

Test ortamlarında (O1, O2) yalnızca rotanın tek bağlantıyla kapsanamadığı kontrol edilir. Bu ortamlarda kural ayarı yapılmaz.

S★ (ideal yerleşim) hesabı çıkarıldı (K-34, K-42): tek referanslı yürüyüş, rota üzerindeki iki nokta arasındaki bağlantıyı vermiyordu.
