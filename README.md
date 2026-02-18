# MiDeSeC Veri Seti Üzerinde YOLOv8 ile Mitoz Tespiti ve Ön İşleme Tekniklerinin Başarıya Etkisi

**Hazırlayan:** Dilay Göl (220205030)  
**Danışman:** Dr. Nooshin Nemati Tolakan
Bu proje, Ostim Teknik Üniversitesi Sayısal Görüntü İşleme dersi kapsamında hazırlanmıştır.

![Project Banner](https://img.shields.io/badge/YOLOv8-Object%20Detection-blue) ![Python](https://img.shields.io/badge/Python-3.12-yellow) ![Status](https://img.shields.io/badge/Status-Completed-green)

## 📌 Proje Özeti
Mitoz sayımı, kanser evresinin belirlenmesinde patologlar için standart ve kritik bir prosedürdür. Ancak doku kesitlerinde; hücrelerin karmaşık yapısı, boyama farklılıkları ve düşük kontrast nedeniyle mitoz tespiti oldukça zor ve zaman alıcıdır.

Bu proje, **YOLOv8** nesne tespit modelini ve **CLAHE (Contrast Limited Adaptive Histogram Equalization)** görüntü işleme tekniğini kullanarak bu süreci otomatize etmeyi ve tespit doğruluğunu artırmayı hedeflemektedir. Çalışma sonucunda, ön işleme tekniklerinin model başarısını **%16.99** oranında artırdığı gözlemlenmiştir.

## 📂 Veri Seti (Dataset)
Projede Ankara Üniversitesi tarafından geliştirilen **MiDeSeC** (H&E boyalı doku görüntüleri) veri seti kullanılmıştır.

🔗 **Veri Seti Bağlantısı:** [NuSeC and MiDeSeC - Kaggle](https://www.kaggle.com/datasets/sonianmty/nusec-and-midesec)

- **Görüntü Formatı:** .bmp
- **Etiketler:** CSV formatındaki poligon koordinatları, YOLO formatına (normalize edilmiş bounding box) dönüştürülmüştür.
- **Veri Bölümleme:** Veri seti %80 Eğitim (Train) ve %20 Doğrulama (Validation) olacak şekilde ayrılmıştır.

## ⚙️ Yöntem ve Metodoloji

### 1. Model Mimarisi
Nesne tespiti için **YOLOv8m (Medium)** modeli seçilmiştir.

### 2. Görüntü Ön İşleme (Preprocessing)
Histopatolojik görüntülerde hücre ve arka plan arasındaki düşük kontrastı gidermek için iki farklı deney kurgulanmıştır:
* **Deney 1 (Raw):** Ham görüntüler üzerinde herhangi bir işlem yapılmadan eğitim.
* **Deney 2 (CLAHE):** Hücre sınırlarını belirginleştirmek için CLAHE algoritması (`clipLimit=2.0`, `tileGridSize=(8,8)`) uygulanmıştır.

### 3. Eğitim Parametreleri
* **Epoch:** 50
* **Görüntü Boyutu:** 1024x1024
* **Batch Size:** 8
* **Optimizer:** AdamW

## 📊 Deneysel Sonuçlar
Yapılan deneyler sonucunda CLAHE ön işlemesinin model performansını tüm metriklerde artırdığı kanıtlanmıştır.

| Metrik | Raw (Ham Veri) | CLAHE (İşlenmiş) | Değişim (Fark) |
|-------------|----------------|------------------|----------------|
| **mAP@50** | 0.6606 | **0.7683** | 📈 **+%16.31** |
| **Precision** | 0.8611 | **0.9411** | 📈 **+%9.29** |
| **Recall** | 0.5909 | **0.7264** | 📈 **+%22.93** |
| **F1-Score** | 0.7009 | **0.8199** | 📈 **+%16.99** |

> **Sonuç:** Karmaşıklık matrisleri incelendiğinde, CLAHE ile eğitilen modelde sınıflandırma hatalarının (özellikle Yanlış Pozitiflerin) azaldığı ve mitoz tespitindeki güvenilirliğin arttığı doğrulanmıştır.

## 🚀 Kurulum ve Kullanım

Bu projeyi yerel ortamınızda veya Kaggle üzerinde çalıştırmak için aşağıdaki adımları izleyebilirsiniz.

### Gereksinimler
```bash
pip install ultralytics opencv-python pandas matplotlib numpy scikit-learn