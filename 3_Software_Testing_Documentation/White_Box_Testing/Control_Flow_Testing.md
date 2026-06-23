# Control Flow Testing - White Box Testing

Dokumen ini mendokumentasikan skenario pengujian alur kontrol (*Control Flow Testing*) pada metode `store` di [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php). Pengujian dirancang untuk memenuhi metrik *Statement Coverage* dan *Branch Coverage* secara penuh.

---

## 1. Analisis Metrik Cakupan Kontrol

### A. Cakupan Pernyataan (Statement Coverage)
*   **Target:** Mengeksekusi setiap baris kode di metode `store` minimal sekali.
*   **Formula Cakupan:**
    $$\text{Statement Coverage} = \frac{\text{Jumlah pernyataan yang dieksekusi}}{\text{Total seluruh pernyataan di kode}} \times 100\%$$
*   **Jumlah Test Case Minimal untuk 100%:** 5 Kasus Uji.

### B. Cakupan Cabang (Branch Coverage)
*   **Target:** Mengeksekusi seluruh alternatif hasil keputusan (cabang `true` dan cabang `false` pada setiap kondisi `if` dan logical operators `||`).
*   **Formula Cakupan:**
    $$\text{Branch Coverage} = \frac{\text{Jumlah cabang keputusan yang dieksekusi}}{\text{Total seluruh cabang keputusan di kode}} \times 100\%$$
*   **Jumlah Test Case Minimal untuk 100%:** 7 Kasus Uji (Sesuai dengan Basis Path).

---

## 2. Peta Kasus Uji Cakupan Kontrol (Control Flow Test Suite)

Tabel berikut menunjukkan parameter input dan cabang mana saja yang dieksekusi oleh masing-masing test case:

| ID Uji | Input Parameter | Status DB Mock | Cabang Logika yang Dievaluasi | Baris Kode yang Dieksekusi | Hasil Cakupan (Statement / Branch) |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **TC-CF-01** | `field_id = 2`<br>`booking_date = today`<br>`start_time = 08:00`<br>`end_time = 10:00` | Lapangan 2 status = `'maintenance'` | `if ($field->status !== 'active')` ➔ **True** | Baris 14-27 (Validasi dasar, query, return error inaktif). | 55% Statement / 14% Branch |
| **TC-CF-02** | `field_id = 1`<br>`booking_date = today`<br>`start_time = 08:30`<br>`end_time = 10:00` | Lapangan 1 status = `'active'` | `if ($field->status !== 'active')` ➔ **False**<br>`if ($start->minute !== 0)` ➔ **True** | Baris 14-25, 27-38 (Lolos status, parse waktu, return error menit). | 65% Statement / 28% Branch |
| **TC-CF-03** | `field_id = 1`<br>`booking_date = today`<br>`start_time = 08:00`<br>`end_time = 10:15` | Lapangan 1 status = `'active'` | `if ($start->minute !== 0)` ➔ **False**<br>`if ($end->minute !== 0)` ➔ **True** | Baris 14-25, 27-38 (Lolos status, lolos start min, return error end min). | 65% Statement / 42% Branch |
| **TC-CF-04** | `field_id = 1`<br>`booking_date = today`<br>`start_time = 06:00`<br>`end_time = 08:00` | Lapangan 1 status = `'active'` | `if ($start->hour < 7)` ➔ **True** | Baris 14-25, 27-37, 40-44 (Lolos menit, return error luar operasional). | 75% Statement / 57% Branch |
| **TC-CF-05** | `field_id = 1`<br>`booking_date = today`<br>`start_time = 21:00`<br>`end_time = 23:00` | Lapangan 1 status = `'active'` | `if ($start->hour < 7)` ➔ **False**<br>`if ($end->hour > 22)` ➔ **True** | Baris 14-25, 27-37, 40-44 (Lolos start hour, return error luar operasional). | 75% Statement / 71% Branch |
| **TC-CF-06** | `field_id = 1`<br>`booking_date = today`<br>`start_time = 08:00`<br>`end_time = 10:00` | Lapangan 1 status = `'active'`. Ada booking bentrok di database. | `if ($overlap)` ➔ **True** | Baris 14-25, 27-37, 40-58 (Lolos operasional, query overlap, return error bentrok). | 85% Statement / 85% Branch |
| **TC-CF-07** | `field_id = 1`<br>`booking_date = today`<br>`start_time = 08:00`<br>`end_time = 10:00` | Lapangan 1 status = `'active'`. Slot waktu kosong di DB. | `if ($overlap)` ➔ **False** | Baris 14-25, 27-37, 40-55, 58-73 (Lolos semua validasi, buat booking, redirect sukses). | **100% Statement / 100% Branch** |
