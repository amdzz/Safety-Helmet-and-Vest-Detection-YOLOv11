# Safety-Helmet-and-Vest-Detection-YOLOv11

Kelompok 5:
- ZONI ARYANTONI ALBAB (G1A022043)  
- YEBI DEPRIANSYAH (G1A022063)  
- AHMAD ZUL ZHAFRAN (G1A022088)

Berikut adalah bagian pertama dari laporan dalam format `README.md` berdasarkan struktur CRISP-DM dan skenario tugas Anda:

---

## Business Understanding

Proyek ini dibuat sebagai bentuk pemenuhan tugas Ujian Akhir Semester (UAS) mata kuliah **Computer Vision**, yang diampu oleh **Arie Vatresia, S.T., M.T.I., Ph.D.**. Tujuan dari tugas ini adalah untuk merancang sistem **pengenalan visual berbasis komputer** yang mampu mendeteksi atribut keselamatan berupa **helm dan rompi keselamatan** yang digunakan oleh para pekerja proyek konstruksi.

### Latar Belakang Skenario

Sebuah **startup rintisan** berencana membangun sistem pemantauan keselamatan berbasis CCTV untuk meningkatkan **kepatuhan pekerja terhadap protokol K3** (Keselamatan dan Kesehatan Kerja). Sistem ini akan bekerja secara **real-time** dan membantu mengidentifikasi apakah setiap individu di area proyek telah menggunakan **helm dan rompi** sesuai standar keselamatan.

Sebagai bagian dari tim pengembang sistem, kami ditugaskan untuk:

1. **Menentukan metode deteksi objek** terbaik untuk kasus ini (misalnya: YOLO, SSD, Faster R-CNN).
2. **Mengumpulkan dan memberi anotasi** data objek seperti helm dan rompi.
3. **Membangun arsitektur sistem komputer visi** untuk menyelesaikan permasalahan ini.
4. **Melakukan evaluasi performa model**, baik dari sisi akurasi deteksi maupun efisiensi inferensi.
5. Memberikan **rekomendasi pengembangan sistem** ke depannya agar dapat digunakan di dunia nyata.

### Tujuan Utama

* Membangun model deteksi objek yang mampu mengenali keberadaan **helm** dan **rompi keselamatan** secara akurat dan real-time.
* Menyediakan dasar sistem **monitoring otomatis** berbasis visual yang dapat membantu **pengawasan proyek konstruksi**.
* Menghasilkan evaluasi performa sistem dan **strategi pengembangan lanjutan** untuk peningkatan di masa depan.

Terima kasih! Dengan informasi tambahan ini, berikut adalah versi lengkap dan terperinci untuk bagian **Data Understanding** dari `README.md`, telah diperbarui sesuai dengan sumber dataset yang digunakan:

---

## Data Understanding

Untuk menyelesaikan proyek ini, kami menggunakan dataset **Safety Helmet and Reflective Jacket** yang tersedia secara publik di Kaggle. Dataset ini dibuat oleh **Nirav B Naik** dan dirancang khusus untuk pelatihan model deteksi objek dalam konteks **keselamatan kerja**.

### Deskripsi Dataset

Dataset ini terdiri dari **10.500 gambar** yang telah dianotasi dengan **bounding box** untuk dua jenis objek:

* **Helm keselamatan** (safety helmet)
* **Rompi reflektif** (reflective jacket)

Setiap gambar diambil dari berbagai **lingkungan kerja nyata** seperti:

* Lokasi proyek konstruksi
* Area pabrik
* Area kerja luar ruangan

> Rata-rata, setiap gambar memiliki **5 objek** yang dianotasi, menciptakan beragam skenario visual yang mencerminkan kondisi di lapangan.

### Format dan Struktur

Dataset disusun dalam format **YOLOv7**, yang berarti:

* Setiap gambar memiliki **file label (.txt)** yang menyertakan informasi bounding box.

* Format anotasi:

  ```
  <class_id> <x_center> <y_center> <width> <height>
  ```

  (semua nilai dalam skala relatif terhadap ukuran gambar)

* Format gambar: **JPEG (.jpg)**

### Pembagian Dataset

Dataset ini telah dibagi sebelumnya untuk keperluan pelatihan dan evaluasi:

| Split     | Jumlah Gambar | Jumlah Anotasi | Jumlah File Total |
| --------- | ------------- | -------------- | ----------------- |
| **Train** | 7.350 (70%)   | 7.350          | 14.700            |
| **Test**  | 1.575 (15%)   | 1.575          | 3.150             |
| **Valid** | 1.575 (15%)   | 1.575          | 3.150             |
| **Total** | 10.500        | 10.500         | 21.000            |

> Setiap file label mencerminkan keberadaan **satu atau lebih objek**, membuat dataset ini ideal untuk model **deteksi objek multi-kelas**.

