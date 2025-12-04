# 18_Butona_Basma_Sayisi_Kadar_Sure_LED_Yakma_Kisa_Kod (Refactored)

Bu proje, **STM32F407-Discovery** kartı üzerinde butona kaçıncı kez basıldığına bağlı olarak LED'in yanma süresini dinamik olarak değiştiren uygulamanın **kısaltılmış ve optimize edilmiş** versiyonudur.

Bu depo, matematiksel mantık kullanarak `if-else` bloklarını nasıl ortadan kaldırabileceğimizi ve "Temiz Kod" (Clean Code) prensiplerini gösterir.

> **🔗 Versiyon Karşılaştırması**
>
> Bu projenin, her durumun (1. basış, 2. basış...) ayrı `if-else` bloklarıyla uzun uzun yazıldığı temel versiyonu için:
>
> ➡️ **[17_Butona_Basma_Sayisi_Kadar_Sure_LED_Yakma (Temel Yöntem)](https://github.com/yurttaskardelen/17_Butona_Basma_Sayisi_Kadar_Sure_LED_Yakma)**
>
> * **Proje 17 (Uzun):** Her saniye artışı için yeni kod satırları gerekir.
> * **Proje 18 (Bu Proje):** Tek bir formül ile sınırsız saniye kontrolü yapılabilir.

---

### 🎯 Proje Senaryosu

Sistem, bir değişkeni (`butona_basildi`) sayaç olarak kullanır ve bu sayacı doğrudan gecikme fonksiyonuna (`HAL_Delay`) parametre olarak gönderir.

1.  **Her Basışta:** Sayaç 1 artar.
2.  **Sınır Kontrolü:** Eğer sayaç 5 veya daha az ise;
    * LED yanar.
    * `Sayaç x 1000` milisaniye boyunca yanık kalır (Örn: 3. basışta 3000 ms).
    * LED söner.
3.  **Sınır Aşımı:** Eğer sayaç 5'i geçerse (6. basış), sayaç sıfırlanır ve LED yanmaz (veya bir sonraki turda 1'den başlar).

**Avantaj:** Eğer süreyi 5 saniyeden 20 saniyeye çıkarmak isterseniz, kodda sadece `if (butona_basildi <= 5)` kısmındaki 5'i 20 yapmanız yeterlidir. Kod satırı eklemenize gerek yoktur.

---

### ⚙️ Pull-Up Konfigürasyonu

Projenin düzgün çalışması için `.ioc` dosyasında buton pininin (`PA0`) **Pull-Up** olarak ayarlanması gereklidir.

* **Pin:** `PA0` -> `GPIO_Input`
* **Resistor:** `Pull-up`
  
<img width="843" height="644" alt="image" src="https://github.com/user-attachments/assets/a5bccc60-b813-4f18-9e9a-a4f0fd3519bf" />

---

### 🛠️ Gerekli Donanım

* **1x** STM32F407-Discovery Geliştirme Kartı
* **1x** LED
* **1x** 220 Ohm Direnç
* **1x** Push-Button
* **Breadboard ve Jumper Kablolar**

---

### 🔌 Devre Şeması

Buton bağlantısı **Pull-Up** mantığına göre (GND'ye) yapılmalıdır.

| Bileşen | STM32 Pini | Bağlantı Detayı |
| :--- | :--- | :--- |
| **Buton** | `PA0` | Bir bacak **PA0**, diğer bacak **GND** |
| **LED** | `PA1` | Anot -> **PA1**, Katot -> Direnç -> **GND** |


<img width="346" height="480" alt="image" src="https://github.com/user-attachments/assets/5b2998e0-3e4e-4f1a-84cc-8264f9fee38a" />

---

### 💻 Kod Bloğu (Refactor Edilmiş)

<img width="593" height="578" alt="image" src="https://github.com/user-attachments/assets/44d6365a-5f00-4dd1-b006-a705d7f1ef31" />

---

### 🚀 Nasıl Kullanılır?

1.  Bu depoyu klonlayın (`git clone ...`).
2.  STM32CubeIDE yazılımını açın.
3.  `File > Open Projects from File System...` seçeneği ile proje klasörünü seçin.
4.  Proje içindeki `.ioc` dosyasını açarak pin yapılandırmasını inceleyebilirsiniz.
5.  Derleyin (Build) ve ST-Link V2 üzerinden kartınıza yükleyin (Run).
