# Test Plan (Rencana Pengujian)
## Mengacu pada Standar IEEE 829

**Nama Proyek:** [Nama Proyek Anda]  
**Versi Dokumen:** 1.0  
**Tanggal Terbit:** 23 Juni 2026  
**Penyusun:** [Nama QA Lead]  

---

## 1. Pendahuluan
Rencana Pengujian (*Test Plan*) ini disusun untuk mendefinisikan ruang lingkup, pendekatan, sumber daya, jadwal, serta manajemen risiko dari aktivitas pengujian perangkat lunak untuk [Nama Aplikasi/Sistem]. Dokumen ini memastikan tim QA dan pengembang menyepakati kriteria kualitas produk sebelum rilis.

## 2. Item Pengujian (Test Items)
Perangkat lunak yang akan diuji mencakup seluruh komponen sistem [Nama Aplikasi/Sistem] pada build terbaru:
*   Aplikasi Web Frontend (React/Vue/HTML/CSS/JS)
*   Application Programming Interface (API) Server
*   Basis Data (MySQL/PostgreSQL)

## 3. Fitur yang Akan Diuji (Features to be Tested)
Berikut adalah fitur-fitur fungsional utama yang masuk dalam ruang lingkup pengujian:
1.  **Autentikasi & Autorisasi:** Registrasi pengguna, login, logout, lupa password, pembatasan akses role.
2.  **Manajemen Lapangan:** Tambah, edit, hapus data lapangan oleh admin.
3.  **Pemesanan & Jadwal:** Pilihan slot tanggal/jam, validasi bentrok jadwal, perhitungan biaya.
4.  **Notifikasi:** Email konfirmasi pemesanan setelah disetujui.

## 4. Fitur yang Tidak Akan Diuji (Features Not to be Tested)
Fitur yang berada di luar cakupan pengujian siklus ini:
1.  **Keamanan Tingkat Tinggi (Penetration Testing):** Pengujian penetrasi server (DDoS, brute-force server level) tidak dilakukan pada tahap ini.
2.  **Integrasi Payment Gateway:** Simulasi pembayaran menggunakan sandbox/mocking, tidak menggunakan uang asli dan server bank asli.
3.  **Kompabilitas Browser Kuno:** Pengujian tidak mencakup Internet Explorer atau browser versi lama di bawah tahun 2020.

## 5. Pendekatan / Strategi Pengujian (Test Approach)
Aktivitas pengujian akan menggunakan kombinasi beberapa metode berikut:
*   **Black Box Testing:** Menguji fungsionalitas sistem berdasarkan input dan output tanpa melihat kode sumber.
*   **System Testing:** Menguji sistem secara keseluruhan dari alur awal (end-to-end) hingga akhir.
*   **Regression Testing:** Pengujian ulang setelah adanya perbaikan bug untuk memastikan fitur lain tidak terganggu.
*   **User Acceptance Testing (UAT):** Pengujian akhir bersama pengguna / klien untuk memvalidasi kesesuaian kebutuhan bisnis.

## 6. Kriteria Kelulusan (Pass/Fail Criteria)
Sistem dinyatakan lulus pengujian dan siap rilis jika memenuhi kriteria berikut:
*   100% kasus uji berkategori *High/Critical* berstatus **Pass**.
*   Minimal 90% dari keseluruhan kasus uji berstatus **Pass**.
*   Tidak ada bug aktif berkategori *Severity: Critical* atau *High*.

## 7. Kriteria Penangguhan & Kelanjutan (Suspension & Resumption Criteria)
*   **Kriteria Penangguhan:** Pengujian akan ditangguhkan jika terdapat bug kritis yang memblokir alur utama (misal: sistem sama sekali tidak bisa login atau database crash), yang mengakibatkan pengujian fitur lainnya tidak dapat dilanjutkan.
*   **Kriteria Kelanjutan:** Pengujian akan dilanjutkan setelah tim pengembang menyerahkan build perbaikan terbaru yang mengatasi masalah pemblokir tersebut.

## 8. Deliverables Pengujian (Test Deliverables)
Dokumen yang akan diserahkan oleh tim pengujian setelah siklus uji selesai:
*   Dokumen Rencana Pengujian (Test Plan - *dokumen ini*)
*   Spesifikasi Kasus Uji (Test Cases)
*   Laporan Hasil Eksekusi Uji (Test Execution Report / Bug Log)
*   Dokumen UAT Sign-off dari pengguna/klien

## 9. Kebutuhan Lingkungan (Environmental Needs)
*   **Lingkungan Pengujian (Testing Environment):** Server Staging terpisah dari server produksi. URL: `https://staging.domain.com`.
*   **Perangkat Keras:** PC Penguji & Smartphone Android/iOS untuk pengujian tampilan mobile.
*   **Alat Bantu Pengujian:** Chrome DevTools, Postman (untuk uji API), dan Jira/Trello untuk pelaporan bug.

## 10. Jadwal Pengujian (Schedule)
| Aktivitas | Durasi | Estimasi Tanggal Mulai | Estimasi Tanggal Selesai |
| :--- | :---: | :---: | :---: |
| Pembuatan Test Plan & Test Cases | 3 Hari | 24 Juni 2026 | 26 Juni 2026 |
| Eksekusi Pengujian Siklus 1 | 4 Hari | 29 Juni 2026 | 02 Juli 2026 |
| Perbaikan Bug (oleh Dev) | 3 Hari | 03 Juli 2026 | 05 Juli 2026 |
| Regression Testing (Siklus 2) | 2 Hari | 06 Juli 2026 | 07 Juli 2026 |
| User Acceptance Testing (UAT) | 2 Hari | 08 Juli 2026 | 09 Juli 2026 |

## 11. Risiko dan Rencana Kontinjensi
*   **Risiko:** Keterlambatan penyerahan build aplikasi dari tim pengembang.
    *   *Rencana Kontinjensi:* Menambahkan durasi kerja lembur atau memprioritaskan hanya pengujian fitur kritis (*Critical path testing*).
*   **Risiko:** Perubahan kebutuhan (requirements change) di tengah proses pengujian.
    *   *Rencana Kontinjensi:* Melakukan revisi cepat terhadap dokumen Test Case dan memperbarui jadwal rilis.

## 12. Persetujuan (Approvals)
Dokumen ini disetujui oleh perwakilan masing-masing tim:

| Peran | Nama | Tanda Tangan / Status | Tanggal |
| :--- | :--- | :---: | :---: |
| **QA Lead** | [Nama QA Lead] | [ ] Belum Disetujui | |
| **Project Manager** | [Nama PM] | [ ] Belum Disetujui | |
| **Lead Developer** | [Nama Lead Dev] | [ ] Belum Disetujui | |
