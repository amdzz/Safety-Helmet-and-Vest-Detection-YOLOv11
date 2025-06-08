# 🦺 Deteksi Helm & Rompi dengan YOLOv11

Aplikasi ini menggunakan model YOLOv11 yang telah dilatih khusus untuk mendeteksi penggunaan helm dan rompi pada gambar atau video. Aplikasi dijalankan menggunakan Streamlit untuk tampilan antarmuka yang interaktif dan sederhana.

---

## Cara Menjalankan Aplikasi
### 1. Buat Virtual Environment

download model pada link https://drive.google.com/file/d/1ZIzU9H-dbyaHdyILA2XoWpxxNxEZ1qOw/view

---

### 2. Buat Virtual Environment

```bash
python -m venv venv
venv\Scripts\activate      # Untuk Windows
source venv/bin/activate   # Untuk macOS/Linux
````

### 3. Install Dependensi

```bash
pip install -r requirements.txt
```

### 4. Jalankan Aplikasi Streamlit

```bash
streamlit run app.py
```

---

## Fitur Aplikasi

* **Deteksi Gambar**: Upload gambar (jpg, jpeg, png) untuk mendeteksi helm dan rompi.
* **Deteksi Video**: Upload video (mp4, avi, mov) untuk menjalankan deteksi secara frame-by-frame.
* **Deteksi Kamera (Real-Time)**: Jalankan deteksi langsung dari webcam secara live (hanya tersedia saat dijalankan secara lokal).
* **Visualisasi Otomatis**: Hasil deteksi ditampilkan secara langsung di halaman web menggunakan bounding box.

```
