# Ölçüm ve Analiz

**Son güncelleme:** 6 Kasım 2026  
**Sorumlular:** Mustafa (kayıt altyapısı ve betikler), Aleyna ve Hatice (analiz)

## 1. Hipotezler

| | Hipotez | Test edildiği yer |
|---|---|---|
| **H1** | SBZ'nin bağlanma ve toparlanma süreleri ESP-WIFI-MESH'ten kısadır. | Aşama 1B |
| **H2** | Atlama sayısı arttıkça en yüksek sürdürülebilir hız tüm protokollerde belirgin biçimde düşer (tek radyo, aynı kanal). | Aşama 1A |
| **H3** | Uyarlamalı kurallar (S2, S3), sabit mesafeye göre O2 ve O3'te daha az röleyle daha az kesinti sağlar; O1'de fark küçüktür. | Aşama 2 |
| **H4** | Eğilim kuralı (S3), eşik kuralına (S2) göre daha kısa kesintiler üretir. | Aşama 2 |
| *Keşif* | SBZ ile eğilim kuralının birlikte sağladığı kazanç, ikisinin ayrı ayrı getirdiklerinin toplamından büyüktür. | Aşama 2 (etkileşim) |

## 2. Metrikler

| Grup | Metrik | Tanım | Aşama |
|---|---|---|---|
| **Teslim** | Paket teslim oranı (PDR) | Merkeze ulaşan benzersiz `seq` sayısı / gönderilen `seq` sayısı | 1, 2 |
| | Goodput | Merkeze ulaşan benzersiz uygulama verisi (bit) / süre (s) → kbit/s | 1, 2 |
| | En yüksek sürdürülebilir hız | PDR ≥ %95 iken taşınan en yüksek yük basamağı | 1A |
| **Gecikme** | RTT p50 / p95 | Yankı paketiyle ölçülen gidiş-dönüş süresinin ortancası ve 95. yüzdeliği. **Tek yönlü gecikme iddia edilmez** (kart saatleri eş zamanlı değil). | 1, 2 |
| | Jitter (ikincil) | Ardışık RTT örnekleri arasındaki mutlak farkların ortalaması (RFC 3393 mantığı) | 1, 2 |
| **Süreklilik** | Kesinti sayısı | Merkezde 500 ms'den uzun süre robottan paket gelmeyen aralık sayısı | 1, 2 |
| | Toplam kesinti süresi | Bu aralıkların toplamı (s) | 1, 2 |
| | Bağlanma süresi | t=60 s'den sonra merkeze robottan ulaşan **ilk** paketin zamanı − 60 s | 1B |
| | Toparlanma süresi | t=360 s'den sonra akışın yeniden başladığı an − 360 s. "Yeniden başladı" = ardışık 1 s'lik pencerede PDR ≥ %50. Hiç başlamazsa **"toparlanmadı"** (sansürlü). | 1B |
| | Geri katılma süresi | t=480 s'den sonra paketlerin yeniden Röle 3 üzerinden geçmeye başladığı an − 480 s (hop sayısı ve yol kaydından) | 1B |
| **Yerleştirme** | Kullanılan röle sayısı | Koşuda bırakılan röle sayısı | 2 |
| | Bırakma konumları | Enkoder mesafesi + lazer metre doğrulaması | 2 |
| | Görev başarısı | Hedefe bağlantıyla ulaşıldı mı? (evet/hayır) | 2 |
| | Ulaşılan en uzak mesafe | Bağlantının son kez sağlandığı nokta (m) | 2 |
| | Röle verimliliği | Ulaşılan mesafe / kullanılan röle | 2 |
| | İdeale göre fazla röle | Kullanılan röle − S★ | 2 |
| **Ağ içi (ikincil)** | Yeniden gönderim, atlama kullanımı, ebeveyn değişimi, gerçekleşen topoloji | Sonuçları açıklamak için | 1, 2 |

**Kararlı durum:** 1B'de 60–360 s arası. Aşama 2'de bütün koşu.

## 3. Kayıt altyapısı

### 3.1. Nerede ne kaydedilir?

| Kaynak | Ne? | Nasıl? |
|---|---|---|
| **Merkez** | Gelen her paket | Seri port → bilgisayardaki `kosu.py` → `paketler.csv` |
| **Robot ağ kartı** | RTT, RSSI, mesafe, bırakma olayları | (1) `DATA` yükünün ilk 16 baytında telemetri olarak merkeze gönderilir; (2) bağlantı koptuğunda veri kaybolmasın diye robot kartının flaş belleğine de yazılır, koşu sonunda USB ile alınır |
| **Röleler** | Durum değişimleri (NORMAL/ATLAMA), tekrar sayaçları | Periyodik özet paketiyle merkeze; ikincil |
| **Elle** | Deney günlüğü, lazer metre ölçümleri | [deney_plani.md](deney_plani.md) 10. bölümdeki şablon |

