# Röle Düğümü Bırakma Mekanizması

**Son güncelleme:** 6 Kasım 2026 · **Sürüm:** 2.0 (revolver tipi döner tabla)  
**Sorumlular:** Mustafa (3D tasarım ve baskı), Hacer (elektronik, montaj, kabul testi)  
**Durum:** Tasarım kararı verildi; CAD çizimi yapılacak.

## 1. Tasarım

Robotun arkasına, NEMA 17 step motorla dönen, revolver tipi **6 bölmeli** bir döner tabla yerleştirilir. Bölmelerin 5'i röle düğümlerini taşır, 1'i başlangıçta boştur.

Tablanın altında **sabit bir taban** vardır. Tabanda yalnızca bir bölmeye denk gelen tek bir bırakma açıklığı bulunur. Başlangıçta boş bölme bu açıklığın üstündedir. Robot bırakma kararı verince durur, tabla **60°** döner ve sıradaki düğüm açıklıktan zemine düşer. Bu işlem beş kez tekrarlanabilir.

Mekanizma 3D baskıyla üretilir, tek motorludur ve hareketli kapak gerektirmez. Tabla bir rulman üzerinde döner; böylece ağırlık doğrudan motor miline binmez. Düğümlerin yatay ve hep aynı yönde düşmesi hedeflenir.

```text
Üstten görünüş                         Yandan görünüş

        ┌─────┐                         ┌──[NEMA 17]──┐
     ┌──┤  1  ├──┐                      │   ▼ mil     │
     │ 6└─────┘2 │                  ┌───┴─────────────┴───┐
     │  ┌─────┐  │                  │ döner tabla (6 bölme)│ ← rulman üzerinde döner
     │ 5│  ●  │3 │                  ├──────────┬──────────┤
     │  └─────┘  │                  │  sabit   │ açıklık  │ ← sabit taban
     └──┤  4  ├──┘                  └──────────┘          │
        └─────┘                                  düğüm ↓ düz düşer
   Başlangıç: boş bölme açıklığın üstünde   ═══════════════════ zemin (3–5 cm)
   Her bırakma: tabla 60° döner
```

**Neden bu tasarım?**

- Tek motor, tek hareketli parça. Kapak, mandal ya da yay yok.
- Düğüm sabit tabanın üstünde kayarak açıklığa gelir ve düz düşer; anten yönelimi her bırakmada aynı kalır.
- Rulman ağırlığı taşır; motor yalnızca tablayı döndürür.

Alternatiflerin neden seçilmediği: [kararlar.md](kararlar.md).

## 2. Zorunlu istekler

| Konu | İstek | Neden? |
|---|---|---|
| Düz düşüş | Düğüm **yatay kalmalı, büyük yüzü aşağı bakacak şekilde** dönmeden ve devrilmeden düşmeli. Açıklık her yanda düğümden 2–3 mm büyük olmalı. | Antenin her bırakmada aynı yöne bakması ölçümlerin adil olması için şart. |
| Konum | Bırakma açıklığı robotun **en arka temas noktasının (sarhoş teker) gerisinde** olmalı. Robot bırakmadan sonra yerinde dönmeden önce en az ~20 cm ilerler. | Düğüm tekerleklerin önüne düşerse robot üstünden geçer. Diferansiyel sürüşte yerinde dönüş, robotun arkasını yana süpürür ve yeni bırakılan düğüme çarpabilir. |
| Hizalama işareti | Tablanın kenarında ve sabit tabanda karşılıklı birer **çentik/işaret** bulunmalı. İki işaret üst üste gelince boş bölme tam açıklığın üstünde olmalı. Tabla, motor akımsızken elle rahatça çevrilebilmeli. | Ev konumu sensörü yok. Tabla her koşudan önce elle bu işarete getirilir. |
| Metal yok | Düğüm bölmelerinin yakınında metal olmamalı. Motor, mil ve rulman merkezde kalabilir. | Metal Wi-Fi sinyalini bozar. |
| Kendiliğinden düşme yok | Robot kapı eşiği gibi küçük bir basamaktan (~1 cm) geçerken ve ani dururken düğümler yerinden çıkmamalı. | Bırakma yalnızca kuralın kararıyla olmalı. 43 mm'lik tekerlerle daha yüksek bir eşik zaten aşılamaz. |

## 3. Ölçüler ve parçalar

| Konu | Bilgi |
|---|---|
| Düğümün içindekiler | Pil **53 × 34 × 6 mm**, kart **21 × 17,5 mm**, ~4 cm esnek anten, sürgülü anahtar |
| Düğüm kutusu | Küçük ve yassı; hedef **~60 × 40 × 15 mm**. Açılıp kapanabilir; anahtar dışarıdan erişilebilir, USB-C'ye erişim olursa iyi olur. |
| Düğüm kütlesi | Dolu hâlde ~40–60 g (kutu basılınca tartılacak) |
| Motor | NEMA 17 (42 × 42 mm, 200 adım/tur). Ağırlığı azaltmak için kısa gövdeli ("pancake", ~20–25 mm) model değerlendirilebilir. |
| Rulman | Merkezde 608ZZ (8 × 22 × 7 mm) ya da ince döner tabla rulmanı |
| Zemin yüksekliği | Düğümün alt yüzü ile zemin arası **3–5 cm** (düşüş yüksekliği; şasi alçak olduğu için tabla şasinin üstünde durur) |
| Malzeme | PLA |
| Montaj | Şasi de 3D baskı olduğu için mekanizma ile şasi **birlikte tasarlanır**; tabla şasiye vidayla ya da geçmeli bağlanır. Motor ve sensör kabloları gövdeye doğru çıkar. |