### Keunggulan Dataset

* Volume besar (10.5K gambar) cocok untuk deep learning.
* Variasi tinggi dalam lokasi dan kondisi pencahayaan.
* Cocok untuk industri **konstruksi, manufaktur, dan pertambangan**.
* Sudah siap pakai untuk model **YOLOv7 dan turunannya (termasuk YOLOv11)**.

---

Berikut adalah laporan lanjutan dengan judul **"## Model dan Evaluasi"** yang mendeskripsikan proses training model YOLO dan hasil evaluasinya berdasarkan output yang Anda berikan:

---

## Model dan Evaluasi

### Model yang Digunakan

Pada proyek ini, digunakan model **YOLOv8 (YOLO11m.pt)** dari pustaka **Ultralytics** untuk mendeteksi dua jenis objek keselamatan kerja, yaitu:

* **Safety Helmet**
* **Reflective Jacket**

Model dilatih langsung tanpa melakukan preprocessing manual pada data gambar. Proses pelatihan dilakukan menggunakan file konfigurasi `data.yaml` yang mendeskripsikan dataset, label, dan path direktori.

### Konfigurasi Pelatihan

Model dilatih dengan parameter sebagai berikut:

* `epochs`: 10
* `imgsz`: 640
* `batch`: 16
* `device`: 0 (GPU)
* `patience`: 5 (early stopping apabila tidak ada peningkatan dalam 5 epoch)
* `model`: YOLOv8 pretrained (`yolo11m.pt`)

```python
model.train(
    data="/content/safety-Helmet-Reflective-Jacket/data.yaml",
    epochs=10,
    imgsz=640,
    batch=16,
    device=0,
    patience=5,
    project="runs/train",
    name="exp"
)
```

### Proses Pelatihan

Model dilatih selama **10 epoch**, dengan peningkatan performa yang cukup signifikan dari awal hingga akhir. Berikut adalah ringkasan hasil evaluasi tiap epoch berdasarkan metrik utama:

| Epoch | Precision | Recall | mAP\@0.5  | mAP\@0.5:0.95 |
| ----- | --------- | ------ | --------- | ------------- |
| 1     | 0.596     | 0.455  | 0.480     | 0.219         |
| 2     | 0.729     | 0.647  | 0.720     | 0.426         |
| 3     | 0.725     | 0.676  | 0.740     | 0.468         |
| 4     | 0.816     | 0.729  | 0.819     | 0.539         |
| 5     | 0.858     | 0.803  | 0.887     | 0.612         |
| 6     | 0.858     | 0.827  | 0.900     | 0.632         |
| 7     | 0.882     | 0.842  | 0.923     | 0.680         |
| 8     | 0.894     | 0.867  | 0.935     | 0.700         |
| 9     | 0.891     | 0.882  | 0.941     | 0.693         |
| 10    | 0.899     | 0.898  | **0.949** | **0.727**     |

### Evaluasi Akhir Model

Model terbaik disimpan sebagai `best.pt` pada direktori `runs/train/exp/weights/`. Evaluasi akhir terhadap model ini menghasilkan:

* **Precision (P)**: 0.898
* **Recall (R)**: 0.898
* **mAP\@0.5**: 0.949
* **mAP\@0.5:0.95**: 0.727

#### Evaluasi Per Kelas

| Kelas             | Precision | Recall | mAP\@0.5 | mAP\@0.5:0.95 |
| ----------------- | --------- | ------ | -------- | ------------- |
| Safety Helmet     | 0.934     | 0.906  | 0.960    | 0.729         |
| Reflective Jacket | 0.863     | 0.889  | 0.937    | 0.725         |

### Interpretasi

* **Model menunjukkan performa deteksi yang sangat baik** untuk kedua kelas, dengan nilai mAP\@0.5 > 0.93 dan mAP\@0.5:0.95 > 0.72.
* **Precision dan recall seimbang**, menunjukkan model tidak hanya tepat tetapi juga sensitif dalam mendeteksi objek keselamatan kerja.
* Model cukup cepat, dengan waktu inferensi sekitar **11.5 ms per gambar**, cocok untuk aplikasi real-time di lingkungan kerja industri atau proyek pembangunan.

### Kesimpulan

Model YOLOv8 mampu memberikan hasil yang sangat baik dalam mendeteksi objek keselamatan seperti **helm dan rompi reflektif**. Tanpa preprocessing tambahan, model tetap mampu belajar secara efektif karena dataset yang sudah ditata dengan baik.

Langkah selanjutnya adalah melakukan **inference** pada data uji lapangan atau real-time footage dan melakukan deployment jika dibutuhkan.

---

Berikut adalah versi bagian **## Hasil dan Inferensi** tanpa kode, berfokus pada penjelasan naratif hasil dan interpretasinya:

---

## Hasil dan Inferensi

