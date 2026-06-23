# Matrix Testing (Pengujian Matriks)

Pengujian Matriks (*Matrix Testing*) digunakan untuk memetakan hubungan antara kebutuhan fungsional (dari SRS), entitas database (dari SDD), dan skenario pengujian yang dikembangkan. Ini memastikan seluruh cakupan pengujian (*traceability*) terpenuhi.

---

## 1. Matriks Penelusuran Kebutuhan (Requirements Traceability Matrix)

Berikut adalah matriks pemetaan yang menghubungkan ID Fitur SRS dengan tabel database yang terpengaruh serta ID Kasus Uji yang memvalidasinya.

| ID Kebutuhan (SRS) | Nama Fitur | Tabel Database Terkait | ID Kasus Uji (Test Case) | Status Pengujian |
| :--- | :--- | :--- | :--- | :---: |
| **SKPL-F-001** | Registrasi Akun | `users` | TC-BB-AUTH-001, TC-BB-AUTH-002 | Terlaksana |
| **SKPL-F-003** | Login Pengguna | `users` | TC-BB-AUTH-003 | Terlaksana |
| **SKPL-F-005** | Booking Lapangan | `bookings`, `fields` | TC-BB-BOOK-001, TC-GB-API-001 | Terlaksana |
| **SKPL-F-007** | Kelola Lapangan | `fields` | TC-BB-ADM-001 | Siap Uji |

---

## 2. Matriks Uji CRUD Tabel Database

Matriks ini memastikan setiap aksi manipulasi data (Create, Read, Update, Delete) yang didefinisikan dalam kode program telah diuji di tingkat basis data.

| Nama Tabel | Create (C) | Read (R) | Update (U) | Delete (D) | Keterangan |
| :--- | :---: | :---: | :---: | :---: | :--- |
| `users` | TC-AUTH-001 | TC-AUTH-003 | [ ] | [ ] | Pendaftaran & Login teruji |
| `fields` | [ ] | TC-BOOK-001 | [ ] | [ ] | Tampilan lapangan teruji |
| `bookings` | TC-BOOK-001 | TC-BOOK-001 | [ ] | [ ] | Pembuatan transaksi teruji |
