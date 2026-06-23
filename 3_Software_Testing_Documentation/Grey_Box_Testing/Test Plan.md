# Grey Box Test Plan (Rencana Pengujian Grey Box)

Dokumen ini mendefinisikan rencana pengujian (*Test Plan*) tingkat Grey Box untuk memvalidasi integrasi data, REST API, dan basis data pada sistem [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan).

---

## 1. Ruang Lingkup Pengujian Grey Box

Pengujian difokuskan pada area abu-abu di mana data ditransfer dari antarmuka pengguna (Frontend) melalui logika API (Backend) dan disimpan secara persisten di database (DBMS MySQL).

### A. Komponen yang Diuji:
1.  **REST API Endpoints:** Menguji input validasi HTTP status, JSON schema response, header, dan handling exception Laravel.
2.  **Integritas Basis Data:** Memeriksa apakah data transaksi sewa ditulis secara tepat di MySQL, memicu pembaruan foreign key, dan memvalidasi pencegahan *double-booking* di tingkat query database.
3.  **Unggah Berkas Bukti Transfer:** Memeriksa penulisan path berkas pada kolom database (`bookings.proof_of_payment`) dan penyimpanan fisiknya di direktori server `public/uploads/proofs/`.
4.  **Autentikasi & Otorisasi Sesi:** Memverifikasi hak akses routes `/admin/*` menggunakan Role Middleware.

---

## 2. Kebutuhan Sumber Daya Uji

### A. Kredensial dan Akses:
*   Akses kueri langsung ke database staging/lokal (`db_booking_lapangan`) menggunakan DBMS.
*   Akses ke file log staging di `C:\xampp\htdocs\WebsiteBookingLapangan\storage\logs\laravel.log`.

### B. Perkakas Pengujian (Testing Tools):
*   **Postman:** Menjalankan kumpulan koleksi API otomatis (*API Automation testing*).
*   **DBeaver / phpMyAdmin:** Melakukan pengecekan data langsung di tabel relasional.
*   **Laravel Log Viewer:** Memantau log pengecualian (*exceptions log*).

---

## 3. Kriteria Penangguhan & Kelulusan Uji

*   **Kriteria Penangguhan (Suspension):** Pengujian dihentikan jika kueri MySQL mengalami kebocoran koneksi (*Too many connections error*) atau database migration gagal dipasang pada server staging.
*   **Kriteria Kelulusan (Exit Criteria):**
    1.  100% kasus uji API sukses memanipulasi database dengan presisi.
    2.  Kueri SQL pencegahan *double-booking* 100% berhasil memblokir bentrok jadwal di tingkat database.
    3.  Seluruh response API gagal validasi mengembalikan kode status HTTP `422 Unprocessable Entity` dengan skema pesan error JSON terstandarisasi.
    4.  Sistem logging mencatat error internal server dengan format stamp waktu yang benar tanpa memperlihatkan query error ke pengguna luar.
