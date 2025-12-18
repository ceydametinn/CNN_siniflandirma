# CNN Sınıflandırma
Harika bir karar. Projenin sadece koddan ibaret olmadığını, arkasında ciddi bir **parametre mühendisliği (Hyperparameter Engineering)** ve stratejik analiz olduğunu gösteren, Github profilinde "Ben bu işi biliyorum" diye bağıracak profesyonel bir `README.md` hazırladım.

Az önce senin yazdığın o harika teknik analiz kısmını ("Dropout 0.5 vs 0.2" vb.) metnin **"Teknik Mimari ve Hiperparametre Analizi"** başlığı altına özel olarak yerleştirdim.

Bunu kopyala, GitHub'da `README.md` dosyanın içine yapıştır.

-----

# 🧵 CNN İle Nesne Sınıflandırma: Düğme ve Makara Tespiti

## 📖 Proje Hakkında

Bu proje, **Makine Öğrenmesi (BLG-407)** dersi kapsamında geliştirilmiş bir Görüntü İşleme (Computer Vision) çalışmasıdır. Projenin temel amacı, kısıtlı ve özgün bir veri seti üzerinde **Transfer Learning (Bilgi Transferi)** ile **Sıfırdan CNN Eğitimi (From Scratch)** yöntemlerinin performanslarını karşılaştırmak ve "Small Data" (Az Veri) problemini aşmak için gerekli optimizasyon stratejilerini belirlemektir.

Proje kapsamında **Düğmeler** ve **Makaralar** olmak üzere iki sınıfı içeren özgün bir veri seti oluşturulmuş ve üç farklı model mimarisi üzerinde kapsamlı deneyler yapılmıştır.

-----

## 🛠️ Kullanılan Teknolojiler ve Araçlar

  * **Dil:** Python 3.10
  * **Framework:** TensorFlow & Keras
  * **Veri İşleme:** NumPy, Pandas
  * **Görselleştirme:** Matplotlib
  * **Ortam:** Google Colab (GPU Destekli)

-----

## 📂 Veri Seti (Dataset)

Veri seti, proje kapsamında internetten alınmamış, **tamamen özgün olarak** tarafımdan oluşturulmuştur.

  * **Sınıflar:** `dugmeler` (Buttons) ve `makaralar` (Spools)
  * **Veri Kaynağı:** Farklı açılardan ve ışık koşullarından çekilmiş fotoğraflar.
  * **Toplam Görüntü Sayısı:** 200 Adet.
  * **Ön İşleme:**
      * Yeniden Boyutlandırma: `128x128` piksel.
      * Normalizasyon: Piksel değerleri 0-255 aralığından 0-1 aralığına çekilmiştir.
  * **Veri Ayrımı:** %80 Eğitim (Training) - %20 Doğrulama (Validation).

-----

## 🧠 Uygulanan Modeller ve Stratejiler

Başarıyı artırmak ve yöntemleri kıyaslamak adına 3 aşamalı bir yol izlenmiştir:

### 1️⃣ Model 1: Transfer Learning (VGG16)

  * **Yöntem:** ImageNet üzerinde eğitilmiş **VGG16** modelinin ağırlıkları kullanılarak "Fine-Tuning" yapılmıştır.
  * **Sonuç:** Hazır modelin güçlü özellik çıkarıcıları sayesinde %97.50 başarıya ulaşılmıştır.

### 2️⃣ Model 2: Basit CNN (Sıfırdan Eğitim)

  * **Yöntem:** 3 Bloklu (Conv2D + MaxPool) özel bir CNN mimarisi kurulmuştur.
  * **Gözlem:** Veri artırma ve optimizasyon yapılmadığı için modelin veriyi **ezberlediği (Overfitting)** ve Loss değerinin 1.06 seviyelerine çıktığı görülmüştür.

### 3️⃣ Model 3: Gelişmiş CNN (Optimizasyon & Augmentation)

  * **Yöntem:** Model 2'nin optimize edilmiş halidir.
  * **Uygulanan Stratejiler:**
      * **Data Augmentation:** Veri seti sanal olarak (döndürme, kaydırma, zoom) artırıldı.
      * **ModelCheckpoint:** Eğitim sırasında en iyi performans gösteren ağırlıklar kaydedildi.
      * **Hiperparametre Ayarı:** Dropout ve Learning Rate optimize edildi (Aşağıda detaylandırılmıştır).