### Visualisasi Hasil Prediksi menggunakan foto

![image](https://github.com/user-attachments/assets/ffdbe693-554e-4d1e-9681-b2eed06ac884)


Untuk menguji kemampuan model deteksi objek yang telah dilatih, dilakukan proses inferensi terhadap delapan gambar uji yang dipilih secara acak dari dataset. Pada proses ini, model memindai setiap gambar dan menghasilkan prediksi berupa kotak pembatas (bounding box) pada objek-objek yang dikenali, lengkap dengan label klasifikasi dan skor kepercayaan (confidence score) untuk masing-masing deteksi.

### Interpretasi Hasil

Berdasarkan visualisasi hasil prediksi:

* Model **berhasil mendeteksi dengan akurasi tinggi** dua jenis Alat Pelindung Diri (APD) utama, yaitu **Helm Keselamatan (Safety Helmet)** dan **Jaket Reflektif (Reflective Jacket)**.
* Setiap objek yang terdeteksi ditandai dengan **kotak pembatas berwarna hijau**, disertai label dan nilai kepercayaan yang umumnya berada di atas angka **0.90**, menandakan tingkat keyakinan yang sangat baik dari model.
* Model mampu mengenali APD **dalam berbagai kondisi**, termasuk:

  * Latar belakang lokasi yang berbeda (dalam maupun luar ruangan).
  * Beragam pose dan orientasi tubuh pekerja.
  * Situasi pencahayaan yang bervariasi.

Deteksi tetap akurat meskipun objek berada dalam posisi yang tidak ideal atau terdapat beberapa individu dalam satu gambar. Hal ini menunjukkan bahwa model mampu melakukan **generalization** dengan baik terhadap variasi dalam data uji.

### Inferensi dan Aplikasi

Dari hasil yang diperoleh, dapat disimpulkan bahwa model:

* **Siap digunakan dalam skenario dunia nyata**, khususnya untuk sistem pemantauan keselamatan berbasis komputer.
* Memiliki potensi untuk **diintegrasikan ke dalam sistem real-time**, seperti kamera pengawas di area kerja berisiko.
* Mampu memberikan **peringatan dini atau pelaporan otomatis** apabila ditemukan pekerja yang tidak mengenakan APD yang sesuai.

Hasil inferensi ini memperkuat validasi bahwa model telah mencapai performa deteksi objek yang sangat memadai dan **siap untuk diimplementasikan lebih lanjut pada sistem otomatisasi keselamatan kerja.**

---

### Inferensi pada Video

![image](https://github.com/user-attachments/assets/c4d159b8-1e14-4243-b45b-75f2f1002e5f)


Sebagai langkah lanjutan setelah pengujian pada gambar statis, dilakukan juga evaluasi performa model deteksi objek terhadap **data video**. Sebuah video pendek berisi dua individu yang sedang berjalan digunakan sebagai bahan uji. Model dipasang untuk melakukan prediksi pada setiap **frame video** secara real-time, dengan hasil deteksi divisualisasikan dalam bentuk kotak pembatas dan label pada objek yang terdeteksi.

### Hasil Deteksi

Dari hasil pengujian terhadap video tersebut, diperoleh temuan sebagai berikut:

* **Model berhasil mendeteksi kedua individu** secara konsisten sepanjang video.
* Setiap individu yang teridentifikasi oleh model **ditandai mengenakan Safety Helmet dan Reflective Jacket**, sesuai dengan label prediksi yang ditampilkan.
* Kotak pembatas muncul secara **stabil dan tepat sasaran** pada setiap frame, menunjukkan kemampuan pelacakan (tracking) yang baik dari model terhadap objek yang bergerak.

### Inferensi dan Validasi

Kemampuan model dalam mengenali **APD pada objek bergerak** menunjukkan bahwa:

* Model tidak hanya efektif pada gambar diam, tetapi juga dapat **diaplikasikan untuk deteksi objek secara dinamis dalam video**, yang merupakan skenario realistis di lingkungan kerja seperti proyek konstruksi atau area industri.
* Akurasi deteksi pada video menunjukkan **konsistensi prediksi dari frame ke frame**, yang menjadi indikator penting untuk aplikasi pemantauan keselamatan berbasis kamera pengawas.

### Kesimpulan dari Uji Video

Berdasarkan hasil pengujian ini, dapat disimpulkan bahwa model deteksi objek yang telah dilatih mampu:

* **Mendeteksi APD (Safety Helmet dan Reflective Jacket) dengan akurasi tinggi dalam video.**
* Melacak keberadaan pekerja secara **berkelanjutan dan stabil dalam kondisi pergerakan.**
* **Siap untuk digunakan dalam sistem pemantauan berbasis video real-time**, seperti sistem keamanan otomatis yang memberikan notifikasi ketika seseorang tidak mengenakan perlengkapan keselamatan yang wajib.

---




