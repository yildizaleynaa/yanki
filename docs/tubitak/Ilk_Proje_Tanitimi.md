# Uyarlamalı Röle Düğümü Yerleştirme Projesi — Tanıtım

> 23 Ekim 2026 · @Ustad Ceri
>
> *Kaynak: `Uyarlamalı Röle Düğümü Yerleştirme Projesi — Tanıtım.pdf` (6 sayfa) dosyasından Markdown'a çevrilmiştir.*

## Proje Özeti

Bu proje, kapalı ve engelli ortamlarda ilerleyen bir keşif robotunun arkasına bıraktığı düşük maliyetli röle düğümlerinin ne zaman bırakılması gerektiğini araştırır. Üç bırakma stratejisi (sabit mesafe, sinyal gücü eşiği, eğilim tabanlı tahmin) üç farklı kapalı ortamda karşılaştırılır.

Gerçek kamera kullanılmaz; bunun yerine kameranın ağa bindireceği yük, farklı çözünürlük profillerini taklit eden sentetik bir trafik üreteciyle oluşturulur. Böylece deneyler tekrarlanabilir olur ve sonuçlar doğrudan "kaç röleyle hangi görüntü kalitesi taşınabilir" sorusuna cevap verir.

Beklenen çıktılar: stratejilerin veri bütünlüğü açısından karşılaştırmalı analizi, açık kaynak donanım ve yazılım, ve paylaşılabilir bir ölçüm veri seti.

## Problem ve Motivasyon

Enkaz, maden, tünel ve bodrum gibi ortamlarda kablosuz sinyal beton, metal ve dönüşler yüzünden hızla zayıflar. Keşif robotu içeri girdikçe operatörle bağlantısını kaybeder; operatör hem görüntüyü hem de sensör verisini yitirir.

Uluslararası yeraltı robotik yarışmalarında bazı ekipler bu sorunu robotun arkasına ağ düğümleri bırakarak çözmüştür. Ancak bu çözümler pahalı, askeri sınıf telsizlerle geliştirilmiştir. Düşük maliyetli mikrodenetleyicilerle kurulan bir röle zincirinde düğümlerin hangi kurala göre bırakılması gerektiği ve bu zincirin operatörün en çok ihtiyaç duyduğu görüntü akışını kaç düğüme kadar taşıyabileceği yeterince ölçülmemiştir.

Türkiye'nin deprem riski, maden kazaları ve artan tünel ve metro inşaatları bu sorunu yerel olarak da önemli kılmaktadır.

## Amaç, Araştırma Soruları ve Hipotezler

Projenin amacı, düşük maliyetli bir röle zincirinde düğüm bırakma stratejisinin veri bütünlüğüne etkisini ölçmek ve en verimli stratejiyi belirlemektir.

### Araştırma soruları

1. Hangi bırakma stratejisi, en az düğüm kullanarak en kesintisiz bağlantıyı sağlar?
2. Röle sayısı arttıkça zincirin taşıyabildiği en yüksek sürdürülebilir veri hızı nasıl değişir?
3. Farklı görüntü kalitesi profilleri (720p, 480p, 360p eşdeğeri) en fazla kaç röleye kadar kabul edilebilir kayıp ve gecikmeyle taşınabilir?

### Hipotezler

- **H1:** Sinyal kalitesine dayalı stratejiler (eşik ve eğilim), sabit mesafeli bırakmaya göre daha az düğümle daha yüksek bağlantı sürekliliği sağlar.
- **H2:** Eğilim tabanlı strateji, sinyal düşüşünü önceden algıladığı için eşik tabanlı stratejiye göre daha kısa kesintiler üretir.
- **H3:** Sürdürülebilir veri hızı her ek röleyle belirgin biçimde düşer; yüksek kaliteli profil, düşük kaliteli profillere göre daha az röle sayısında sınıra ulaşır.

## Önerilen Sistem

Sistem dört bileşenden oluşur ve tamamı düşük maliyetli, Wi-Fi destekli mikrodenetleyicilerle kurulur. Veri şu zincirden geçer:

```
Robot → Röle 1 → Röle 2 → … → Operatör istasyonu
```

