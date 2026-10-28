# 🇹🇷 Şafak Vakti: İstanbul Semalarında / Bayrak Rüzgarı
## Unreal Engine 5 Kapsamlı Blueprint & Teknik Geliştirme Rehberi

Bu belge, oyunun **Unreal Engine 5** üzerinde profesyonel, akıcı ve bağımlılık yapıcı bir oyun hissiyatına (game feel) sahip olması için gereken tüm teknik Blueprint şemalarını, materyal ayarlarını ve optimizasyon ipuçlarını içerir.

---

## 1. Karakter Kontrolü ve Akıcı Fizik Mantığı (`BP_BayrakKarakter`)

Flappy tarzı oyunlarda oyuncunun en çok dikkat ettiği şey **"ağırlık hissi"** ve **"zıplama tepkisidir"**. Bayrağın sadece yukarı aşağı kayması yerine, zıpladığında burnunu yukarı dikmesi, düşerken ise süzülerek aşağı eğilmesi gerekir.

### A. Karakter Ayarları (Character Movement Component)
* **Gravity Scale:** `1.8` - `2.2` (Daha keskin ve tatmin edici düşüş)
* **Jump Z Velocity:** `0` (Varsayılan zıplama yerine doğrudan `Launch Character` kullanacağız)
* **Air Control:** `1.0`
* **Constrain to Plane:** `True` (Side-scroller mantığı için X ekseninde sabit kalması amacıyla: **Plane Constraint Normal = (0, 1, 0)** yani Y ekseninde hareketi kilitliyoruz).

### B. Zıplama Blueprint Grafiği (Event Graph)
```
[Input Action: Space / Tık]
       │
       ▼
[Launch Character (Launch Velocity Z: +650, XY Override: False, Z Override: True)]
       │
       ├─► [Play Sound 2D (Rüzgar / Kanat Sesi)]
       └─► [Spawn Emitter At Location (Hafif Rüzgar İzi Partikülü)]
```

### C. Açı Dönüş Enterpolasyonu (Pitch Rotation on Tick)
Bayrağın dikey hızına (`Velocity Z`) göre eğilmesini sağlamak için:
1. `Event Tick` düğümünü çekin.
2. `Get Velocity` -> `Break Vector` -> **Z ekseni** değerini alın.
3. `Map Range Clamped` düğümüne bağlayın:
   * **In Range A / B:** `-800.0` / `600.0`
   * **Out Range A / B:** `-45.0` (Aşağı bakış açısı) / `35.0` (Yukarı şahlanış açısı)
4. Çıkan sonucu `Make Rotator (Pitch)` içine verin.
5. `RInterp To` (Current: `GetActorRotation`, Target: Oluşturulan Rotator, DeltaTime: `GetWorldDeltaSeconds`, InterpSpeed: `8.0`) düğümüyle yumuşatın ve `SetActorRotation` düğümüne aktarın.

---

## 2. Dalgalanan Türk Bayrağı Materyali (World Position Offset - WPO)

Ağır fiziksel kumaş simülasyonları yerine mobil ve arcade performansı için en temiz yöntem **Materyal Tabanlı Sinüs Dalgasıdır**. Bu sayede bayrak direğe yapışık kalırken ucu gerçekçi şekilde rüzgarda dalgalanır.

### Materyal Grafiği (M_DalgalananBayrak)
1. **Shading Model:** `Default Lit` veya `Unlit` (Arcade canlılığı için Unlit tavsiye edilir).
2. **Base Color:** Al zemin üzerine beyaz Hilal ve Yıldız dokusu (`T_TurkBayragi`).
3. **World Position Offset (WPO) Ağı:**
   * `TexCoord (UV)` düğümünü çağırın -> `Component Mask (R)` ile yatay UV eksenini alın (0=direk tarafı, 1=dalgalanan uç).
   * Bir `Power` düğümüne bağlayarak (`Exponent = 1.5`) direk tarafının hiç oynamamasını sağlayın (Maske).
   * `Time` düğümünü `Scalar Parameter (RuzgarHizi = 6.0)` ile çarpın.
   * `Absolute World Position`'ın X/Y değerlerini ekleyip bir `Sine` düğümüne bağlayın.
   * `Sine` sonucunu maske ve `Scalar Parameter (DalgaSiddeti = 15.0)` ile çarpın.
   * Bir `Make Float3 (0, 0, Z)` veya Normal vektörü ile çarparak **World Position Offset** pinine bağlayın.

---

## 3. Sonsuz Engel ve Toplanabilir Üretici (`BP_SonsuzUretici`)

Sonsuz koşu mekaniğinde harita durur, engeller sola doğru akar veya dünya parça parça üretilir. En hafif ve pratik yaklaşım **sola doğru kayan engellerdir**.

```
[Set Timer by Event (Her 1.8 Saniyede Bir, Looping)]
       │
       ▼
[Random Float in Range (-200, +220)]
       │
       ▼
[SpawnActor: BP_BulutEngel (X: +1200, Y: 0, Z: OrtaNokta + Rand)]
       │
       ▼
[Rastgele Zar (%30 Çay, %30 Simit, %40 Yıldız)]
       │
       ▼
[SpawnActor: BP_Toplanabilir (Engelin Arasındaki Boşluğa)]
```

### Engel Hareketi (`BP_BulutEngel` Event Tick)
* `AddActorWorldOffset`: **Delta X = -1 * Hiz * DeltaSeconds** (Örn: `Hiz = 380`).
* **Otomatik Temizleme (Yok Etme):**
  * `GetActorLocation X` < `-1200.0` ise -> `DestroyActor` (Bellek şişmesini önler).

---

## 4. Kültürel Kombo & Puanlama Sistemi (`GM_BayrakOyun`)

Oyunu bağımlılık yapıcı hale getiren unsur, sıradan yıldız toplamanın ötesindeki **"Milli Kahvaltı Kombosu"**dur.

| Toplanan Öğe | Taban Puan | Efekt & Ses |
| :--- | :--- | :--- |
| ☕ **İnce Belli Çay** | +10 Puan | Ney tınısı (Re tonu) + Buhar partikülü |
| 🥯 **Çıtır Simit** | +15 Puan | Çıtırdayış + Susam saçılma partikülü |
| ⭐ **Şanlı Hilal** | +30 Puan | Mehter kös & zurna vuruşu |
| 🏆 **Çay + Simit Kombosu** | **+50 Bonus** | Ekranda *"Afiyet Olsun! Çay & Simit Kombosu"* yazısı |

### Kombo Blueprint Mantığı:
1. Bir `SonAlinanOge` enum/string değişkeni ve `SonAlinmaZamani` float değişkeni tutun.
2. Çay alındıktan sonra 4 saniye içinde Simit alınırsa (veya tersi):
   * `Skor += 50`
   * `Play Sound 2D (A_Kutlama_Mehter)`
   * Arayüze (Widget) `ShowComboAnimation` fonksiyonunu tetikleyin.

---

## 5. Kalıcı Rekor Sistemi (`SaveGame`)

1. **Blueprint Class** -> `SaveGame` seçip adını `SG_BayrakSkor` koyun.
2. İçine `EnYuksekSkor (Integer)` değişkeni ekleyin.
3. Oyun bittiğinde (`GameOver` anında):
   * `Does Save Game Exist (SlotName: "BayrakRekor")` kontrolü yapın.
   * Varsa `Load Game from Slot` -> Cast to `SG_BayrakSkor` -> Mevcut skor büyükse yeni rekoru yazıp `Save Game to Slot` deyin.
   * Yoksa `Create Save Game Object` ile ilk rekoru kaydedin.