## 4. Tasarımda dikkat edilecekler

1. **Tabla çapı.** 60 × 40 mm'lik 6 düğüm yatay yerleşince tabla çapı ~20 cm'ye çıkabilir. Şasi 3D baskı olduğu için genişliği tablaya göre verilebilir; tablanın arkaya taşması da kabul edilebilir. Düğüm kutusu küçüldükçe tabla da küçülür; kutu, tabla ve şasi birlikte tasarlanmalı.
2. **Yazıcının baskı alanı.** Baskı alanı 220 × 220 mm'den küçükse tabla parçalı tasarlanır.
3. **Ağırlık ve denge.** Tahrik iki **mikro** motorla yapılıyor; robotu hafif tutmak önemli. Tabla, NEMA 17, 5 düğüm ve 3S pille robot ~1–1,3 kg olabilir. **Kısa gövdeli (pancake) NEMA 17 güçlü şekilde önerilir.** Ağırlık merkezi tahrik tekerleklerinin üstüne yakın olmalı: tabla tamamen arkada kalırsa yük sarhoş tekere biner, tahrik tekerleri kayar. Pil ve kartlar öne konarak denge sağlanabilir. 5 düğüm bırakıldıkça arka hafifler; boş tablayla da denge kontrol edilmeli.
4. **Kayma sürtünmesi.** Düğümler tabanın üstünde kayarak döner. Taban yüzeyi pürüzsüz olmalı, düğüm kenarlarına pah kırılmalı ki açıklığın kenarına takılmasınlar.

## 5. Elektronik ve yazılım

- **Sürücü:** TMC2209, doğrudan 3S pilden (10,8–12,6 V) beslenir. Sessiz çalışır ve bekleme akımını kendisi düşürebilir.
- **Bağlantı:** Sürüş kartında STEP, DIR ve EN için 3 pin ([donanim.md](donanim.md) 3.4).
- **Başlangıç konumu (elle):** Ev konumu sensörü yoktur. Her koşudan önce robot kapalıyken tabla elle döndürülür ve hizalama işaretleri üst üste getirilir (boş bölme açıklıkta). Sonra düğümler yüklenir ve robot açılır. Sürüş kartı açıldığı andaki konumu sıfır kabul eder ve motoru hemen bekleme akımıyla tutar.
- **60° hesabı:** 200 adımlık motorda 60° = 33,33 adım ediyor; mikro adımla da tam sayı çıkmıyor. Bu yüzden her bırakmada hedef konum **başlangıca göre mutlak olarak** hesaplanır. 1/16 mikro adımda i. bırakmanın hedefi `yuvarla(i × 3200 / 6)` olur. Yuvarlama hatası birikmez; en fazla 1 mikro adım (~0,1°).
- **Bekleme akımı:** Hareket bittikten sonra akım düşürülür ama **asla kesilmez** (TMC2209'un bekleme akımı azaltma özelliği). Sensör olmadığı için tabla bir kez kayarsa yazılım bunu fark edemez.
- **Koşu ortasında güç kesilirse:** Tablanın konumu kaybolur. Robot kapatılır, tabla işarete göre elle yeniden hizalanır, kalan düğümler kontrol edilir ve koşu geçersiz sayılıp tekrarlanır.
- **Akış:** Ağ kartı `B k` gönderir → sürüş kartı robotu durdurur → tabla k. konuma döner → ~1 s beklenir → `K k <mm>` geri bildirilir. Mesaj biçimi: [donanim.md](donanim.md) 3.4.

## 6. Kabul testi

- Her bölmeden art arda **20 bırakma**, toplam 100.
- Bırakmaların **hepsinde** düğüm düz iniyor ve hiç takılma olmuyor.
- 100 bırakma boyunca tabla kaymıyor: her 5 bırakmada (bir tam tur) hizalama işaretlerinin hâlâ üst üste geldiği gözle kontrol edilir.
- Robot ~1 cm'lik bir eşikten geçerken ve ani dururken düğümler yerinden çıkmıyor.

Sonuçlar tarih ve sürüm numarasıyla bu dosyanın sonuna eklenir.

**B planı:** Mekanizma çalışmazsa robot durur, düğüm aynı noktaya elle bırakılır. Karar kuralı ve kayıt değişmez; araştırma sonucu etkilenmez.

## 7. Kabul testi kayıtları

| Tarih | Mekanizma sürümü | Bırakma | Düz iniş | Takılma | İşaret kayması | Not |
|---|---|---:|---:|---:|---|---|
| | | | | | | |
