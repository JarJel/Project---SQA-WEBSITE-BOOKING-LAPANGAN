# Formal Inspection (Inspeksi Formal)

Inspeksi Formal (dikenal sebagai *Fagan Inspection*) adalah proses peninjauan kode program secara terstruktur dan formal untuk mengidentifikasi kesalahan logika, kecacatan performa, atau ketidakpatuhan terhadap arsitektur sebelum perangkat lunak dipindahkan ke tahap rilis produksi.

---

## 1. Alur Proses Inspeksi Formal

Proses inspeksi mengikuti 6 tahapan terstruktur:

```text
[Planning] ➔ [Overview] ➔ [Preparation] ➔ [Inspection Meeting] ➔ [Rework] ➔ [Follow-up]
```

1.  **Planning (Perencanaan):** Moderator menentukan jadwal, mengumpulkan dokumen prasyarat (SRS/SDD), dan menentukan tim inspeksi.
2.  **Overview (Gambaran Umum):** Penulis kode memberikan pengantar arsitektur kode kepada tim inspeksi.
3.  **Preparation (Persiapan):** Setiap anggota tim mempelajari kode secara mandiri untuk menemukan potensi masalah.
4.  **Inspection Meeting (Pertemuan):** Tim berkumpul untuk menelusuri kode secara detail, mendaftarkan cacat logika (*defects*).
5.  **Rework (Perbaikan):** Penulis melakukan perbaikan kode berdasarkan daftar masalah yang ditemukan.
6.  **Follow-up (Tindak Lanjut):** Moderator memverifikasi bahwa seluruh perbaikan telah diuji dan disetujui.

---

## 2. Formulir Rapat Inspeksi Formal (Inspection Log)

**ID Proyek:** [ID Proyek Anda]  
**Tanggal Rapat:** 24 Juni 2026  
**Dokumen Kode:** `app/Http/Controllers/PaymentController.php`  

### Hasil Penilaian Inspeksi:
*   **Total Cacat Ditemukan:** 3
*   **Tingkat Keparahan (Severity):** 1 Major, 2 Minor
*   **Keputusan:** [ ] Lulus Tanpa Syarat | [x] Lulus Setelah Rework | [ ] Ulangi Rapat Inspeksi

### Rincian Temuan Inspeksi:
| No | Lokasi Kode | Deskripsi Cacat | Kategori (Major/Minor) | Status |
| :--- | :--- | :--- | :---: | :---: |
| 1 | Baris 72 | Nilai kembalian status pembayaran dari pihak ketiga tidak divalidasi tanda tangannya (signature key validation missing). | Major | Resolved |
| 2 | Baris 140 | Logging exception tidak mencatat ID Booking yang bersangkutan. | Minor | Resolved |
| 3 | Baris 190 | Terdapat kode mati (*dead code*) yang tidak pernah dieksekusi. | Minor | Resolved |
