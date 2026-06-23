# Software Testing Documentation
## Laporan Hasil Pengujian (Test Execution Report)

**Nama Proyek:** [Nama Proyek Anda]  
**Tanggal Pengujian:** [Tanggal Pengujian]  
**Penguji:** [Nama Penguji/QA]  
**Lingkungan Uji (Environment):** Staging / Local Dev (OS Windows, Chrome v120)

---

## 1. Ringkasan Eksekusi Pengujian (Executive Summary)

Laporan ini menyajikan hasil eksekusi kasus uji yang telah dirancang sebelumnya pada dokumen *Test Cases*. Tujuan laporan ini adalah memberikan visibilitas status kualitas perangkat lunak saat ini sebelum proses rilis.

### Ringkasan Statistik
*   **Total Kasus Uji:** 9
*   **Berhasil (Pass):** 7
*   **Gagal (Fail):** 2
*   **Belum Dieksekusi (Block/Not Run):** 0
*   **Tingkat Kelulusan (Pass Rate):** 77.7%

```text
[████████████████████████████████████████          ] 77.7% Pass
```

---

## 2. Rincian Hasil Eksekusi

| ID Kasus Uji | Skenario Uji | Status (Pass/Fail) | Catatan / Temuan Bug |
| :--- | :--- | :---: | :--- |
| **TC-AUTH-001** | Pendaftaran Akun Berhasil | **Pass** | Berhasil membuat user baru di DB. |
| **TC-AUTH-002** | Pendaftaran Akun Gagal - Format Email | **Pass** | Validasi frontend & backend bekerja. |
| **TC-AUTH-003** | Login Berhasil | **Pass** | Token JWT/Session tersimpan dengan benar. |
| **TC-BOOK-001** | Pemesanan Lapangan (Slot Tersedia) | **Pass** | Transaksi terbuat dengan status 'Pending'.|
| **TC-BOOK-002** | Pemesanan Lapangan (Double Booking) | **Fail** | **Bug #001**: Sistem membolehkan pemesanan double pada jam yang bentrok. |
| **TC-ADM-001** | Admin Menyetujui Booking (Approve) | **Pass** | Status booking berubah jadi 'Approved'. |
| ... | ... | ... | ... |

---

## 3. Daftar Bug / Temuan Masalah (Defect Log)

Berikut adalah detail bug yang ditemukan selama eksekusi pengujian:

### Bug #001: Terjadi Tumpang Tindih Pemesanan (Double Booking)
*   **Tingkat Keparahan (Severity):** High / Critical
*   **Prioritas (Priority):** High
*   **Deskripsi:** Sistem membolehkan dua pengguna yang berbeda memesan Lapangan A pada tanggal dan jam yang sama persis, tanpa memunculkan pesan kesalahan.
*   **Langkah Reproduksi:**
    1. Login dengan Akun A, pesan Lapangan A tanggal 25 Juni jam 17:00-18:00. Approve pemesanan.
    2. Login dengan Akun B, pesan Lapangan A pada tanggal dan jam yang sama.
    3. Klik "Pesan Sekarang".
*   **Hasil Aktual:** Pemesanan Akun B tetap tersimpan dan statusnya juga disetujui tanpa peringatan bentrok.
*   **Hasil yang Diharapkan:** Pemesanan Akun B diblokir dan muncul pesan error.

---

## 4. Kesimpulan dan Rekomendasi
Perangkat lunak **Belum Siap Dirilis** ke lingkungan produksi (Production) karena masih terdapat bug berkategori *High/Critical* terkait dengan logika pemesanan tumpang tindih. Pengembang harus memperbaiki **Bug #001** sebelum pengujian siklus berikutnya (Regression Testing) dimulai.
