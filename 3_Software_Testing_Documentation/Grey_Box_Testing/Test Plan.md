# Grey Box Test Plan (Rencana Pengujian Grey Box)

Dokumen ini mendefinisikan rencana pengujian (*Test Plan*) khusus untuk aktivitas pengujian tingkat Grey Box pada sistem booking lapangan.

---

## 1. Ruang Lingkup Pengujian Grey Box

Pengujian difokuskan pada integrasi antara antarmuka pengguna (Frontend), logika aplikasi (Backend API), dan tempat penyimpanan data (Database Server).

### Cakupan Pengujian:
1.  **Antarmuka API (API Endpoints):** Keakuratan response status code, response body schema, header, dan handling error API.
2.  **Validasi Database:** Konsistensi penyimpanan data, integritas relasi tabel (Foreign Keys), trigger database, dan validasi tipe data kolom.
3.  **Antrean Tugas Backend (Queues / Jobs):** Pengujian job pengiriman email secara asinkronus (background worker).
4.  **Penanganan Keamanan Sesi (Session & Token Management):** Validasi JWT/Session payload untuk berbagai role pengguna.

---

## 2. Kebutuhan Sumber Daya Uji

*   **Akses Database:** Akses Read/Write ke database staging menggunakan kredensial pengujian khusus.
*   **Perkakas Pengujian (Testing Tools):**
    *   **Postman:** Uji endpoint API secara otomatis (*API collection runner*).
    *   **DBeaver / phpMyAdmin:** Inspeksi langsung data di tabel-tabel database.
    *   **Laravel Telescope / Laravel Log Viewer:** Memantau query SQL yang berjalan, log error, dan status antrean job email.
*   **Kriteria Kelulusan Uji:**
    1.  Semua endpoint API utama (Auth, Booking, Lapangan) mengembalikan response sesuai kontrak API.
    2.  Seluruh query SQL yang dihasilkan oleh aplikasi menggunakan parameter binding (tidak rentan SQL Injection).
    3.  Data yang terisi pada database 100% presisi sesuai input pengguna.
    4.  Job email pengingat booking diproses sukses oleh queue worker tanpa kegagalan.
