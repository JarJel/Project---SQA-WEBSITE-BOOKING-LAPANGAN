# Matrix Testing (Pengujian Matriks) - Grey Box Testing

Dokumen ini mendokumentasikan hasil pengujian matriks (*Matrix Testing*) untuk memetakan hubungan antara kebutuhan fungsional sistem, komponen tabel basis data, dan skenario kasus uji pada repositori [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan).

---

## 1. Matriks Penelusuran Kebutuhan (Requirements Traceability Matrix)

Matriks ini memastikan setiap kebutuhan fungsional (didefinisikan dalam SRS) memiliki representasi skema database yang tepat dan kasus uji yang valid:

| ID Kebutuhan (SRS) | Kebutuhan Fitur | File Controller Laravel | Tabel & Kolom Database Terkait | ID Kasus Uji | Status Verifikasi |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **SKPL-F-001** | Registrasi Akun Baru | `Auth/RegisteredUserController.php` | `users` (`name`, `email`, `password`, `phone`, `role`) | TC-EP-002, TC-EP-009 | Terlaksana |
| **SKPL-F-002** | Autentikasi Pengguna | `Auth/AuthenticatedSessionController.php` | `users` (`email`, `password`) | TC-BVA-002, TC-EP-003 | Terlaksana |
| **SKPL-F-003** | Mengubah Status Lapangan | `Admin/FieldController.php` | `fields` (`status`) | TC-EP-004 | Terlaksana |
| **SKPL-F-004** | Pengajuan Booking Lapangan | `BookingController.php` (method `store`) | `bookings` (`user_id`, `field_id`, `booking_date`, `start_time`, `end_time`, `total_price`, `status`) | TC-BVA-001 s.d 009,<br>TC-EP-001 s.d 010 | Terlaksana |
| **SKPL-F-005** | Unggah Bukti Transfer | `BookingController.php` (method `uploadPayment`) | `bookings` (`proof_of_payment`) | TC-ROB-002 | Terlaksana |
| **SKPL-F-006** | Pembatalan Booking | `BookingController.php` (method `cancel`) | `bookings` (`status`) | TC-BEH-002 | Terlaksana |

---

## 2. Matriks Uji CRUD & Validasi Query SQL Database

Penguji Grey Box memvalidasi manipulasi data di tabel database dengan mengamati eksekusi Query SQL langsung pada log MySQL (`mysql-query.log` atau via Laravel Telescope):

| Nama Tabel | Operasi | Kueri SQL Terkait yang Dieksekusi | Kasus Uji Terkait | Status DB |
| :--- | :---: | :--- | :--- | :---: |
| **`users`** | **C** | `INSERT INTO users (name, email, password, phone, role, ...) VALUES (?, ?, ?, ?, 'user', ...)` | TC-EP-001 | Valid |
| | **R** | `SELECT * FROM users WHERE email = ? LIMIT 1` | TC-EP-002 | Valid |
| **`fields`** | **R** | `SELECT * FROM fields WHERE id = ? LIMIT 1` | TC-EP-003 | Valid |
| | **U** | `UPDATE fields SET status = 'maintenance' WHERE id = ?` | TC-EP-004 | Valid |
| **`bookings`** | **C** | `INSERT INTO bookings (user_id, field_id, booking_date, start_time, end_time, total_price, status) VALUES (?, ?, ?, ?, ?, ?, 'pending')` | TC-EP-001 | Valid |
| | **R** | `SELECT EXISTS (SELECT * FROM bookings WHERE field_id = ? AND booking_date = ? AND status IN ('pending', 'approved') AND (start_time < ? AND end_time > ?))` | TC-EP-010 | Valid |
| | **U** | `UPDATE bookings SET proof_of_payment = ?, status = 'pending' WHERE id = ?` | TC-ROB-002 | Valid |
| | **U** | `UPDATE bookings SET status = 'cancelled' WHERE id = ?` | TC-BEH-002 | Valid |
