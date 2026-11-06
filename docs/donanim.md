# Donanım

**Son güncelleme:** 6 Kasım 2026 · **Sorumlular:** Hacer (robot elektroniği), Mustafa (kutular ve mekanizma)  
**Durum:** Tasarım. Pin atamaları **öneridir**; kartlar gelince Hacer kesinleştirir ve bu dosyayı günceller.

Satın alma listesi ve fiyatlar: [maliyet.md](maliyet.md). Bırakma mekanizması: [birakma_mekanizmasi.md](birakma_mekanizmasi.md).

## 1. Ağ kartı: Seeed Studio XIAO ESP32-C3

| Özellik | Değer | Bizim için anlamı |
|---|---|---|
| Radyo | 2,4 GHz 802.11 b/g/n, tek radyo | Röle aynı kanalda hem alıp hem gönderir. Çok atlamada verim düşer, bu beklenen bir sonuç. |
| İşlemci | RISC-V, tek çekirdek, 160 MHz | Motor kontrolü bu karta konmaz; robotta ayrı sürüş kartı var (3. bölüm). |
| Anten | U.FL konnektörlü harici anten (pakette var) | Tüm kartlarda aynı anten, aynı yönelim. |
| Pil | BAT+/BAT− lehim noktaları, tek hücre LiPo şarj devresi | Ayrı şarj modülü gerekmez. |
| Boyut | 21 × 17,5 mm | Düğüm kutusu küçük tutulabilir. |

Toplam **8 kart**: merkez, 5 röle, robot ağ kartı ve 1 yedek. Hepsi aynı ESP-IDF sürümüyle programlanır ([deney_ayarlari.md](deney_ayarlari.md)).

## 2. Röle düğümü

```text
 ┌──────────────── 3D baskı kutu (~60 × 40 × 15 mm) ─────────────────┐
 │  [LiPo 1000 mAh, 53×34×6]   [anahtar]   [XIAO ESP32-C3]  ~~anten~~ │
 │        BAT+ ──────────────────o/o──────────── BAT+                 │
 │        BAT− ────────────────────────────────── BAT−                │
 └────────────────────────────────────────────── USB-C erişimi ──────┘
```

| Parça | Not |
|---|---|
| 3,7 V 1000 mAh **koruma devreli** LiPo | BAT+/BAT− noktalarına lehimlenir. **Asla 5 V pinine bağlanmaz.** |
| Sürgülü aç/kapa anahtarı | Pilin + hattına seri bağlanır. Not: anahtar kapalıyken USB'den şarj da kesilebilir; şarj sırasında anahtar açık tutulur. |
| Anten | Yaklaşık 4 cm esnek yapışkanlı anten. Kutunun üst iç yüzüne, **her kutuda aynı yöne** yapıştırılır. |
| Kutu | Mustafa tasarlar. Yassı ve düz düşmeye uygun, anahtar dışarıdan erişilebilir. Dolu kütle hedefi 40–60 g. |

**Pil süresi:** Röleler her deney gününden önce tam şarj edilir. Bir koşuya (en fazla ~15 dk) yettiği pilotta doğrulanır. Enerji ölçümü yapılmaz.

## 3. Bırakıcı robot (2WD diferansiyel)

### 3.1. Yapı

```text
            Üstten görünüş (ileri yön ↑)

          ┌───────────────────────┐
     [L]══╡   3D baskı şasi       ╞══[R]     L, R: enkoderli mikro DC motor + N20 teker (43 mm)
          │  ESP32-S3   XIAO      │
          │  pil (3S)   DRV8833   │
          │        ( o )          │          o: sarhoş teker (arka destek)
          ├───────────────────────┤
          │   revolver tabla      │          tabla arkada; bırakma açıklığı
          │       ▼ açıklık       │          sarhoş tekerin gerisinde
          └───────────────────────┘
```

- **Tahrik:** İki enkoderli mikro DC motor (sol, sağ) + arkada bir sarhoş teker. Dönüşler motor hız farkıyla yapılır (diferansiyel sürüş).
- **Şasi:** 3D baskı. Tabla ile birlikte tasarlanır; bu sayede tablanın sığacağı genişlik baştan verilebilir.
- **Mesafe:** Motorların kendi enkoderleri kullanılır; ayrı IR sensörü ya da delikli disk yok.

### 3.2. Parçalar

