# Sample Testing (Pengujian Sampel & Uji Acak)

Dokumen ini mendokumentasikan skenario pemilihan sampel (*Sample Testing*) dan pengujian acak (*Ad-hoc/Random Testing*) untuk fitur **Pemesanan Lapangan (`store` method)** pada aplikasi [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan).

---

## 1. Pendekatan Pengujian Sampel

Karena banyaknya kemungkinan input variatif pada sistem, penguji memilih sampel representatif untuk mencakup seluruh skenario operasional secara hemat dan terukur. Pemilihan sampel dibagi menjadi 4 kelompok utama:

1.  **Sampel Data Pengguna (Users):**
    *   Pengguna dengan peran `'admin'` (untuk menguji bypass hak akses dan halaman admin).
    *   Pengguna dengan peran `'user'` (untuk menguji alur sewa standar).
2.  **Sampel Status Lapangan (Fields):**
    *   Lapangan Futsal A (Status: `'active'`, harga per jam: `150,000`).
    *   Lapangan Badminton 1 (Status: `'maintenance'`, harga per jam: `50,000`).
3.  **Sampel Waktu & Jam Sewa (Timings):**
    *   Hari kerja (Weekday) vs Hari libur (Weekend).
    *   Jam operasional normal pagi (`08:00 - 10:00`) vs sore/malam (`19:00 - 21:00`).
4.  **Sampel Status Pembayaran (Payments):**
    *   Pembayaran tepat nominal (valid).
    *   Pembayaran kurang nominal / bukti buram (invalid).

---

## 2. Tabel Matriks Kasus Uji Sampel (Sample Test Matrix)

| ID Uji | Peran Pengguna | ID Lapangan (Status) | Pilihan Tanggal & Jam | Nilai Input Bukti Transfer | Hasil yang Diharapkan | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :---: |
| **TC-SMP-001** | `user` (Budi) | ID: 1 (`active`) | Besok, `08:00 - 10:00` | Gambar JPG valid (1.2 MB) | Pemesanan sukses status `pending` ➔ diunggah bukti ➔ disetujui admin ➔ status `approved`. | Pass |
| **TC-SMP-002** | `user` (Budi) | ID: 1 (`active`) | Besok, `09:00 - 11:00` | - | **Ditolak (Error Overlap)**<br>Slot jam 09:00 - 10:00 sudah dipesan di TC-SMP-001. | Pass |
| **TC-SMP-003** | `user` (Budi) | ID: 2 (`maintenance`) | Besok, `10:00 - 12:00` | - | **Ditolak (Error Lapangan)**<br>Muncul pesan "Lapangan sedang tidak aktif". | Pass |
| **TC-SMP-004** | `admin` (Rudi) | ID: 1 (`active`) | Besok, `07:00 - 08:00` | - | **Diterima**<br>Admin dapat melakukan simulasi sewa secara langsung. | Pass |
| **TC-SMP-005** | `user` (Budi) | ID: 1 (`active`) | Hari ini, `21:00 - 23:00` | - | **Ditolak (Error Jam Operasional)**<br>Jam selesai sewa melebihi batas 22:00. | Pass |
| **TC-SMP-006** | `user` (Budi) | ID: 1 (`active`) | Hari ini, `15:00 - 16:00` | Gambar PDF (invalid) | Booking pending terbuat ➔ upload bukti gagal karena format bukan gambar. | Pass |