- **Keşif robotu:** Uzaktan kumandayla sürülen tekerlekli bir şase. Üzerindeki mikrodenetleyici enkoderlerle kat edilen mesafeyi ölçer, bağlantı kalitesini izler, seçilen stratejiye göre bırakma kararı verir ve bir servo mekanizmasıyla röle düğümünü zemine bırakır. Aynı zamanda sentetik trafiğin kaynağıdır.
- **Röle düğümleri (6–8 adet):** Pille çalışan, kutulanmış küçük mikrodenetleyici kartları. Gelen paketi çözmeden bir sonraki düğüme iletir ve kendi bağlantı istatistiklerini kaydeder.
- **Operatör istasyonu:** Zincirin son ucundaki alıcı düğüm ve ona bağlı bir bilgisayar. Gelen paketleri doğrular, gidiş-dönüş ölçümü için geri yansıtır ve tüm ölçümleri kaydeder.
- **İzleme arayüzü:** Operatör bilgisayarında çalışan bir web arayüzü. Zincirdeki her bağlantının anlık kalitesini, teslim oranını ve gecikmeyi gösterir; deney sonunda kayıtları dışa aktarır.

**Sentetik trafik üreteci.** Gerçek kamera, sahneye göre değişen bit hızıyla deneye kontrolsüz bir değişken ekleyeceği için kullanılmaz. Bunun yerine robot, kameranın ağa bindireceği yükü taklit eden sabit profilli bir akış üretir; video kodlamadaki periyodik büyük anahtar kareler de düzenli paket patlamalarıyla taklit edilir. Her paket sıra numarası, zaman damgası ve hata denetim kodu (CRC) taşır.

## Yöntem

Deney, **3 strateji × 3 ortam × 4 trafik profili** şeklinde kurulur ve her kombinasyon en az 10 kez tekrarlanır.

### Bırakma stratejileri

| Kod | Strateji | Bırakma kuralı |
|---|---|---|
| S1 | Sabit mesafe | Her X metrede bir düğüm bırakılır (X ön testlerle belirlenir) |
| S2 | Sinyal gücü eşiği | Son düğüme olan sinyal gücü belirli bir eşiğin altına düşünce bırakılır |
| S3 | Eğilim tabanlı tahmin | Sinyal gücünün düşüş eğimi ve paket kaybı birlikte izlenir; eşiğe ulaşılmadan önce bırakılır |

### Test ortamları

- **O1:** Düz koridor
- **O2:** Dönüşlü koridor ve merdiven boşluğu
- **O3:** Betonu yoğun bodrum katı

Hepsi kampüs içinde bulunur.

### Trafik profilleri

| Profil | Temsil ettiği yük | Hedef bit hızı | Paket yapısı |
|---|---|---|---|
| T | Sensör telemetrisi (gaz, sıcaklık, ivme, konum) | Düşük | 32–64 bayt, saniyede 10 paket |
| V1 | 720p eşdeğeri görüntü | ~2 Mbps | Büyük paketler + periyodik patlamalar |
| V2 | 480p eşdeğeri görüntü | ~1 Mbps | Büyük paketler + periyodik patlamalar |
| V3 | 360p eşdeğeri görüntü | ~0,5 Mbps | Büyük paketler + periyodik patlamalar |

Görüntü profillerinin bit hızları ön testlerde gerçek video örneklerinden ölçülerek kesinleştirilecektir. Telemetri tüm deneylerde görüntü profiliyle birlikte gönderilir.

### Ölçülen metrikler

- Paket teslim oranı
- Gidiş-dönüş gecikmesi
- Gecikme değişkenliği (jitter)
- Ardışık kayıp uzunluğu
- Sırası bozulmuş paket oranı
- Bozuk paket oranı (CRC)
- Toplam bağlantı kesinti süresi
- Kullanılan düğüm sayısı

Ayrıca her röle sayısı için zincirin taşıyabildiği en yüksek sürdürülebilir bit hızı, akış hızı kademeli artırılarak bulunur.

Gecikme, uçlardaki saatlerin senkronize edilmesi gerekmesin diye paketlerin geri yansıtılmasıyla gidiş-dönüş olarak ölçülür. Ölçüm saati ve ortamdaki diğer kablosuz ağlar her deneyde kaydedilir.

### Analiz

Stratejiler arasındaki farklar varyans analizi (ANOVA) ve ardından çoklu karşılaştırma testleriyle değerlendirilir; anlamlılık düzeyi 0,05 alınır.

