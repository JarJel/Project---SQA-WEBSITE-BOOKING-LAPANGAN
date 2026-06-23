# Panduan Teknik Pengujian Black Box

Dokumen ini memberikan panduan komprehensif mengenai penerapan berbagai teknik pengujian Black Box pada sistem [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan). Pengujian Black Box dilakukan untuk memverifikasi fungsionalitas sistem dari luar tanpa melihat atau memodifikasi kode sumber secara langsung.

---

## 1. Strategi Pengujian Fungsional

Strategi pengujian Black Box pada proyek ini dibagi ke dalam 10 taktik pengujian yang terstruktur dan terukur guna memastikan tidak ada celah logika bisnis dari sisi pengguna:

1.  **[Boundary Value Analysis (BVA)](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/Boundary%20Value%20Analysis%20(BVA).md)**
    *   *Tujuan:* Menguji kestabilan sistem pada batas minimum dan maksimum operasional, seperti batas tanggal hari ini dan jam operasional gedung (07:00 hingga 22:00).
2.  **[Equivalence Partitioning](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/Equivalence%20Partitioning.md)**
    *   *Tujuan:* Membagi data masukan ke dalam kelas data valid dan invalid (contoh: status lapangan, format waktu, durasi sewa, format email) untuk meminimalkan redundansi kasus uji.
3.  **[Decision Table Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/Decision%20Table%20Testing.md)**
    *   *Tujuan:* Memetakan kombinasi logika masukan biner yang kompleks dari aturan bisnis sistem ke dalam matriks aksi yang rapi.
4.  **[Cause-Effect Relationship](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/CAUSE-EFFECT%20RELATIONSHIP.md)**
    *   *Tujuan:* Menganalisis alur sebab-akibat boolean yang terjadi antara parameter input dengan keluaran sistem berupa pesan kesalahan atau penyimpanan database.
5.  **[Behaviour Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/BEHAVIOUR%20TESTING.md)**
    *   *Tujuan:* Melacak siklus hidup transisi status transaksi booking (`pending`, `approved`, `rejected`, `cancelled`, `expired`) dan memastikan tidak ada transisi ilegal.
6.  **[Comparison Table Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/COMPARISON%20TABLE%20TESTING.md)**
    *   *Tujuan:* Membandingkan parameter input dengan record database guna memverifikasi algoritma irisan jadwal (*overlap prevention*), serta menguji konsistensi rendering layout pada berbagai web browser.
7.  **[Robustness Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/Robustness%20Testing.md)**
    *   *Tujuan:* Menguji daya tahan sistem terhadap serangan siber dasar (SQL Injection, CSRF, Mass-Assignment) dan masukan abnormal (file gambar bukti transfer > 2MB).
8.  **[Performance Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/Performance%20testing.md)**
    *   *Tujuan:* Mengukur waktu respon halaman utama dan pemesanan API di bawah beban kerja bertahap menggunakan skrip **k6**.
9.  **[Endurance Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/Endurance%20Testing.md)**
    *   *Tujuan:* Memantau stabilitas memori RAM server web PHP dan ketersediaan koneksi database MySQL selama 6 jam simulasi transaksi non-stop.
10. **[Sample Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/SAMPLE%20TESTING.md)**
    *   *Tujuan:* Menyeleksi sampel data pengujian yang representatif (peran pengguna, status lapangan, tipe waktu) untuk efisiensi eksekusi pengujian berkala.
