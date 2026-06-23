# Regression Testing (Pengujian Regresi) - Grey Box Testing

Dokumen ini mendokumentasikan skenario dan prosedur pengujian regresi (*Regression Testing*) tingkat Grey Box pada sistem [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan). Pengujian regresi bertujuan memastikan perbaikan bug (seperti penanganan double-booking) atau pembaruan migrasi database tidak merusak fungsi yang sudah berjalan normal.

---

## 1. Pemicu Skenario Regresi

*   **Pemicu A:** Perbaikan bug logika kueri tumpang tindih (*overlap validation*).
*   **Pemicu B:** Pembaruan migrasi database (penambahan kolom baru, manipulasi relasi, dll.).
*   **Pemicu C:** Refactoring kode middleware autentikasi (role-check).

---

## 2. Kasus Uji Regresi Tingkat Grey Box

### TC-REG-001: Verifikasi Integrasi Pasca Perbaikan Bug Double-Booking
*   **Skenario:** Developer telah mengubah penanganan overlap kueri di [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php) untuk mencegah dua transaksi pending/approved beririsan jam.
*   **Prosedur Verifikasi:**
    1.  Jalankan 2 instance Postman secara simultan untuk mengirim request sewa lapangan 1 pada tanggal yang sama, jam 10:00 - 12:00.
    2.  *Langkah DB:* Jalankan kueri SQL untuk mengecek record yang terbuat:
        ```sql
        SELECT id, user_id, field_id, start_time, end_time, status FROM bookings 
        WHERE field_id = 1 AND booking_date = '2026-06-25';
        ```
*   **Hasil yang Diharapkan:** Hanya ada 1 record baru yang berhasil ditambahkan dengan status `pending`/`approved`. Request kedua harus diblokir di tingkat PHP/Database, menghasilkan error `Slot waktu yang Anda pilih sudah dipesan oleh pengguna lain`.

---

### TC-REG-002: Verifikasi Integritas Relasional Pasca Migrasi Skema Database
*   **Skenario:** Menguji migrasi database terbaru (penambahan kolom `phone` di tabel `users` dan kolom `proof_of_payment` di tabel `bookings`).
*   **Prosedur Verifikasi:**
    1.  *Langkah Skema:* Hubungkan ke DBMS, jalankan kueri deskripsi tabel:
        ```sql
        DESCRIBE users;
        DESCRIBE bookings;
        ```
        Pastikan kolom `phone` (VARCHAR 20) dan `proof_of_payment` (VARCHAR 255) terdaftar secara fisik di database.
    2.  *Langkah Integrasi:* Jalankan alur registrasi pengguna baru dengan menyertakan nomor HP. Periksa apakah nilai `phone` tersimpan dengan benar.
    3.  *Langkah Transaksi:* Lakukan upload bukti pembayaran. Jalankan kueri SQL:
        ```sql
        SELECT proof_of_payment FROM bookings WHERE id = ?;
        ```
        Pastikan kolom tersebut terisi path file yang valid (misal: `uploads/proofs/file.jpg`).
*   **Hasil yang Diharapkan:** Seluruh kolom baru dapat menyimpan dan memanipulasi data tanpa memicu kegagalan integritas basis data (*Foreign Key constraints* atau *Data truncation errors*).
