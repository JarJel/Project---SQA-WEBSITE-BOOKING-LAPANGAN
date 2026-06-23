# Regression Testing (Pengujian Regresi)

Pengujian Regresi (*Regression Testing*) pada tingkat Grey Box dilakukan untuk memastikan bahwa perubahan kode (perbaikan bug, penambahan fitur, refaktor database) tidak mengganggu modul backend yang telah bekerja dengan benar dan tidak merusak integritas basis data.

---

## 1. Pemicu Pengujian Regresi

Aktivitas pengujian ini dipicu oleh peristiwa-peristiwa berikut:
*   **Adanya Pull Request (PR) Perbaikan Bug:** Terutama perbaikan bug kritis seperti penanganan pemesanan ganda (double-booking).
*   **Perubahan Skema Database (Database Migration):** Penambahan kolom baru, pengubahan tipe data, atau manipulasi relasi tabel.
*   **Pembaruan Dependensi Pihak Ketiga:** Pembaruan library PHP/Laravel, Node modules, atau SDK payment gateway.

---

## 2. Skenario Pengujian Regresi Grey Box

### TC-REG-001: Verifikasi Integrasi Pasca Perbaikan Bug Double-Booking
*   **Skenario:** Menguji kembali modul booking setelah developer menerapkan penguncian database (*pessimistic locking* atau *unique constraint*).
*   **Verifikasi Backend:**
    1.  Kirim 2 request booking bersamaan untuk slot yang sama menggunakan JMeter.
    2.  Pastikan hanya 1 yang sukses (HTTP 201) dan 1 gagal (HTTP 422 atau 409).
    3.  Periksa query log database untuk memastikan query `SELECT ... FOR UPDATE` dieksekusi dengan benar.

### TC-REG-002: Verifikasi Integritas Data Relasional Migrasi Database
*   **Skenario:** Melakukan booking lapangan setelah ditambahkan kolom `discount_code` di tabel `bookings`.
*   **Verifikasi Database:**
    1.  Lakukan pemesanan tanpa kode diskon. Pastikan record tersimpan dan kolom `discount_code` bernilai `NULL`.
    2.  Lakukan pemesanan dengan kode diskon valid. Pastikan data tersimpan, diskon terpotong, dan relasi asing (*Foreign Key*) ke tabel promo konsisten.
