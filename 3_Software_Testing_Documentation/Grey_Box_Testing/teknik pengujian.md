# Panduan Teknik Pengujian Grey Box

Dokumen ini memberikan panduan komprehensif mengenai penerapan berbagai teknik pengujian Grey Box pada sistem [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan). Pengujian Grey Box menggabungkan aspek fungsional luar (Black Box) dengan pengetahuan parsial arsitektur data internal (White Box) seperti skema database, kueri SQL, skema API, dan file log server.

---

## 1. Strategi Pengujian Integrasi Data

Strategi pengujian Grey Box difokuskan pada lima taktik utama yang didokumentasikan di folder ini:

1.  **[Pattern Testing (Pengujian Pola)](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Grey_Box_Testing/Pattern%20Testing.md)**
    *   *Tujuan:* Memvalidasi pola respon JSON API terstandarisasi untuk keseragaman integrasi frontend-backend, serta menganalisis pola ekspresi reguler (regex) log server untuk memantau keamanan dan error handling.
2.  **[Matrix Testing (Pengujian Matriks)](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Grey_Box_Testing/Matrix_Testing.md)**
    *   *Tujuan:* Menyusun matriks ketertelusuran persyaratan (*Requirements Traceability Matrix*) untuk menjamin semua fungsi SRS terwakili di database, serta memetakan pengujian CRUD database untuk setiap operasi tabel relasional.
3.  **[Orthogonal Array Testing (OATS)](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Grey_Box_Testing/Orthogonal_Array_Testing.md)**
    *   *Tujuan:* Menerapkan rancangan larik ortogonal $L_9(3^3)$ untuk memotong jumlah kombinasi masukan (jenis lapangan, cara bayar, tipe pengguna) secara signifikan namun tetap menjaga efektivitas pengujian interaksi berpasangan (*pairwise*).
4.  **[Regression Testing (Pengujian Regresi)](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Grey_Box_Testing/Regression_Testing.md)**
    *   *Tujuan:* Melakukan pengujian ulang pasca migrasi basis data atau perbaikan bug logika (seperti double-booking) guna memastikan tidak ada fungsi lama yang terganggu dan database tetap konsisten.
5.  **[Test Plan.md (Rencana Pengujian Grey Box)](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Grey_Box_Testing/Test%20Plan.md)**
    *   *Tujuan:* Mendefinisikan rencana kerja pengujian terpadu, batas penangguhan pengujian, dan target kelulusan (Exit Criteria) yang disepakati oleh tim pengembang dan QA.