**Robot telemetrisi (DATA yükünün ilk 16 baytı):** son RTT (u16, ms), son RSSI (i8), toplam yol (u32, mm), bırakılan röle sayısı (u8), kural durumu (u8), son bırakma yolu (u32, mm), rezerv (3 B). Kalan 144 bayt sentetik doldurma verisidir.

### 3.2. Klasör yapısı

```text
veri/
  ham/
    <run_id>/
      paketler.csv        merkezde paket düzeyi kayıt
      robot.csv           robot flaşından alınan olay ve telemetri kaydı
      meta.json           koşu bilgileri (aşama, ortam, protokol, kural, tekrar, tarih, yazılım sürümleri)
  gunluk/
    YYYY-AA-GG.md         deney günlükleri
  ozet/
    kosular.csv           her koşu için bir satır (betikle üretilir)
```

Ham veri **hiçbir zaman elle düzeltilmez**. Hatalı bir koşu silinmez; `meta.json`'da `"gecersiz": true` ve gerekçe yazılır.

### 3.3. Paket düzeyi kayıt (`paketler.csv`)

```csv
run_id,t_rx_ms,type,src,seq,hop,ttl,flags,t_src_ms,len,rssi
```

`t_rx_ms`: merkezin paketi aldığı andaki yerel zamanı. `rssi`: merkezin bu paketi son atlamadan aldığı sinyal gücü.

### 3.4. Koşu özeti (`kosular.csv`)

```csv
run_id,tarih,asama,ortam,protokol,strateji,tekrar,yuk_kbps,gonderilen,alinan_benzersiz,pdr,goodput_kbps,rtt_p50_ms,rtt_p95_ms,jitter_ms,kesinti_sayisi,kesinti_toplam_s,baglanma_s,toparlanma_s,toparlandi,geri_katilma_s,maks_hiz_kbps,role_sayisi,s_yildiz,basari,ulasilan_m,gecersiz,notlar
```

## 4. Analiz

**Araçlar:** Python (pandas, statsmodels, scipy). Analiz betikleri depoda tutulur; aynı ham veriden aynı sonuç çıkmalıdır.

### 4.1. Modeller

| Aşama | Model | Rastgele etki |
|---|---|---|
| 1A | En yüksek sürdürülebilir hız ~ protokol × atlama sayısı | gün |
| 1B | Metrik ~ protokol × ortam | gün |
| 2 | Metrik ~ protokol × kural × ortam | gün |

- Koşular farklı günlerde yapıldığı için **gün**, karma etkili modelde rastgele etki olarak ele alınır.
- Koşu sırası gün içinde rastgeledir.
- Normal dağılmayan metriklerde (kesinti süresi, toparlanma süresi gibi) **parametrik olmayan testler** kullanılır: Kruskal–Wallis, ardından Holm düzeltmeli ikili karşılaştırmalar.
- **Her sonuç p değeriyle birlikte etki büyüklüğü ve %95 güven aralığıyla raporlanır.** Örneğin ortanca farkı (Hodges–Lehmann) için bootstrap güven aralığı ya da Cliff'in deltası.
- **Toparlanma süresi** sansürlü olabilir (P3'te ya da atlamanın mümkün olmadığı durumlarda). İki parça raporlanır: toparlanan koşu oranı ve toparlananlarda süre dağılımı.

### 4.2. Ana grafikler

1. **Kapasite eğrisi:** x = atlama sayısı, y = en yüksek sürdürülebilir hız; her protokol için bir çizgi, 360p/480p/720p yatay çizgileri.
2. **1B kutu grafikleri:** Bağlanma ve toparlanma süreleri, protokol × ortam.
3. **Etkileşim grafiği:** Ortam başına bir panel; x = kural, y = toplam kesinti süresi (ve röle sayısı); her protokol için bir çizgi.
4. **İdeale uzaklık:** Kural başına "kullanılan − S★" dağılımı.
5. **Örnek koşu zaman çizgisi:** RSSI, PDR ve bırakma anları aynı zaman ekseninde.

### 4.3. Raporlamada dikkat

- Sonuçlar "kamera yükü benzetimi" ile sınırlıdır; video protokolünün kendi ek yükü modellenmemiştir.
- Router durumu, kanal, verici gücü, anten ve ortam koşulları her sonuçla birlikte belirtilir.
- Protokoller ve kurallar yalnızca RSSI'ye göre değil; teslim, goodput, gecikme, kesinti, toparlanma ve röle sayısıyla birlikte değerlendirilir.
