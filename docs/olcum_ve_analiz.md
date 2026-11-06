# Ölçüm ve Analiz

**Son güncelleme:** 6 Kasım 2026 (sade tasarım, [kararlar.md](kararlar.md) K-42)  
**Sorumlular:** Mustafa (kayıt altyapısı ve betikler), Aleyna ve Hatice (analiz)

## 1. Araştırma soruları ve hipotezler

| | Soru / hipotez | Birincil ölçüt | Test edildiği yer |
|---|---|---|---|
| **AS1** | Bırakılan yeni röle, kendini kuran mesh'te ve bırakma sırasını bilen SBZ'de ne kadar sürede katılır ve bu fark bırakma anındaki kesintiye nasıl yansır? | | |
| **H1** | SBZ'de yeni rölenin ağa katılma süresi ve bırakma anı kesintisi ESP-WIFI-MESH'tekinden kısadır. | Bağlanma süresi (Aşama 1), bırakma anı kesintisi (Aşama 2) | Aşama 1 + 2 |
| **AS2** | Ayarları tek bir kalibrasyon ortamında belirlenen kurallardan hangisi, farklı test ortamlarında daha kısa kesinti ve daha az röleyle çalışır? | | |
| **H2** | RSSI eşiği (S2) ve eğilim (S3), O1'de ve O2'de sabit mesafeden (S1) daha kısa toplam kesinti sağlar. Nedeni: K'ye göre ayarlanan X, ortam değişince röleyi gereğinden geç ya da erken bırakır. | Toplam kesinti süresi | Aşama 2 |
| **AS3** | Kural ile protokol birbirini telafi eder mi? | | |
| **H3** | S3'ün S2'ye göre sağladığı bırakma anı kesintisi azalması, mesh'te (P1) SBZ'dekinden (P2) büyüktür. Nedeni: erken bırakma yavaş katılımı telafi eder. | (S3 − S2) farkının P1 ile P2 arasındaki farkı (etkileşim) | Aşama 2 |

Hipotezler başvuruda ([TYBS metni](tubitak/TYBS_Basvuru_Metinleri.md) 1.1) de aynen bu biçimde yazılı.

## 2. Metrikler

| Grup | Metrik | Tanım | Aşama |
|---|---|---|---|
| **Süreklilik** | **Bırakma anı kesintisi** ★ ana ölçüt | Her bırakma anı t_b için [t_b − 5 s, t_b + 10 s] penceresindeki toplam kesinti süresi; koşu başına ortalama. Pencereler çakışırsa her kesinti saniyesi yalnızca bir kez, en yakın bırakmaya sayılır. | 2 |
| | Bağlanma süresi | t=60 s'den sonra robottan merkeze ulaşan **ilk** paketin zamanı − 60 s. Hiç gelmezse **"katılmadı"** (sansürlü). | 1 |
| | Kesinti sayısı | Merkezde 500 ms'den uzun süre robottan paket gelmeyen aralık sayısı | 1, 2 |
| | Toplam kesinti süresi | Bu aralıkların toplamı (s) | 1, 2 |
| **Teslim** | Paket teslim oranı (PDR) | Merkeze ulaşan benzersiz `seq` sayısı / gönderilen `seq` sayısı | 1, 2 |
| | Goodput | Merkeze ulaşan benzersiz uygulama verisi (bit) / süre (s) → kbit/s | 1, 2 |
| | En yüksek sürdürülebilir hız | PDR ≥ %95 iken taşınan en yüksek yük basamağı. **Yalnızca ortak yükü belirlemek için** (Aşama 0); hipotez testi yok. | 0 |
| **Gecikme** | RTT p50 / p95 | Yankı paketiyle ölçülen gidiş-dönüş süresinin ortancası ve 95. yüzdeliği. **Tek yönlü gecikme iddia edilmez** (kart saatleri eş zamanlı değil). | 1, 2 |
| | Jitter (ikincil) | Ardışık RTT örnekleri arasındaki mutlak farkların ortalaması (RFC 3393 mantığı) | 1, 2 |
| **Yerleştirme** | Kullanılan röle sayısı | Koşuda bırakılan röle sayısı | 2 |
| | Bırakma konumları ve zamanları | Enkoder mesafesi + lazer metre doğrulaması; robot saatine göre zaman | 2 |
| | Görev başarısı | Hedefe bağlantıyla ulaşıldı mı? (evet/hayır) | 2 |
| | Ulaşılan en uzak mesafe | Bağlantının son kez sağlandığı nokta (m) | 2 |
| | Röle verimliliği | Ulaşılan mesafe / kullanılan röle | 2 |
| **Ağ içi (ikincil)** | Yeniden gönderim sayısı, ebeveyn değişimi, gerçekleşen topoloji | Sonuçları açıklamak için | 1, 2 |

