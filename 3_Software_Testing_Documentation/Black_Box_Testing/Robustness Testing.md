# Robustness Testing (Pengujian Ketangguhan)

Dokumen ini mendokumentasikan skenario pengujian ketangguhan (*Robustness Testing*) pada aplikasi [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan). Pengujian ini berfokus pada ketahanan aplikasi terhadap kesalahan input ekstrim, serangan siber tingkat dasar, dan kegagalan kondisi operasional eksternal.

---

## 1. Skenario Uji Ketangguhan Aplikasi

### TC-ROB-001: Serangan Injeksi Parameter Mass-Assignment (Otoritas Peran)
*   **Skenario:** Penyerang mencoba memanipulasi HTTP Request saat pendaftaran akun dengan menyisipkan parameter peran secara paksa (`"role": "admin"`) agar mendapatkan hak akses penuh.
*   **Langkah Pengujian:**
    1.  Kirim request `POST` ke `/register` menggunakan Postman dengan payload:
        ```json
        {
          "name": "Hacker",
          "email": "hacker@test.com",
          "password": "Password123!",
          "password_confirmation": "Password123!",
          "role": "admin"
        }
        ```
*   **Hasil yang Diharapkan:** Akun berhasil didaftarkan namun status peran (`role`) di database tetap terisi default yaitu `'user'`. Hal ini karena pengembang telah mengaktifkan proteksi mass-assignment pada [User.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Models/User.php) (menggunakan `$fillable` tanpa memasukkan kolom `role`).
*   **Status:** Pass

---

### TC-ROB-002: Pengunggahan File Bukti Pembayaran Berukuran Sangat Besar
*   **Skenario:** Pengguna mencoba mengunggah file bukti transfer dengan ukuran di atas batas wajar (misal: 10 MB) untuk memicu pemborosan penyimpanan server (*disk space exhaustion*).
*   **Langkah Pengujian:**
    1.  Masuk ke menu riwayat pemesanan.
    2.  Pilih file gambar bukti transfer berukuran **10.5 MB**.
    3.  Klik tombol "Unggah Bukti".
*   **Hasil yang Diharapkan:** Proses unggah diblokir di tingkat framework Laravel sebelum masuk ke memori. Pengguna diarahkan kembali ke halaman sebelumnya dengan pesan error: `The proof of payment field must not be greater than 2048 kilobytes.` (Batas maksimal unggah adalah 2 MB sesuai validasi pada [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php#L97)).
*   **Status:** Pass

---

### TC-ROB-003: Ketidakberadaan Token Proteksi Cross-Site Request Forgery (CSRF)
*   **Skenario:** Meniru serangan CSRF dengan mengirimkan request pemesanan lapangan dari formulir eksternal tanpa melampirkan token CSRF valid (`_token`).
*   **Langkah Pengujian:**
    1.  Kirim request `POST` ke `/bookings` tanpa menyertakan field `_token` pada payload atau header.
*   **Hasil yang Diharapkan:** Framework Laravel langsung menolak request di tingkat middleware, mengembalikan respon halaman kesalahan HTTP **419 Page Expired** (atau status `419 Token Mismatch`).
*   **Status:** Pass

---

### TC-ROB-004: Injeksi SQL (SQL Injection) pada Formulir Input
*   **Skenario:** Memasukkan string kueri SQL berbahaya pada filter input teks (misal: pencarian nama lapangan olahraga).
*   **Langkah Pengujian:**
    1.  Ketik kata kunci pencarian: `Lapangan Futsal' OR '1'='1`
    2.  Kirim pencarian.
*   **Hasil yang Diharapkan:** Sistem memperlakukannya sebagai string literal biasa dan menampilkan "Lapangan tidak ditemukan". Query database aman karena Laravel menggunakan prepared statements / binding parameter di tingkat ORM Eloquent secara otomatis.
*   **Status:** Pass
