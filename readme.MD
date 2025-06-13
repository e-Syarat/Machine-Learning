# Capstone Project: E-Syarat Model 
---
E-Syarat adalah platform pembelajaran alfabet bahasa isyarat berbasis website yang dirancang secara interaktif menggunakan teknologi machine learning. Aplikasi ini dikembangkan untuk mendukung komunikasi yang lebih inklusif, dengan target pengguna mencakup masyarakat umum, keluarga penyandang tuna rungu wicara, serta komunitas tuna rungu wicara sendiri.

Proyek ini merupakan bagian dari capstone project kelas Machine Learning. Tujuannya adalah membangun model object detection sederhana untuk mengenali dan mengklasifikasikan huruf dalam gambar menggunakan arsitektur YOLO dan CNN dengan TensorFlow dan Keras.

---


## Tujuan
- Mendeteksi posisi huruf pada gambar (bounding box)
- Mengklasifikasikan huruf (A-Z) dalam gambar
- Mengevaluasi kinerja model dengan metrik loss, akurasi, dan Intersection over Union (IoU)
- Membuat sistem inferensi dan menyimpan model ke format TensorFlow.js

## **Dataset**

Dataset berupa kumpulan gambar huruf dan file anotasi .csv yang berisi:
- `filename`: Nama file gambar
- `width`, `height`: Ukuran gambar
- `xmin`, `ymin`, `xmax`, `ymax`: Koordinat bounding box
- `class`: Label huruf (A-Z)

## **Alur Proyek**
1. **Data Preparation**
    - Membaca file anotasi dan memeriksa kelengkapan gambar
    - One-hot encoding label huruf
    - Normalisasi koordinat bounding box
    - Resize gambar ke 128x128 piksel
    - Split data ke dalam set train (70%), val (15%), dan test (15%)

2. **Augmentasi**

    `ImageDataGenerator` dengan rotasi dan zoom

3. **Model CNN dengan kombinasi YOLO diimplementasi pada tensorflow**
    - CNN bertingkat dengan Conv2D, MaxPooling2D, Flatten
    - Dua output: bounding box (4 dimensi) dan klasifikasi huruf (26 kelas)
    - Gabungan output menjadi satu tensor [bbox + class]

4. **Custom Loss Function**
    Gabungan dari:
    - Bounding Box Loss: Mean Squared Error (MSE)
    - Class Loss: Categorical Crossentropy
    `total_loss = bbox_loss + 5.0 * class_loss`

5. **Training**
    Optimizer: Adam
    Callback: EarlyStopping (patience=4)
    Evaluasi selama pelatihan berdasarkan val_loss dan val_accuracy

6. **Evaluasi**
    `Accuracy`: **94%**
    Evaluasi akhir dengan metrik:
    - `Validation Accuracy`: **89%** 
    - `Mean IoU (Intersection over Union)` : **0.5734**

7. **Visualisasi**
    Menampilkan prediksi bounding box dan label huruf pada 10 sampel test

8. **Inferensi**
    Pengguna dapat mengunggah gambar dan melihat prediksi model secara visual
    Disediakan fungsi inference() untuk prediksi gambar tunggal


## **Model dan Konversi**
Model disimpan dalam dua format:
- .h5 untuk penggunaan lokal
- TensorFlow.js untuk penggunaan web

```
tensorflowjs_converter \
  --input_format=tf_saved_model \
  --output_format=tfjs_graph_model \
  sample_data/tf_model/ sample_data/tfjs_model/
```

## **Hasil Evaluasi**
- Validation Loss: Tergantung hasil akhir training
- Validation Accuracy: Tergantung training
- Mean IoU: Tergantung training (contoh: 0.72)

## **Teknologi yang Digunakan**
    Python
    TensorFlow / Keras
    Scikit-learn
    OpenCV & Matplotlib
    Google Colab
    TensorFlow.js

## **Struktur File**

.
├── Dataset/
│   ├── data/                 # Dataset
│   └── annotations/          # File _annotations.csv
├── model.h5                  # Model terlatih
├── tfjs_model/               # Model versi TensorFlow.js
└── capstone_notebook.ipynb   # Notebook utama proyek

Untuk setiap library yang digunakan dapat diakses pada `requirements.txt`