| Parça | Görev |
|---|---|
| 2 × NovaMax 6 V 250 rpm **enkoderli** mikro DC motor (3 mm D mil) | Sol ve sağ tahrik; kat edilen yol |
| 2 × N20 kauçuk teker (~43 × 18 mm, 3 mm D göbek) | |
| Metal sarhoş teker (~21 mm) | Arka destek |
| DRV8833 çift kanallı motor sürücü | Kanal A sol, kanal B sağ (kanal başına tek motor) |
| **ESP32-S3 DevKitC-1 N16R8** | Sürüş kartı: motorlar, enkoderler, kumanda, tabla. **Wi-Fi ve Bluetooth hiç başlatılmaz.** |
| TMC2209 + NEMA 17 | Bırakma tablası |
| Joystick modülü + ~3 m çok damarlı kablo | Kablolu kumanda |
| 3 × 18650 (ASPİLSAN A28, 2800 mAh), 3S, 6 pinli üçlü pil yuvası | Robot pili (10,8–12,6 V) |
| 3S 10 A balanslı BMS | Koruma ve dengeleme |
| 5 × 20 mm sigorta yuvası + 4 A sigorta | Pil çıkışında |
| XL4015 düşürücü (6,0 V'a ayarlı) | Motor hattı |
| LM2596 düşürücü (5,0 V'a ayarlı) | Kartlar ve sensörler |
| 12,6 V / 1 A 3S Li-ion şarj cihazı | BMS'nin şarj uçlarından şarj |
| XIAO ESP32-C3 | Robot ağ kartı (rölelerle aynı) |

**Neden ESP32-S3 radyosu kullanılmıyor?** Robot ucundaki radyonun rölelerle birebir aynı olması gerekiyor. Bu yüzden ağ işini XIAO ESP32-C3 yapar. S3'te Wi-Fi/BT sürücüsü hiç başlatılmadığı sürece kart yayın yapmaz ve ölçüme karışmaz. S3 çift çekirdekli olduğu için motor ve enkoder işleri rahatça yürür. Ayrıntı: [kararlar.md](kararlar.md) K-29.

### 3.3. Güç şeması

```text
 3 × 18650 (3S, 10,8–12,6 V) ── 6 pinli yuva ── 3S BMS (B−, B1, B2, B+)
                                                    │
                                     4 A sigorta ── ana anahtar
                                                    │
                     ┌──────────────────────────────┼──────────────────────────┐
                     ▼                              ▼                          ▼
              XL4015 → 6,0 V                   TMC2209 VM                 LM2596 → 5,0 V
                     │                              │                          │
              DRV8833 VM                        NEMA 17               ┌────────┴────────┐
                     │                                                ▼                 ▼
              sol / sağ motor                                   ESP32-S3 5V pini   XIAO 5V pini
                                                                    │
                                                                 3V3 çıkışı → enkoderler, joystick
 Tüm topraklar ortak. Şarj: 12,6 V şarj cihazı → BMS'nin şarj uçları.
```

| Kural | Neden |
|---|---|
| Regülatörler bağlantıdan **önce** ayarlanır: XL4015 6,0 V, LM2596 5,0 V | Ayarsız çıkış kartları ya da motorları yakabilir. |
| BMS'nin **B1 ve B2** ara bağlantıları da yapılır | Yalnızca dıştaki artı ve eksi, balanslı 3S BMS için yetmez. |
| Aynı model, yeni ve benzer doluluktaki 3 hücre | Dengesiz hücreler şarjda tehlikelidir. |
| Pil yuvasının akım sınırı satıcıdan öğrenilir; hücreler titreşime karşı sabitlenir | Ucuz yaylı yuvalarda temas kaybı ve ısınma olabilir. |
| **Yumuşak hızlanma** | NovaMax'in kilitlenme akımı ~2 A; DRV8833 kanal başına ~1,5 A sürekli verir. Kesin değer satıcıdan doğrulanır. |
| Motor uçlarına **100 nF** seramik kondansatör | Fırça gürültüsü kartları resetleyebilir. |
| TMC2209 akımı (Vref) motor etiketinin **altında** | Tabla hafif; düşük akım yeter, motor ısınmaz. |
| **3,3 V lojik:** ESP32-S3 pinleri 5 V'a dayanıklı değildir | Enkoderler 3,3 V ile beslenir (destekliyorsa; değilse seviye dönüştürücü). |
| Tabla hizalanmadan düğüm yüklenmez | Ev konumu sensörü yok. Tabla, hizalama işaretiyle **elle** boş bölme açıklıkta olacak şekilde ayarlanır; sürüş kartı açıldığı andaki konumu sıfır kabul eder ([birakma_mekanizmasi.md](birakma_mekanizmasi.md) 5. bölüm). |

### 3.4. Sürüş kartı pin önerisi (ESP32-S3 DevKitC-1 N16R8)

Kaçınılan pinler: GPIO 0, 3, 45, 46 (açılış ayarı), 19–20 (USB), 26–37 (flash ve sekizli PSRAM), 43–44 (USB-seri köprüsü).

| GPIO | Bağlantı | Not |
|---|---|---|
| 4, 5 | DRV8833 AIN1, AIN2 (sol motor) | LEDC PWM |
| 6, 7 | DRV8833 BIN1, BIN2 (sağ motor) | LEDC PWM |
| 15, 16 | Sol enkoder A, B | PCNT (donanım sayıcı, dörtlü kod çözme) |
| 8, 14 | Sağ enkoder A, B | PCNT |
| 9 | TMC2209 STEP | |
| 10 | TMC2209 DIR | |
| 11 | TMC2209 EN | Aktif düşük |
| 1, 2 | Joystick X, Y | ADC1 |
| 13 | Joystick düğmesi | Acil dur olarak kullanılabilir |
| 17 (TX), 18 (RX) | XIAO ile UART1 | XIAO D7 (RX) ← 17, XIAO D6 (TX) → 18 |

### 3.5. Enkoder kalibrasyonu

NovaMax'in tur başına darbe sayısı ve teker çapı tam bilinmediği için mesafe katsayısı ölçülür. Robot zemin işaretli 5 m'lik düz yolda 5 kez sürülür; ortalama darbe sayısından **mm/darbe** hesaplanır ve [deney_ayarlari.md](deney_ayarlari.md)'ye yazılır. Sol ve sağ enkoderin ortalaması yol olarak kullanılır.

Hız kontrolü: 250 rpm'lik motor ve 43 mm'lik tekerle yüksüz hız ~0,56 m/s'dir. Hedef hız 0,3 m/s; enkoder geri beslemeli basit bir PI kontrolüyle sabit tutulur.

### 3.6. Sürüş kartı ile ağ kartı arasındaki UART

| Konu | Karar |
|---|---|
| **Gerilim seviyesi** | İki kart da 3,3 V; doğrudan bağlanır, dönüştürücü gerekmez. Topraklar ortak. |
| Hız | 115200 baud |
| Biçim | Satır tabanlı metin, `\n` ile biter |

| Yön | Mesaj | Anlamı |
|---|---|---|
| Sürüş → Ağ | `D <mm>` (10 Hz) | Son bırakmadan beri değil, **koşu başından beri** toplam yol (mm). Kural farkı ağ kartında hesaplanır. |
| Ağ → Sürüş | `B <k>` | k numaralı röleyi bırak (dur, tablayı k. konuma döndür) |
| Sürüş → Ağ | `K <k> <mm>` | k bırakıldı, bırakma anındaki toplam yol |
| Ağ → Sürüş | `G` | Devam et (röle aktif oldu ya da zaman aşımı) |
| Sürüş → Ağ | `H` | Sürüş kartı açıldı, tablanın şu anki konumu sıfır kabul edildi, sistem hazır |
| Her iki yön | `E <kod>` | Hata (ör. motor sürücü yanıt vermiyor) |

## 4. Merkez

- XIAO ESP32-C3, dizüstü bilgisayara USB ile bağlı. Gelen paketleri seri port üzerinden bilgisayara aktarır.
- Bilgisayarda kayıt betiği çalışır ([olcum_ve_analiz.md](olcum_ve_analiz.md)).
- **Router:** ESP-WIFI-MESH router olmadan çalışmazsa deneye özel 2,4 GHz router merkezin yanına konur. Kullanılırsa **üç protokolün bütün koşularında açık** tutulur (adillik için).

## 5. Ölçüm araçları

| Araç | Kullanım |
|---|---|
| Lazer mesafe ölçer (40 m) | Aşama 1 röle konumları, rota uzunlukları, enkoder doğrulaması |
| Zemin işaretleme bandı | Röle konumları, başlangıç ve bitiş çizgileri, rota üzerindeki her 5 m |
| Yedek 2 LiPo | Koşular arasında pil değişimi |
