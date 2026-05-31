# Pipeline Pengolahan Citra: Ekstraksi Fitur Koin
Repositori berikut berisi pipeline pengolahan citra lengkap yang dibuat untuk mendeteksi tepi, menerapkan operasi morfologi, dan mengekstrak berbagai fitur dari gambar koin. Proyek ini dibuat untuk memenuhi **Tugas Sesi #12 pada Mata Kuliah Pengolahan Citra**.

---

## Deskripsi & Alasan Pemilihan Citra

* **Sumber Citra:** Gambar sekunder yang diambil dari Google Images.
* **Alasan Pemilihan Objek:**
  1. **Bentuknya Konsisten:** Objek koin memiliki bentuk lingkaran sempurna yang stabil, sehingga sangat cocok untuk menguji fitur bentuk seperti mencari kontur luar, luas, dan keliling objek.
  2. **Teksturnya Jelas:** Permukaan koin memiliki detail corak dan ukiran yang jelas, sehingga sangat cocok untuk analisis tekstur menggunakan metode GLCM (*Contrast* & *Homogeneity*).
  3. **Batas Tepi yang Tegas:** Perbedaan warna yang kontras antara koin dan background membuat proses pemisahan gambar (binerisasi) menjadi lebih mudah, sekaligus mempermudah kita melihat perbedaan hasil deteksi tepi Sobel dan Canny.

---

## Tahapan Pipeline & Hasil Visualisasi

Seluruh program ini dibuat menggunakan bahasa Python dengan library utama `OpenCV`, `NumPy`, `Matplotlib`, dan `Scikit-Image`. Berikut adalah penjelasan singkat dari tiap prosesnya:

### 1. Deteksi Tepi (Edge Detection)
* **Sobel:** Berfungsi untuk mencari garis tepi dengan cara menghitung perubahan kecerahan warna pada arah vertikal dan horizontal.
* **Canny:** Menggunakan beberapa tahapan filter untuk menyaring noise sehingga menghasilkan garis tepi objek yang lebih rapi, tipis, dan bersih dibanding Sobel.

**Visualisasi Deteksi Tepi:**
> <img width="829" height="406" alt="Screenshot 2026-05-31 122440" src="https://github.com/user-attachments/assets/3ebdc636-365f-4d85-b01a-8cf58a780e9f" />

### 2. Operasi Morfologi (Morphological Operations)
* Sebelum masuk ke tahap morfologi, gambar diubah dulu menjadi hitam-putih (citra biner) menggunakan metode *inverse threshold* (`THRESH_BINARY_INV`) biar objek koin terpisah dari background-nya.
* Menggunakan acuan kotak ukuran $5 \times 5$, kita menerapkan operasi **Erosion, Dilation, Opening, dan Closing** untuk membersihkan bintik-bintik kotor di luar koin dan menutup lubang kosong di dalam koin akibat pantulan cahaya.

**Visualisasi Operasi Morfologi:**
> <img width="639" height="656" alt="Screenshot 2026-05-31 122441" src="https://github.com/user-attachments/assets/c117befa-a350-4467-a70a-f6e82653aa5f" />

### 3. Ekstraksi Fitur (Feature Extraction)
* **Fitur Intensitas:** Melihat persebaran warna gambar lewat grafik Histogram dan menghitung nilai rata-rata (*Mean Intensity*) untuk tahu seberapa cerah gambar tersebut.
* **Fitur Tekstur (GLCM):** Menggunakan metode GLCM untuk mencari nilai *Contrast* (untuk tahu seberapa tajam perubahan tekstur permukaan koin) dan *Homogeneity* (untuk tahu seberapa halus/rata permukaannya).
* **Fitur Bentuk (Contour):** Mengambil garis kontur paling luar dan paling besar untuk menghitung nilai geometris berupa *Area* (luas koin dalam hitungan piksel) dan *Perimeter* (keliling luar koin).

**Visualisasi Histogram Intensitas:**
> <img width="502" height="313" alt="Screenshot 2026-05-31 122442" src="https://github.com/user-attachments/assets/a119fd9b-2a05-4b0f-a33d-e47ed2e72b7b" />

## Tabel Ringkasan Hasil Ekstraksi Fitur

Berikut adalah tabel ringkasan nilai fitur yang berhasil didapatkan dari objek koin setelah melewati semua proses pengolahan citra:

| Kategori Fitur | Metrik Ekstraksi | Deskripsi Hasil |
| :--- | :--- | :--- |
| **Intensitas** | Mean Intensity | Mengukur rata-rata tingkat kecerahan keseluruhan gambar. |
| **Tekstur (GLCM)** | Contrast | Menunjukkan seberapa tajam atau kasar perubahan tekstur pada permukaan koin. |
| **Tekstur (GLCM)** | Homogeneity | Mengukur seberapa halus atau seragam tekstur permukaan objek. |
| **Bentuk (Kontur)**| Contour Area | Menghitung total jumlah piksel yang ada di dalam lingkaran koin. |
| **Bentuk (Kontur)**| Contour Perimeter | Menghitung total panjang garis luar yang mengelilingi koin. |

**Tabel Output Program:**
> <img width="320" height="131" alt="Screenshot 2026-05-31 122443" src="https://github.com/user-attachments/assets/69468788-09e9-4c97-b5e9-84635a13f62a" />


## 🛠️ Panduan Menjalankan Program

1. Buka file dokumen `.ipynb` melalui platform **Google Colab** atau **Jupyter Notebook**.
2. Jalankan (*Run*) setiap *cell* kode program secara berurutan dari atas ke bawah.
3. Ketika dialog unggahan muncul, masukkan file gambar koin yang telah disiapkan (`koin.jpg`).
4. Program secara otomatis akan memproses citra, menampilkan seluruh grafik visualisasi, dan memunculkan tabel ringkasan nilai fitur di bagian akhir.
