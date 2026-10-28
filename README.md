# 🇹🇷 Şafak Vakti: İstanbul Semalarında (Bayrak Rüzgarı)

> **İstanbul semalarında şanlı Türk bayrağını dalgalandır, şafak vaktinde simit ve tavşankanı çayları topla!**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![HTML5 / Canvas](https://img.shields.io/badge/Tech-HTML5%20Canvas%20%7C%20Web%20Audio-orange)](index.html)
[![Unreal Engine 5](https://img.shields.io/badge/Engine-Unreal%20Engine%205-blue)](unreal_engine_mimari_rehberi.md)

---

## 🎮 Oyunun Konsepti & Oynanış

**"Şafak Vakti: İstanbul Semalarında"** (veya samimi arcade adıyla **"Bayrak Rüzgarı"**), milli ve kültürel dokunuşlarla zenginleştirilmiş, son derece akıcı ve bağımlılık yapıcı bir arcade oyunudur.

* **Olay Örgüsü:** Gün doğumunda (şafak vakti) Boğaz'ın, Kız Kulesi'nin, minarelerin ve Galata Kulesi'nin silüeti eşliğinde süzülen şanlı Türk Bayrağını fırtınalı kara bulutlardan koruyoruz.
* **Oynanış Mekaniği:** Tek tuşla (`Space` veya ekrana tıklayarak/dokunarak) zıplama ve süzülme (Flappy dinamikleri).
* **Toplanabilir Kültürel Nesneler:**
  * ☕ **İnce Belli Çay:** +10 Puan
  * 🥯 **Çıtır Simit:** +15 Puan
  * ⭐ **Şanlı Hilal & Yıldız:** +30 Puan
  * 🏆 **Çay + Simit Kombosu:** Özel "Kahvaltı Bonusu!" (+50 Puan)
* **Atmosferik Sesler:** Web Audio API ile sıfır harici bağımlılıkla sentezlenmiş ney tınıları ve hilal toplandığında yükselen coşkulu mehter melodisi.

---

## 🚀 Canlı Oyna (Web Sürümü)

Proje, hiçbir kurulum veya kütüphane gerektirmeyen saf HTML5 Canvas ve Web Audio API ile çalışır.
Deponuzda **GitHub Pages** aktif edildiğinde oyun doğrudan `https://<kullanici-adiniz>.github.io/<repo-adi>/` adresinde oynanabilir.

### Yerel Olarak Çalıştırma:
1. Depoyu klonlayın:
   ```bash
   git clone https://github.com/husamettinbinici/safak-vakti.git
   cd safak-vakti
   ```
2. `index.html` dosyasını herhangi bir web tarayıcısında çift tıklayarak açın.

---

## 🛠️ Unreal Engine 5 Sürümü

Bu projenin Unreal Engine 5 (Blueprint) ortamında profesyonel bir 2D / 2.5D arcade oyununa dönüştürülmesi için hazırlanan teknik mimari kılavuzunu inceleyebilirsiniz:

👉 **[Unreal Engine 5 Blueprint & Mimari Kılavuzu](unreal_engine_mimari_rehberi.md)**

* **WPO Bayrak Materyali:** GPU üzerinden sinüs dalgalarıyla sıfır gecikmeli dalgalanan bayrak.
* **Karakter Hissiyatı (Game Feel):** Dikey hıza göre yumuşak açı enterpolasyonu (Pitch Tilt).
* **Sonsuz Üretici (Spawner):** Ekrana geldikçe üretilen ve bellekten temizlenen engeller.
* **Kalıcı Skor (SaveGame):** Cihaza yerel olarak rekor kaydetme mimarisi.

---

## 📜 Lisans

Bu proje [MIT Lisansı](LICENSE) kapsamında açık kaynak olarak paylaşılmıştır.
Geliştirmelere, katkılara ve kültürel yeni eklentilere (örneğin martı sesleri, Boğaz vapurları) tamamen açıktır!