## Kapsam

Projenin araştırma konusu haberleşme stratejisidir; robotun kendisi bu stratejiyi sınamak için bir taşıyıcıdır. Kapsam bilinçli olarak bu soruya odaklanacak şekilde daraltılmıştır.

### Kapsam dışı bırakılanlar

- **Otonom navigasyon:** Robot uzaktan kumandayla sürülür; bırakma kararı dışında otonom davranış yoktur.
- **Gerçek kamera ve video kodlama:** Görüntü yükü sentetik trafikle taklit edilir. Gerçek kamera, sahneye bağlı bit hızı değişimiyle deneylerin tekrarlanabilirliğini bozacaktır.
- **Gerçek enkaz sahası:** Deneyler güvenlik ve tekrarlanabilirlik için kampüs içindeki zorlu kapalı alanlarda yapılır.

## Özgün Değer ve Yaygın Etki

**Özgün değer.** Röle bırakma fikri yüksek bütçeli sistemlerde denenmiştir; bu proje aynı fikri düşük maliyetli donanımla, üç stratejiyi kontrollü ve tekrarlanabilir koşullarda karşılaştırarak sınar. Gerçek kamera yerine profil tabanlı sentetik yük kullanmak, "kaç röleyle hangi görüntü kalitesi taşınabilir" sorusuna ölçülmüş bir cevap verir. Bu boşluk literatür taramasıyla ayrıca doğrulanacaktır.

### Yaygın etki

- **Afet ve arama-kurtarma:** Enkaz altı keşifte operatörle bağlantının sürdürülmesi.
- **Maden, tünel ve metro:** Yeraltı inşaat ve işletmelerinde düşük maliyetli geçici haberleşme.
- **İtfaiye ve bina içi müdahale:** Dumanlı ve kapalı alanlarda sensör verisinin dışarı taşınması.
- **Açık bilim:** Donanım tasarımları, yazılım ve ölçüm veri seti açık kaynak olarak paylaşılır; sonuçların ulusal bir kongrede bildiri olarak sunulması hedeflenir.

## İş Planı ve Riskler

Proje 12 ayda altı iş paketiyle tamamlanır; donanım ve yazılım işleri paralel yürür, deneyler 7.–10. aylarda yapılır.

### İş paketleri · 12 ay

*(PDF'teki zaman çizelgesi grafiğinden aktarılmıştır: deneyler 7.–10. aylarda, analiz ve rapor 10.–12. aylarda.)*

| İş paketi | İçerik | Aylar |
|---|---|---|
| İP1 | Literatür taraması ve sistem mimarisi (tasarım) | 1–2 |
| İP2 | Röle düğümü donanımı ve pil ömrü testleri | 2–5 |
| İP3 | Robot ve bırakma mekanizması | 3–6 |
| İP4 | Web izleme arayüzü | 5–7 |
| İP5 | 3 × 3 × 4 deney matrisi | 7–10 |
| İP6 | İstatistiksel analiz, sonuç raporu ve açık kaynak yayımı | 10–12 |

```
Ay            1  2  3  4  5  6  7  8  9  10 11 12
İP1 Literatür ██ ██
İP2 Düğümler     ██ ██ ██ ██
İP3 Robot           ██ ██ ██ ██
İP4 Arayüz                ██ ██ ██
İP5 Deneyler                    ██ ██ ██ ██
İP6 Analiz                               ██ ██ ██
```

### Riskler ve B planları

| Risk | Olasılık | Etki | B planı |
|---|---|---|---|
| Ölçümlerde yüksek değişkenlik | Yüksek | Orta | Tekrar sayısı artırılır; ölçüm saati ve ortamdaki ağlar kaydedilir |
| Kampüs ortamlarında sinyal beklenenden güçlü çıkar | Orta | Orta | Verici gücü yazılımdan düşürülerek zincir yapay olarak uzatılır |
| Bırakma mekanizması takılır | Orta | Düşük | Düğümler aynı kurala göre elle bırakılır; karşılaştırma etkilenmez |
| Robot arızası | Düşük | Orta | Ölçüm düzeneği elle itilen bir arabaya taşınır |
| Pil ömrü deneye yetmez | Düşük | Düşük | Yedek piller kullanılır; düğüm başına tüketim İP2'de ölçülür |
