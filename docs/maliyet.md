# Malzeme ve Maliyet

**Son güncelleme:** 6 Kasım 2026 · **Fiyat tarihi:** 4 Kasım 2026, KDV dâhil  
**Sorumlu:** Aleyna (satın alma takibi)  
**Durum:** Hiçbir şey satın alınmadı. Fiyatlar alım öncesi yeniden kontrol edilmeli.

## 1. Özet

| Grup | Toplam |
|---|---:|
| A. Ağ düğümleri | 4.276,61 TL |
| B. Bırakıcı robot (2WD) | 3.506,96 TL |
| C. Bırakma mekanizması | 1.536,27 TL |
| D. Ölçüm altyapısı | 2.332,05 TL |
| E. Sarf | 348,11 TL |
| **Toplam** | **12.000,00 TL** |

- **Düğüm başına maliyet:** kart 369,52 + pil 237,00 + anahtar 14,45 + kutu filamenti ~30 ≈ **650 TL**.
- **Tahmini satırlar** (satıcı fiyatı olmayanlar) toplamın yaklaşık 1.900 TL'si: router, TMC2209, joystick, rulman, sarf.
- **Kargo bütçede ayrı satır değil.** Siparişleri az sayıda satıcıda birleştirerek düşük tutun; birkaç satırdaki yüksek tahmin pay bırakıyor.

## 2. Malzeme listesi

### A. Ağ düğümleri