**Kararlı durum:** Aşama 1'de Röle 5'in katılmasından 360 s'ye kadar. Aşama 2'de bütün koşu.

**Bırakma anının zamanı:** Robot ağ kartı bırakma olayını kendi saatine göre kaydeder. Merkezin saatiyle eşleştirmek için bırakma olayı telemetriyle (`DATA` yükünün ilk 16 baytı) merkeze de gönderilir; bağlantı koptuğu için ulaşmazsa robot flaş kaydındaki zaman, son başarılı yankıdan hesaplanan saat farkıyla merkeze çevrilir.

## 3. Kayıt altyapısı

### 3.1. Nerede ne kaydedilir?

| Kaynak | Ne? | Nasıl? |
|---|---|---|
| **Merkez** | Gelen her paket | Seri port → bilgisayardaki `kosu.py` → `paketler.csv` |
| **Robot ağ kartı** | RTT, RSSI, mesafe, bırakma olayları | (1) `DATA` yükünün ilk 16 baytında telemetri olarak merkeze gönderilir; (2) bağlantı koptuğunda veri kaybolmasın diye robot kartının flaş belleğine de yazılır, koşu sonunda USB ile alınır |
| **Röleler** | Yeniden gönderim ve kayıp sayaçları | Periyodik özet paketiyle merkeze; ikincil |
| **Elle** | Deney günlüğü, lazer metre ölçümleri | [deney_plani.md](deney_plani.md) 10. bölümdeki şablon |

**Robot telemetrisi (DATA yükünün ilk 16 baytı):** son RTT (u16, ms), son RSSI (i8), toplam yol (u32, mm), bırakılan röle sayısı (u8), kural durumu (u8), son bırakma yolu (u32, mm), rezerv (3 B). Kalan 144 bayt sentetik doldurma verisidir.

### 3.2. Klasör yapısı

```text
veri/
  ham/
    <run_id>/
      paketler.csv        merkezde paket düzeyi kayıt
      robot.csv           robot flaşından alınan olay ve telemetri kaydı
      meta.json           koşu bilgileri (aşama, ortam, blok, protokol, kural, tekrar, tarih, yazılım sürümleri, PHY hızı)
  gunluk/
    YYYY-AA-GG.md         deney günlükleri
  ozet/
    kosular.csv           her koşu için bir satır (betikle üretilir)
    birakmalar.csv        her bırakma için bir satır (betikle üretilir)
```

Ham veri **hiçbir zaman elle düzeltilmez**. Hatalı bir koşu silinmez; `meta.json`'da `"gecersiz": true` ve gerekçe yazılır.

### 3.3. Paket düzeyi kayıt (`paketler.csv`)

```csv
run_id,t_rx_ms,type,src,seq,hop,ttl,flags,t_src_ms,len,rssi
```

`t_rx_ms`: merkezin paketi aldığı andaki yerel zamanı. `rssi`: merkezin bu paketi son atlamadan aldığı sinyal gücü.

### 3.4. Koşu özeti (`kosular.csv`)

```csv
run_id,tarih,asama,ortam,blok,protokol,kural,tekrar,yuk_kbps,phy,gonderilen,alinan_benzersiz,pdr,goodput_kbps,rtt_p50_ms,rtt_p95_ms,jitter_ms,kesinti_sayisi,kesinti_toplam_s,birakma_ani_kesinti_s,baglanma_s,katildi,role_sayisi,basari,ulasilan_m,gecersiz,notlar
```

### 3.5. Bırakma özeti (`birakmalar.csv`)

```csv
run_id,k,t_birakma_ms,yol_m,lazer_m,rssi_birakma,act_ack_ms,ilk_paket_ms,pencere_kesinti_s
```

