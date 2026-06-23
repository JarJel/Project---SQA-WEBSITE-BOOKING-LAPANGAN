# Teknik Pengujian Black Box

Dokumen ini memberikan ikhtisar mengenai berbagai teknik pengujian Black Box yang diterapkan pada proyek penjaminan mutu sistem website booking lapangan olahraga.

---

## 1. Pengantar Black Box Testing

Pengujian Black Box adalah metode pengujian perangkat lunak di mana fungsionalitas aplikasi diuji tanpa memiliki pengetahuan tentang struktur internal kode, rincian implementasi, atau jalur internal program. Pengujian berfokus sepenuhnya pada input yang diberikan dan output yang dihasilkan.

---

## 2. Daftar Teknik yang Diterapkan

Proyek ini mendokumentasikan dan menerapkan teknik-teknik pengujian berikut di dalam folder **Black Box Testing**:

1.  **[Boundary Value Analysis (BVA)](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/Boundary%20Value%20Analysis%20(BVA).md)**
    *   Menguji batas nilai minimum dan maksimum dari input seperti durasi pemesanan dan panjang karakter nama pengguna.
2.  **[Equivalence Partitioning](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/Equivalence%20Partitioning.md)**
    *   Membagi data input menjadi kelompok valid dan invalid (misalnya, nomor handphone dan email) untuk efisiensi kasus uji.
3.  **[Decision Table Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/Decision%20Table%20Testing.md)**
    *   Memetakan logika bisnis yang kompleks (seperti status otorisasi user, ketersediaan slot, dan status transfer pembayaran) ke dalam tabel keputusan biner.
4.  **[Cause-Effect Relationship](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/CAUSE-EFFECT%20RELATIONSHIP.md)**
    *   Menganalisis hubungan sebab-akibat input sistem terhadap perilaku transisi status transaksi.
5.  **[Behaviour Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/BEHAVIOUR%20TESTING.md)**
    *   Menguji alur kerja pengguna berdasarkan siklus hidup data pemesanan dari awal dibuat hingga selesai digunakan.
6.  **[Comparison Table Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/COMPARISON%20TABLE%20TESTING.md)**
    *   Pengujian kompatibilitas visual dan fungsional di berbagai sistem operasi dan peramban web (browser).
7.  **[Robustness Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/Robustness%20Testing.md)**
    *   Menguji ketangguhan sistem dalam menangani masukan file yang tidak didukung atau parameter URL palsu.
8.  **[Performance Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/Performance%20testing.md)**
    *   Pengukuran kinerja sistem (load testing) di bawah tekanan beban pengguna simultan.
9.  **[Endurance Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/Endurance%20Testing.md)**
    *   Pengujian stabilitas jangka panjang sistem untuk memantau kebocoran memori.
10. **[Sample Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/SAMPLE%20TESTING.md)**
    *   Memilih representasi data uji yang mencakup kondisi pencarian umum.
