# 🩷 Klasifikasi Permasalahan Kulit Wajah — Aplikasi Web (SVM)

Aplikasi web untuk klasifikasi permasalahan kulit wajah (acne, dark spots,
redness, pores, wrinkles, normal) menggunakan ekstraksi fitur warna (RGB,
HSV) dan tekstur (GLCM, LBP) dengan algoritma **SVM**. Mendukung **upload
foto** maupun **scan langsung via kamera**, dengan tampilan warna yang
disesuaikan dengan poster (maroon + pink + cream).

---

## 📁 Struktur Folder

```
skinapp/
├── app.py                  # Aplikasi utama Streamlit
├── features.py              # Pipeline ekstraksi fitur (identik dgn notebook)
├── requirements.txt         # Daftar dependency
├── .streamlit/
│   └── config.toml          # Tema warna aplikasi
└── saved_model/
    ├── model_svm.pkl        # ⚠️ wajib kamu tambahkan sendiri
    ├── scaler.pkl            # ⚠️ wajib
    ├── classes.pkl            # ⚠️ wajib
    ├── best_params.pkl        # opsional
    └── metrics.pkl             # opsional
```

## 1️⃣ Langkah 1 — Ambil file model dari Google Drive

Notebook training kamu menyimpan model ke:
`/content/drive/MyDrive/saved_model/`

Download 5 file `.pkl` dari folder tersebut, lalu **timpa** file
`saved_model/PUT_MODEL_FILES_HERE.txt` dengan file-file itu (taruh di
folder `saved_model/` yang sama).

> File **wajib**: `model_svm.pkl`, `scaler.pkl`, `classes.pkl`
> File **opsional** (untuk menampilkan info akurasi & parameter di
> sidebar): `best_params.pkl`, `metrics.pkl`

## 2️⃣ Langkah 2 — Coba jalankan lokal (opsional, tapi disarankan)

```bash
cd skinapp
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
streamlit run app.py
```

Buka `http://localhost:8501` di browser. Coba upload foto / pakai kamera
untuk memastikan semuanya berjalan sebelum deploy online.

## 3️⃣ Langkah 3 — Deploy ke Streamlit Community Cloud (gratis)

1. **Buat repo GitHub baru** (boleh public/private), lalu upload seluruh
   isi folder `skinapp/` ke repo tersebut (termasuk folder `saved_model/`
   yang sudah berisi file `.pkl`).
   - Catatan: pastikan ukuran total file model tidak terlalu besar
     (umumnya model SVM + scaler hanya beberapa MB, aman untuk GitHub).
   - Kalau pakai Git dari terminal:
     ```bash
     cd skinapp
     git init
     git add .
     git commit -m "Deploy klasifikasi kulit wajah"
     git branch -M main
     git remote add origin https://github.com/USERNAME/NAMA-REPO.git
     git push -u origin main
     ```

2. Buka **[share.streamlit.io](https://share.streamlit.io)** lalu login
   dengan akun GitHub kamu.

3. Klik **"New app"** →
   - **Repository**: pilih repo yang baru dibuat
   - **Branch**: `main`
   - **Main file path**: `app.py`

4. Klik **"Deploy"**. Tunggu beberapa menit sampai proses build selesai
   (menginstall `requirements.txt`).

5. Selesai! Kamu akan mendapat link publik seperti:
   `https://nama-app-kamu.streamlit.app`
   — link ini bisa dibuka di HP/laptop manapun, termasuk untuk **scan
   foto langsung dari kamera HP** (browser akan minta izin akses kamera).

## 🎨 Tampilan

- Warna utama mengikuti poster: **maroon** (`#8c1c2b`), **pink lembut**
  (`#e8a0ab`), dan **cream** (`#fdf6f3`) sebagai latar.
- Ada 2 tab input: **Upload Foto** dan **Scan Langsung (Kamera)** — yang
  kedua memakai `st.camera_input`, jadi otomatis berfungsi sebagai "live
  scan" baik di desktop (webcam) maupun HP (kamera depan/belakang).
- Hasil prediksi ditampilkan dengan kartu besar + bar confidence score
  untuk setiap dari 6 kelas, mengikuti gaya visual notebook aslinya.

## ⚠️ Penting — Konsistensi Pipeline Fitur

File `features.py` di aplikasi ini **disalin persis** dari fungsi
`preprocess_image`, `extract_color_features`, `extract_texture_features`,
dan `extract_features_pipeline` pada notebook training kamu (resize
224×224, fitur warna 36 dimensi, fitur tekstur 14 dimensi = total 50
dimensi). Jangan diubah parameternya, karena harus identik dengan saat
training agar `scaler` dan `model_svm` bekerja dengan benar.

Jika nanti kamu retrain ulang model dengan pipeline fitur yang berbeda
(misal ukuran gambar diganti, jumlah bin histogram diganti, dll), kamu
harus update `features.py` di aplikasi ini juga, supaya tetap cocok
dengan model barunya.

## 🩺 Disclaimer

Aplikasi ini dibuat untuk keperluan **akademik/penelitian** (tugas
klasifikasi citra), bukan alat diagnosis medis resmi. Hasil prediksi
tidak menggantikan konsultasi dengan dokter kulit / dermatolog.