| # | Ürün | Birim | Adet | Toplam | Not |
|---|---|---:|---:|---:|---|
| 1 | [Seeed XIAO ESP32-C3](https://www.e-komponent.com/113991054-xiao-esp32c3-wifi-b-seeed-studio) (anten dâhil) | 369,52 | 8 | 2.956,16 | Merkez, 5 röle, robot, 1 yedek. Yurt dışı tedarik, 7–10 iş günü |
| 2 | [3,7 V 1000 mAh korumalı LiPo (PX603453)](https://www.robolinkmarket.com/power-xtra-px603453-37v-1000-mah-li-polymer-pil-devreli) | 237,00 | 5 | 1.185,00 | Her röleye bir pil |
| 3 | [Sürgülü aç/kapa anahtarı](https://www.robotistan.com/lilypad-slide-switch-anahtar) | 14,45 | 5 | 72,25 | |
| 4 | [USB-A → USB-C veri kablosu](https://www.robotistan.com/type-c-to-usb-data-transfer-kablosu) | 63,20 | 1 | 63,20 | Merkez için |

### B. Bırakıcı robot (2WD diferansiyel)

| # | Ürün | Birim | Adet | Toplam | Not |
|---|---|---:|---:|---:|---|
| 5 | [NovaMax 6 V 250 rpm enkoderli mikro DC motor](https://www.robotzade.com/urun/novamax-enkoderli-6v-250rpm-mikro-dc-motor) | 612,00 | 2 | 1.224,00 | Kilitlenme akımı satıcıdan doğrulanmalı |
| 6 | [N20 kauçuk teker (~43 × 18 mm)](https://www.robo90.com/n20-tekerlek) | 39,00 | 2 | 78,00 | 3 mm D göbek |
| 7 | [ESP32-S3 DevKitC-1 N16R8](https://drmakerelektro.com/urun/esp32-s3-devkitc-1-n16r8-gelistirme-karti-16mb-flash-8mb-psram-harici-anten-girisli) | 800,00 | 1 | 800,00 | Sürüş kartı; Wi-Fi/BT kullanılmaz |
| 8 | [Metal sarhoş teker (~21 mm)](https://www.robotzade.com/urun/metal-sarhos-teker) | 17,00 | 1 | 17,00 | |
| 9 | [DRV8833 motor sürücü](https://www.trmteknoloji.com/urun/drv8833-2-kanal-dc-motor-surucu) | 90,00 | 1 | 90,00 | |
| 10 | [XL4015 5 A düşürücü](https://drmakerelektro.com/urun/xl4015-5a-ayarlanabilir-dc-dc-voltaj-dusurucu) | 149,00 | 1 | 149,00 | Motor hattı, 6,0 V |
| 11 | [LM2596 düşürücü](https://www.robo90.com/lm2596-lityum-batarya-sarj-modulu-dc-dc-voltaj-dusurucu) | 96,00 | 1 | 96,00 | Kartlar, 5,0 V |
| 12 | [ASPİLSAN INR18650A28 2800 mAh](https://www.pilstok.com/aspilsan-18650-a28-2800mah-25a-li-ion-sarjli-pil) | 110,00 | 3 | 330,00 | 3S |
| 13 | [3'lü 18650 pil yuvası, 6 pin](https://www.elektronikport.com/urun/18650-li-on-piller-icin-3-lu-pil-yuvasi) | 74,96 | 1 | 74,96 | Akım sınırı satıcıdan sorulmalı |
| 14 | [3S 10 A balanslı BMS](https://drmakerelektro.com/urun/3s-10a-12v-bms-18650-li-ion-pil-koruma-ve-balans-devresi) | 185,00 | 1 | 185,00 | |
| 15 | [12,6 V / 1 A 3S Li-ion şarj cihazı](https://www.3faz.com/mervesan-126-v-dc-li-ion-batarya-sarj-cihazi-1-a-126-w-li-ion-3s-cozumu) | 297,00 | 1 | 297,00 | |
| 16 | [Sigorta yuvası](https://www.robo90.com/5x20mm-kablolu-cam-sigorta-yuvasi-siyah) + [4 A sigorta](https://www.robo90.com/5x20mm-cam-sigorta-4a) | 16,00 | 1 set | 16,00 | |
| 17 | Joystick modülü + ~3 m kablo + konnektör | ~150,00 | 1 | 150,00 | **Tahmini** |

Şasi 3D baskıyla üretilir (filament C grubunda). Kablo, konnektör, robotun ana anahtarı, lehim ve vidalar atölyeden karşılanır.

### C. Bırakma mekanizması

| # | Ürün | Birim | Adet | Toplam | Not |
|---|---|---:|---:|---:|---|
| 18 | [NEMA 17 step motor (1,7 A)](https://www.robotistan.com/bipolar-nema-17-17a-42byghw609-wantai-step-motor) | 372,30 | 1 | 372,30 | Kısa gövdeli model de olabilir ([alternatif](https://market.samm.com/en/17hs3001-20b-3d-printer-stepper-motor-nema-17-model-en)) |
| 19 | TMC2209 step sürücü | ~300,00 | 1 | 300,00 | **Tahmini** |
| 20 | Rulman (608ZZ) | ~50,00 | 1 | 50,00 | **Tahmini** |
| 21 | [PLA filament 1,75 mm, 1 kg](https://www.robotistan.com/porima-3d-175-mm-pla-filament-red) | 813,97 | 1 | 813,97 | Şasi, tabla, taban, düğüm kutuları. 1 kg sınırda; baskılar hafif tasarlanmalı |

### D. Ölçüm altyapısı

| # | Ürün | Birim | Adet | Toplam | Not |
|---|---|---:|---:|---:|---|
| 22 | 2,4 GHz router (802.11n, 300 Mbps sınıfı) | ~750,00 | 1 | 750,00 | **Tahmini.** ESP-WIFI-MESH router'sız çalışmazsa. **Demirbaş** |
| 23 | [Lazer mesafe ölçer, 40 m](https://www.idefix.com/texta-b40-temel-seviye-lazer-mesafe-olcer-lazermetre-hobi-ve-hafif-isler-icin-p-15981291) | 1.108,05 | 1 | 1.108,05 | Röle konumları, enkoder doğrulaması. **Demirbaş** |
| 24 | [Yedek 1000 mAh LiPo](https://www.robolinkmarket.com/power-xtra-px603453-37v-1000-mah-li-polymer-pil-devreli) | 237,00 | 2 | 474,00 | Koşular arası pil değişimi |

### E. Sarf ve yedek parça

| # | Ürün | Toplam | Not |
|---|---|---:|---|
| 25 | Zemin işaretleme bandı, JST konnektör, kondansatörler, makaron, röle pil kabloları, yedek sigortalar | 348,11 | **Tahmini.** Genel kablo, lehim ve vidalar atölyeden |

## 3. Satın alma sırası

1. **Önce 2 adet XIAO ESP32-C3.** ESP-NOW ve ESP-WIFI-MESH ön testleri (router'sız kök dahil) yapılır.
2. Ön test başarılıysa kalan 6 kart ve 5 pil alınır.
3. Router yalnızca ön testte gerekli çıkarsa alınır.
4. Robot parçaları alınır (motorlar, tekerler, S3, güç). Motor ve teker ölçüleri şasi tasarımına verilir.
5. Step motor, rulman ve filament tasarım netleşince alınır (motor boyu ve rulman tipi tasarıma bağlı).
6. Siparişler birleştirilir (bazı satıcılarda 1.500 TL üzeri kargo ücretsiz).

## 4. Satın alma kuralları

- Fatura **yürütücü veya ortaklardan biri adına** kesilir. İsimsiz fiş geçersizdir.
- Router ve lazer metre **demirbaştır**; proje sonunda üniversite envanterine kaydettirilir.
- Harcanmayan ya da belgelenemeyen para iade edilir. Ayrıntı: [tubitak/](tubitak/).
- Her alım aşağıdaki takip tablosuna aynı gün yazılır, fatura fotoğrafı ortak klasöre konur.

## 5. Satın alma takibi

| # | Kalem | Satıcı | Tarih | Tutar (TL) | Fatura adına | Teslim alan | Şu an nerede | Fatura yüklendi |
|---|---|---|---|---:|---|---|---|---|
| | | | | | | | | |

**Harcanan:** 0,00 TL · **Kalan:** 12.000,00 TL

## 6. Fiyatlar artarsa ne kısılır?

| Seçenek | Tasarruf |
|---|---:|
| Router'a gerek kalmazsa | ~750 TL |
| Lazer metre yerine şerit metre | ~900 TL |
| Yedek LiPo'lardan vazgeçmek | 474 TL |
| Yedek XIAO'dan vazgeçmek | 369,52 TL |
| Elde 3S şarj cihazı veya USB-C kablosu varsa | ~360 TL |

## 7. Satın alınmayacaklar

| Bileşen | Neden |
|---|---|
| TP4056 şarj modülü, 3,3 V regülatör | XIAO kartında var |
| Ek U.FL anten | Kartın paketinde var |
| Hazır plastik kutu | Kutular 3D baskıyla, tablaya uygun ölçüde basılacak |
| Kamera | Kamera yükü yazılımla benzetilecek |
| LoRa, Zigbee, Thread, BLE modülleri | Kapsamda yalnızca Wi-Fi var |
| 2,4 GHz uzaktan kumanda | Ölçülen kanala girişim yapar |
| A4988 / DRV8825 step sürücü | TMC2209 sessiz ve bekleme akımını düşürebiliyor (K-09) |
| 2,4 GHz anten seti (ESP32-S3 için) | S3'ün radyosu kullanılmıyor |
| TT motor, Nano, IR enkoder | 2WD tasarımına geçildi (K-29) |
