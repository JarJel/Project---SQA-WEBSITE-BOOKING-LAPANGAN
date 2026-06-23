# Teknik Pengujian Grey Box

Dokumen ini menjelaskan tinjauan teoritis dan taktis mengenai teknik-teknik pengujian Grey Box yang diterapkan pada proyek penjaminan mutu sistem website booking lapangan olahraga.

---

## 1. Konsep Grey Box Testing

Grey Box Testing adalah kombinasi dari Black Box Testing dan White Box Testing. Penguji tidak memiliki akses penuh ke kode sumber secara menyeluruh (tidak menguji per baris kode fungsi internal), namun memiliki akses ke komponen arsitektural internal seperti diagram database, file konfigurasi, API specification, logs, dan dapat menjalankan query SQL untuk memvalidasi keadaan sistem.

---

## 2. Rincian Teknik yang Diterapkan di Proyek Ini

Berikut adalah detail teknik pengujian Grey Box yang terdokumentasi di folder ini:

### 1. [Pattern Testing (Pengujian Pola)](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Grey_Box_Testing/Pattern%20Testing.md)
*   Memeriksa konsistensi pola respon data API (seperti format JSON error dan sukses) serta pola rekaman log aktivitas keamanan guna mendeteksi ancaman brute force atau akses ilegal.

### 2. [Matrix Testing (Pengujian Matriks)](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Grey_Box_Testing/Matrix_Testing.md)
*   Menganalisis hubungan kebutuhan fungsional dengan skema tabel database melalui matriks CRUD (*Create, Read, Update, Delete*) dan matriks ketertelusuran persyaratan (*Requirements Traceability Matrix*).

### 3. [Orthogonal Array Testing (OATS)](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Grey_Box_Testing/Orthogonal_Array_Testing.md)
*   Menggunakan rancangan statistik larik ortogonal untuk mengoptimalkan pengujian kombinatorial variabel input (seperti kombinasi jenis lapangan, metode pembayaran, dan tipe member) guna meminimalkan jumlah skenario pengujian dengan cakupan maksimal.

### 4. [Regression Testing (Pengujian Regresi)](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Grey_Box_Testing/Regression_Testing.md)
*   Menguji ulang sistem setelah modifikasi kode atau database migration untuk memastikan fungsi-fungsi lama tidak rusak dan relasi database tetap terintegrasi secara aman.
