# Proyek UAS — OCR Plat Nomor

*Integrasi Visual Language Model (VLM) dan Python untuk Mengenali Teks Otomatis*

---

### Tentang Proyek Ini

Proyek ini dibuat buat tugas Ujian Akhir Semester (UAS) mata kuliah Komputer Vision. Tujuannya adalah untuk membaca plat nomor kendaraan dari gambar secara otomatis menggunakan teknologi OCR (Optical Character Recognition) yang dipadukan dengan Visual Language Model dari LLM Studio.

Nantinya, hasil bacaan plat nomor ini akan kita cek akurasinya dengan menghitung berapa banyak kesalahan karakter yang terjadi. Metode yang dipakai namanya Character Error Rate (CER).

---

### Struktur Folder Proyek

Folder utama bernama UAS/ dan di dalamnya ada beberapa folder dan file seperti ini:

* test/

  * image/ → tempat foto-foto plat nomor dalam format .jpg
  * label/ → file .txt yang berisi tulisan plat nomor sebenarnya (ground truth)
* generate_ground_truth.py → script untuk mengubah data label jadi file CSV
* main.py → script utama untuk menjalankan proses deteksi dan evaluasi
* ground_truth.csv → hasil gabungan label yang sudah dirapikan
* results.csv → hasil akhir prediksi beserta nilai akurasinya

---

### Cara Menjalankan Proyek Ini

1. *Salin Dataset*

   Ambil folder test dari dataset "Indonesian License Plate Recognition" yang dikasih sama dosen, terus taruh folder itu di dalam folder UAS/. Di dalam test/ ada gambar plat nomor dan file teks labelnya.

2. *Buat File Ground Truth*

   Jalankan script ini untuk gabungin label .txt dan nama file gambar jadi satu file CSV yang bisa dipakai untuk evaluasi:

   
   python generate_ground_truth.py
   

3. *Load Model di LLM Studio*

   Pastikan model llava-v1.6-34b sudah dimuat di LLM Studio. Cek dengan perintah:

   
   lms ls
   

   Kalau model sudah muncul, jalankan servernya:

   
   lms start server
   

   Server akan aktif di alamat: [http://localhost:1234/v1/chat/completions](http://localhost:1234/v1/chat/completions)

4. *Jalankan Program Utama*

   Proses inferensi dan evaluasi otomatis bisa dijalankan dengan perintah:

   
   python main.py
   

   Program ini akan:

   * Membaca gambar plat nomor dari folder test/image
   * Mengirim gambar ke model untuk dibaca teksnya (OCR)
   * Menghitung nilai kesalahan (CER) tiap gambar
   * Menyimpan hasil prediksi dan evaluasi ke file results.csv

5. *Lihat Hasil Akhir*

   File results.csv berisi hasil prediksi dari model, label asli, rumus perhitungan error, dan skor error-nya. Contohnya seperti ini:

   | image          | ground\_truth | prediction | CER\_formula          | CER\_score |
   | -------------- | ------------- | ---------- | --------------------- | ---------- |
   | test001\_1.jpg | B9140BCD      | 9140       | CER = (0 + 4 + 0) / 8 | 0.5        |
   | test001\_2.jpg | B2407UZO      | B2407UZ    | CER = (0 + 1 + 0) / 8 | 0.125      |

   *Penjelasan:*

   * CER_formula menunjukkan cara hitung kesalahan karakter (jumlah substitusi, hapus, dan tambah dibagi panjang teks).
   * CER_score makin kecil berarti makin akurat hasil OCR-nya.
   * File ini penting untuk menilai performa model secara keseluruhan.
