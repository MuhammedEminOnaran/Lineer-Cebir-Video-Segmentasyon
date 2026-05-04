# Akışkan Video Segmentasyonu ve Arka Plan Çıkarma (Moving Video Segmentation)

Bu depo, **Lineer Cebir** dersi proje gereksinimleri kapsamında hareketli nesnelerin (araçların) akışkan video üzerinden segmentasyonu amacıyla geliştirilmiş kodları ve sonuçları içermektedir.

## 📌 Proje Özeti
Projede, açık kaynaklı bir araç segmentasyon veri seti (COCO128-seg) kullanılarak, hareketli nesnelerin arka plandan ayrıştırılması işlemi gerçekleştirilmiştir. İşlemler Google Colab ortamında, Python dili kullanılarak yapılmış olup, temelinde matris hesaplamaları ve morfolojik görüntü işleme teknikleri yatmaktadır.

## 🛠️ Kullanılan Teknolojiler ve Parametreler
* **Platform:** Google Colab (T4 GPU Hızlandırıcısı)
* **Model:** YOLOv8n-seg (Ultralytics)
* **Kütüphaneler:** OpenCV, NumPy

Proje yönergesinde istenen spesifik parametreler model içerisine şu şekilde entegre edilmiştir:
1. **Batch Size (16):** Veri setindeki görüntü matrisleri tek tek değil, 16'lı tensör paketleri halinde GPU'ya gönderilerek paralel hesaplama süreci optimize edilmiştir.
2. **Threshold / Olasılık Eşiği (0.30):** Modelin her piksel için ürettiği olasılık matrisinden dönen değerler, 0.30 eşik değerine (threshold) göre filtrelenmiş ve arka planın sıfırlandığı ikili (binary) maskelere dönüştürülmüştür.
3. **Kernel Boyutu (5x5):** Elde edilen ham segmentasyon maskesi üzerindeki ufak gürültüleri (noise) temizlemek amacıyla 5x5 boyutunda birler matrisi (`np.ones`) kullanılarak **Morfolojik Açılma (Morphological Opening)** işlemi uygulanmıştır.

## 🧮 Lineer Cebir Yaklaşımı
Bu projede video, $M \times N \times 3$ boyutlarında (RGB) ardışık matrisler dizisi olarak ele alınmıştır. Segmentasyon işlemi esnasında arka planın silinmesi `np.zeros_like` ile oluşturulan sıfırlar matrisi üzerine, sadece eşik değerini (threshold) geçen piksellerin izdüşümünün alınmasıyla (maskeleme) gerçekleştirilmiştir.

## 📂 Dosya Yapısı
* `segmentasyon_kodlari.ipynb`: Model eğitimi, eşikleme ve kernel işlemlerinin yapıldığı ana Google Colab defteri.
* `sonuc_videosu.mp4`: Parametrelerin uygulanmasıyla elde edilen, arka planın siyah (sıfır matrisi) olduğu ve sadece hareketli araçların tespit edildiği çıktı videosu.