-----

## 🧪 Teknik Mimari ve Hiperparametre Analizi

Bu çalışmada yapılan hiperparametre değişikliklerinin model başarısına etkisi detaylıca analiz edilmiştir:

### 1\. Filtre Sayısı ve Mimari

Model 2 ve Model 3'te **32-64-128** filtre yapısı korunmuştur. Bu yapı, 128x128 boyutundaki basit morfolojiye sahip nesneler (Düğme/Makara) için yeterli özellik çıkarımı (feature extraction) sağlamaktadır. Daha derin bir ağ (örneğin 256 filtre) bu veri boyutunda gereksiz parametre artışına ve işlem yüküne yol açacağı için tercih edilmemiştir.

### 2\. Dropout Oranının Kritik Etkisi (0.5 vs 0.2)

  * **Model 2 Durumu:** Standart **0.5** Dropout kullanılmış ancak model az veriyle yeterince öğrenemeden nöronlar kapatıldığı için performans düşük kalmıştır (Underfitting sinyalleri).
  * **Model 3 Çözümü:** Dropout oranı **0.2'ye** düşürülerek modelin kapasitesi artırılmış, ancak **Veri Artırma (Augmentation)** ile ezberleme (overfitting) dengelenmiştir.

### 3\. Öğrenme Oranı (Learning Rate) Optimizasyonu

  * **Model 2 Durumu:** Varsayılan Learning Rate (0.001) kullanıldığında Loss grafiğinde kararsız dalgalanmalar görülmüştür.
  * **Model 3 Çözümü:** Oran **0.0001'e** çekilerek modelin gradyan inişinde (gradient descent) daha küçük ve emin adımlarla ilerlemesi sağlanmış, bu da başarı artışına doğrudan katkıda bulunmuştur.

-----

## 📊 Deneysel Sonuçların Karşılaştırılması

Aşağıdaki tablo, üç farklı modelin başarı oranlarını ve hata paylarını (Loss) özetlemektedir.

| Model No | Mimari Tipi | Veri Artırma | Dropout & LR | Test Başarısı (Accuracy) | Test Kaybı (Loss) | Sonuç Yorumu |
| :--- | :--- | :---: | :--- | :---: | :---: | :--- |
| **Model 1** | Transfer Learning (VGG16) | Hayır | 0.5 / 0.0001 | **%97.50** | **0.1304** | Hazır ağırlıklar sayesinde kusursuz ayrım sağlandı. |
| **Model 2** | Basit CNN (Sıfırdan) | Hayır | 0.5 / Default | **%70.00** | **1.0651** | Veri azlığı sebebiyle aşırı öğrenme (Overfitting) yaşandı. Loss çok yüksek. |
| **Model 3** | **Gelişmiş CNN** | **EVET** | **0.2 / 0.0001** | **%82.50** | **0.6322** | Veri artırma ve LR optimizasyonu ile **Loss yarı yarıya düşürüldü**. Kararlı öğrenme sağlandı. |

### 📈 Sonuç Değerlendirmesi

Model 1 (Transfer Learning) en yüksek başarıyı verse de, donanım maliyeti yüksektir. **Model 3**, uygulanan mühendislik teknikleri sayesinde, sıfırdan eğitilen ve az veriye sahip bir modelin bile **%80 barajını aşabileceğini** kanıtlamıştır. Özellikle Model 2'ye kıyasla Loss değerindeki ciddi düşüş, modelin güvenilirliğini artırmıştır.

-----

## 🚀 Kurulum ve Çalıştırma

Projeyi kendi bilgisayarınızda veya Google Colab üzerinde çalıştırmak için:

1.  Repoyu klonlayın:
    ```bash
    git clone https://github.com/kullanici_adiniz/Proje_Ismi.git
    ```
2.  Gerekli kütüphaneleri yükleyin:
    ```bash
    pip install tensorflow numpy matplotlib pandas
    ```
3.  Notebook dosyalarını (`Model1.ipynb`, `Model2.ipynb`, `Model3.ipynb`) sırasıyla çalıştırın.

-----



Ceyda Metin
**Bölüm:** Bilgisayar Mühendisliği