`ilk_paket_ms`: bırakılan rölenin üzerinden geçen ilk paketin merkeze ulaşma zamanı (`hop` ve yol kaydından). Bırakma anında rölenin ne kadar sürede katıldığını gösterir.

## 4. Analiz

**Araçlar:** Doğrusal karma modeller (log dönüştürülmüş süreler) için Python `statsmodels` `MixedLM`. Poisson ve binom karma modeller için R `glmmTMB` (veya `lme4`); `statsmodels` bu modelleri frekansçı olarak desteklemez. Hangisinin kullanılacağı İP4'te Aşama 1 verisiyle sınanarak kesinleşir. Analiz betikleri depoda tutulur; aynı ham veriden aynı sonuç çıkmalıdır.

### 4.1. Modeller

| Aşama | Model | Dağılım / bağlantı | Rastgele etki |
|---|---|---|---|
| 1 | Bağlanma süresi, PDR, RTT ~ protokol × ortam | Süre: log-normal · PDR: binom (alınan/gönderilen) | gün |
| 2 | Bırakma anı kesintisi, toplam kesinti ~ protokol × kural × ortam | log(süre + 0,1 s) normal | blok (gün) |
| 2 | Kesinti sayısı, röle sayısı ~ protokol × kural × ortam | Poisson | blok (gün) |
| 2 | Görev başarısı ~ protokol × kural × ortam | Binom (logit) | blok (gün) |

- **H1:** Protokol ana etkisi (Aşama 1 bağlanma süresi ve Aşama 2 bırakma anı kesintisi).
- **H2:** Kural kontrastları S2 − S1 ve S3 − S1, toplam kesinti üzerinde, her ortam ayrı raporlanır.
- **H3:** Etkileşim kontrastı (S3 − S2)ₚ₁ − (S3 − S2)ₚ₂, bırakma anı kesintisi üzerinde. Robotlu koşullarda 8 tekrar bu test için seçildi.
- **Çoklu karşılaştırma:** Birincil ölçütlerdeki karşılaştırmalara (H1'in iki ölçütü, H2'nin iki kontrastı, H3) **Holm düzeltmesi** uygulanır. Diğer karşılaştırmalar keşif amaçlı raporlanır.
- **Her sonuç p değeriyle birlikte etki büyüklüğü ve %95 güven aralığıyla raporlanır.** Log ölçekli modellerde etki "oran" olarak verilir (ör. "SBZ'de bırakma anı kesintisi mesh'in 0,4 katı, %95 GA 0,3–0,6").
- **Sansürlü değerler:** "Katılmadı" olan bağlanma süreleri iki parça raporlanır: katılan koşu oranı ve katılanlarda süre dağılımı.
- Koşu sırası blok içinde rastgeledir.

### 4.2. Ana grafikler

1. **Bırakma anı zaman çizgisi:** Örnek koşularda RSSI, PDR ve bırakma anları aynı zaman ekseninde; P1 ve P2 yan yana. Ana fikri tek bakışta gösteren grafik.
2. **Etkileşim grafiği:** Ortam başına bir panel; x = kural, y = bırakma anı kesintisi; her protokol için bir çizgi. H3 burada görünür.
3. **Kural karşılaştırması:** Ortam başına kural × toplam kesinti ve kural × röle sayısı.
4. **Aşama 1 kutu grafikleri:** Bağlanma süresi, protokol × ortam.

### 4.3. Raporlamada dikkat

- Sonuçlar "kamera yükü benzetimi" ile sınırlıdır; video protokolünün kendi ek yükü modellenmemiştir.
- Router durumu, kanal, verici gücü, PHY hızı, anten ve ortam koşulları her sonuçla birlikte belirtilir.
- P1–P2 farkı tek bir özelliğe bağlanmaz; iki yaklaşımın toplam farkıdır.
- Arıza sonrası toparlanma ve kapasite–atlama eğrisi kapsam dışıdır; bunlar hakkında sonuç yazılmaz.
- Protokoller ve kurallar yalnızca RSSI'ye göre değil; kesinti, teslim, goodput, gecikme ve röle sayısıyla birlikte değerlendirilir.
