# Sistemin Büyük Resmi

**Son güncelleme:** 6 Kasım 2026

## Parçalar

Sistem üç parçadan oluşur: **merkez**, **röleler** ve **bırakıcı robot**. Ağ tarafındaki bütün uçlarda aynı kart vardır (XIAO ESP32-C3, aynı anten). Böylece karşılaştırmayı donanım farkı bozmaz.

```text
                 Wi-Fi üzerinden çok atlamalı zincir (en fazla 6 atlama)

 [Merkez] <───> [Röle 1] <───> [Röle 2] <───> ... <───> [Röle 5] <───> [Robot]
   │              │                                        │             │
 USB ile PC     LiPo pil                                 LiPo pil      Robot pili
   │
 Kayıt + analiz
```

| Parça | Adet | Ne yapar? |
|---|---:|---|
| **Merkez** | 1 kart | Zincirin başı. Bilgisayara USB ile bağlıdır. Gelen her paketi zaman damgasıyla kaydeder, yankı paketlerini geri gönderir. ESP-WIFI-MESH'te sabit köktür. |
| **Röle** | 5 kart | Pil ve kutu içinde. Robotun tamburunda **bekleme modunda** durur; bırakılınca "aktif ol" komutuyla ağa katılır ve paketleri iletir. |
| **Robot ağ kartı** | 1 kart | Rölelerle aynı kart. Kamera yerine geçen sentetik veriyi üretir, son rölenin sinyalini izler, **bırakma kararını verir**. |
| **Robot sürüş kartı** | 1 kart | ESP32-S3; Wi-Fi ve Bluetooth'u hiç açılmaz. Motorları, enkoderleri, kumandayı ve bırakma tablasını yönetir. |
| **Yedek** | 1 kart | Arıza ve geliştirme için. |

## Veri akışı

```text
 Robot ağ kartı                    Röleler                       Merkez
 ──────────────                    ───────                       ──────
 Sentetik veri (160 B paket) ──►  bir sonrakine ilet  ──►  ...  ──►  kaydet (seq, zaman)
                                                                      │
 RTT hesapla  ◄──────────────  ...  ◄── geri ilet  ◄────────────  yankı (seçili paketler)

 Son röle ── yoklama paketi (10 Hz) ──► Robot: RSSI'yi ölç ──► bırakma kuralı
```

- **Yukarı yön (robot → merkez):** Ölçülen asıl trafik budur. Kamera akışını temsil eder.
- **Aşağı yön (merkez → robot):** Yalnızca küçük yankı paketleri gider. Bunlarla gidiş-dönüş süresi ölçülür.
- **Yoklama:** Son bırakılan röle robota düzenli olarak küçük paketler gönderir. Robot bu paketlerin sinyal gücünü (RSSI) ölçer ve bırakma kuralına verir.

## Robotun içi

```text
 [Kablolu joystick] ──kablo (~3 m)──► [Sürüş kartı: ESP32-S3, radyo kapalı]
                                        │  DRV8833 ──► 2 × enkoderli mikro DC motor (2WD diferansiyel)
                                        │  motor enkoderleri (kat edilen yol)
                                        │  TMC2209 ──► NEMA 17 ──► 6 bölmeli döner tabla
                                        │
                                      UART   (mesafe ──►   ◄── BIRAK n)
                                        │
                                      [Robot ağ kartı: XIAO ESP32-C3]
                                         sentetik trafik + bırakma kuralı
```

**Neden iki kart?** Robot ucundaki radyonun rölelerle birebir aynı olması gerekiyor; bu yüzden ağ işini XIAO ESP32-C3 yapar, S3'ün radyosu hiç açılmaz. Ayrıca ESP32-C3 tek çekirdekli; motor kontrolü aynı karta konursa ağ ölçümlerinin zamanlamasını bozabilir.

**Robot:** İki tekerlekli diferansiyel sürüş (sol ve sağ enkoderli mikro DC motor, arkada sarhoş teker), 3D baskı şasi, 3S Li-ion pil. Ayrıntı: [donanim.md](donanim.md).

**Neden kablolu kumanda?** 2,4 GHz uzaktan kumanda, ölçtüğümüz Wi-Fi kanalına girişim yapar.

## Bir bırakmanın akışı

```text
 Robot ağ kartı          Sürüş kartı           Tabla             Bırakılan röle (k)
 ──────────────          ───────────           ─────             ──────────────────
 kural "bırak" der
   │ BIRAK k ───────────► robotu durdur
   │                      tablayı 60° döndür ──► röle düşer
   │ ◄──── TAMAM, mesafe ─┘
   │ "aktif ol" (k, üst = k−1) ────────────────────────────────────► protokolü başlat
   │ ◄─────────────────────────────────────────────────────────────  aktif oldum
   │ bırakma konumunu ve zamanını kaydet
   │ DEVAM ─────────────► robot yoluna devam eder
                          (rölenin ağa katılmasını beklemez)
```

## Ağ yaklaşımları ve bırakma kuralları (özet)

| | Seçenekler | Ayrıntı |
|---|---|---|
| **Ağ yaklaşımı** | P1 ESP-WIFI-MESH · P2 SBZ | [protokoller_ve_stratejiler.md](protokoller_ve_stratejiler.md) |
| **Bırakma kuralı** | S1 sabit mesafe · S2 RSSI eşiği · S3 eğilim (ayarlar yalnızca K'de) | [protokoller_ve_stratejiler.md](protokoller_ve_stratejiler.md) |
| **Ortam** | K düz koridor (yalnızca kalibrasyon) · O1 L koridor · O2 beton bodrum | [deney_plani.md](deney_plani.md) |

## Kapsam dışı

- Wi-Fi dışındaki radyolar (Wi-Fi 6, Zigbee, Thread, BLE, LoRa).
- Gerçek kamera, video kodlama ve görüntü işleme. Kameranın yükünü sentetik trafik temsil eder.
- Otonom sürüş ve haritalama. Robotun kendi verdiği tek karar bırakma kararıdır.
- Enerji tüketimi ölçümü.
- 5'ten fazla röle, şifreleme, kablosuz güncelleme (OTA).
