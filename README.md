# 🎬 IMDB Sentiment Analysis with RNN (Keras & TensorFlow)

Bu proje, Keras ve TensorFlow kütüphaneleri kullanılarak IMDB veri seti üzerinden **Yinelenen Sinir Ağları (RNN)** ile film yorumlarının duygu analizi (Olumlu / Olumsuz) yapılmasını sağlayan bir derin öğrenme çalışmasıdır.

---

## 🛠️ Kullanılan Teknolojiler ve Araçlar
* **Python** (3.13.7)
* **TensorFlow / Keras**
* **NumPy**
* **RNN (SimpleRNN Mimarisi)**

---

## 🚀 Proje Adımları ve Akışı

### 1. Veri Setinin Hazırlanması ve Yüklenmesi
* **Kaynak:** Keras kütüphanesinin dahili IMDB veri seti kullanıldı.
* **Kelime Sınırı (`num_words=15000`):** En sık kullanılan popüler 15.000 kelime seçildi, nadir kelimeler filtrelendi.
* **Standart Boyut (`maxlen=130`):** Cümle uzunlukları `pad_sequences` ile 130 kelimeye sabitlendi (kısa kalanlar sıfırlarla dolduruldu).

### 2. Model Mimarisi (RNN)
Metinlerin ardışıklığını ve bağlamını yakalamak için şu katmanlar kuruldu:
* **Embedding Katmanı:** Kelimeleri 32 boyutlu vektör uzayına taşıdı.
* **SimpleRNN Katmanı:** 16 nörondan oluşan temel RNN katmanı (`ReLU` aktivasyonu).
* **Dense Katmanı:** Çıktıyı `0` (Olumsuz) ile `1` (Olumlu) arasında bir olasılığa sıkıştıran `Sigmoid` çıkış katmanı (`1` nöron).

### 3. Modelin Eğitimi (Training)
* Model **`rmsprop`** optimizatörü ve `binary_crossentropy` maliyet fonksiyonu ile derlendi.
* Eğitim ve doğrulama metrikleri takip edilerek modelin öğrenme süreci gözlemlendi.

### 4. Canlı Tahmin Pipeline'ı (Real-Time Inference)
Eğitilen model dışarıdan girilen ham İngilizce metinlerle test edildi:
* Metin küçük harflere çevrilip kelimelerine ayrıldı.
* IMDB resmi sözlüğü (`imdb.get_word_index()`) ile sayısal ID'lere dönüştürüldü.
* Keras kaydırma kuralı gereği gerçek ID'lere **`+3`** eklendi; sözlükte bulunmayan kelimeler için **`2` (OOV)** kodu atandı.
* `pad_sequences` ile boyutlandırılarak modele (`model.predict`) verildi ve `Sigmoid` olasılık skoruyla duygu sınıfı (Olumlu / Olumsuz) raporlandı.
