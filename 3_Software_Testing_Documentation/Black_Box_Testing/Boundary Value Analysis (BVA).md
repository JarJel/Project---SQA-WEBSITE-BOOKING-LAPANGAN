# Boundary Value Analysis (BVA) Testing - Fitur Pemesanan Lapangan

Dokumen ini menjelaskan rancangan kasus uji nilai batas (*Boundary Value Analysis*) untuk fitur **Pemesanan Lapangan (`store` method)** di [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php) secara terperinci. BVA digunakan untuk menguji nilai-nilai ekstrem pada batas valid dan invalid dari rentang input yang ditentukan oleh aturan bisnis.

---

## 1. Spesifikasi Batas Input Bisnis

Berdasarkan implementasi kode program, variabel-variabel input memiliki batasan nilai sebagai berikut:
1.  **Tanggal Pemesanan (`booking_date`):**
    *   *Batasan:* Harus hari ini atau hari-hari setelahnya (tidak boleh hari kemarin).
    *   *Ekspresi Kode:* `after_or_equal:today`
2.  **Jam Mulai (`start_time`):**
    *   *Batasan:* Tidak boleh kurang dari pukul **07:00** (Jam operasional mulai).
    *   *Ekspresi Kode:* `$start->hour < 7` (memicu error jika True).
3.  **Jam Selesai (`end_time`):**
    *   *Batasan:* Tidak boleh lebih dari pukul **22:00** (Jam operasional tutup).
    *   *Ekspresi Kode:* `$end->hour > 22` (memicu error jika True).
4.  **Menit Pemesanan (`start_time` & `end_time`):**
    *   *Batasan:* Harus tepat menit `:00` (kelipatan jam genap).
    *   *Ekspresi Kode:* `$start->minute !== 0 || $end->minute !== 0` (memicu error jika True).

---

## 2. Definisi Nilai Uji Ekstrem

### A. Batas Tanggal (Hari ini = $T_0$)
*   **$T_{-1}$ (Kemarin):** Batas invalid bawah.
*   **$T_0$ (Hari ini):** Batas valid bawah.
*   **$T_{+1}$ (Esok hari):** Nilai normal valid.

### B. Batas Jam Mulai ($07:00$)
*   **06:59 (06:00 / 06:59):** Batas invalid bawah.
*   **07:00:** Batas valid bawah.
*   **08:00:** Nilai normal valid.

### C. Batas Jam Selesai ($22:00$)
*   **21:00:** Nilai normal valid.
*   **22:00:** Batas valid atas.
*   **22:01 (22:01 / 23:00):** Batas invalid atas.

---

## 3. Matriks Detail Kasus Uji BVA

Berikut adalah skenario uji nilai batas lengkap dengan payload request, evaluasi batas, dan respon detail dari Laravel Validator:

| ID Uji | Parameter Uji | Payload Request (JSON) | Kategori Batas | Hasil yang Diharapkan | Status |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **TC-BVA-001** | `booking_date` | `{ "field_id": 1, "booking_date": "2026-06-22", "start_time": "08:00", "end_time": "10:00" }` (Asumsi hari ini = 23 Juni 2026) | Di bawah batas bawah ($T_{-1}$ / Lampau) | **Gagal Validasi**<br>Status: `302 Found` (Redirect back)<br>Error message: `The booking date field must be a date after or equal to today.` | Pass |
| **TC-BVA-002** | `booking_date` | `{ "field_id": 1, "booking_date": "2026-06-23", "start_time": "08:00", "end_time": "10:00" }` | Tepat pada batas bawah ($T_0$ / Hari ini) | **Berhasil**<br>Status: `302 Found` (Redirect ke dashboard dengan sukses) | Pass |
| **TC-BVA-003** | `booking_date` | `{ "field_id": 1, "booking_date": "2026-06-24", "start_time": "08:00", "end_time": "10:00" }` | Di atas batas bawah ($T_{+1}$ / Besok) | **Berhasil**<br>Status: `302 Found` (Pemesanan diproses) | Pass |
| **TC-BVA-004** | `start_time` | `{ "field_id": 1, "booking_date": "2026-06-23", "start_time": "06:00", "end_time": "08:00" }` | Di bawah batas operasional bawah (`06:00 < 07:00`) | **Gagal Validasi**<br>Status: `302 Found` (Redirect back)<br>Error message: `Pemesanan harus di dalam jam operasional (07:00 - 22:00).` | Pass |
| **TC-BVA-005** | `start_time` | `{ "field_id": 1, "booking_date": "2026-06-23", "start_time": "07:00", "end_time": "09:00" }` | Tepat pada batas operasional bawah (`07:00`) | **Berhasil**<br>Status: `302 Found` (Pemesanan diproses) | Pass |
| **TC-BVA-006** | `end_time` | `{ "field_id": 1, "booking_date": "2026-06-23", "start_time": "20:00", "end_time": "22:00" }` | Tepat pada batas operasional atas (`22:00`) | **Berhasil**<br>Status: `302 Found` (Pemesanan diproses) | Pass |
| **TC-BVA-007** | `end_time` | `{ "field_id": 1, "booking_date": "2026-06-23", "start_time": "21:00", "end_time": "23:00" }` | Di atas batas operasional atas (`23:00 > 22:00`) | **Gagal Validasi**<br>Status: `302 Found` (Redirect back)<br>Error message: `Pemesanan harus di dalam jam operasional (07:00 - 22:00).` | Pass |
| **TC-BVA-008** | Menit `start` | `{ "field_id": 1, "booking_date": "2026-06-23", "start_time": "08:01", "end_time": "10:00" }` | Penyimpangan menit batas bawah (`08:01`) | **Gagal Validasi**<br>Status: `302 Found` (Redirect back)<br>Error message: `Pemesanan harus dalam kelipatan jam genap (misal: 08:00, 09:00).` | Pass |
| **TC-BVA-009** | Menit `end` | `{ "field_id": 1, "booking_date": "2026-06-23", "start_time": "08:00", "end_time": "09:59" }` | Penyimpangan menit batas atas (`09:59`) | **Gagal Validasi**<br>Status: `302 Found` (Redirect back)<br>Error message: `Pemesanan harus dalam kelipatan jam genap (misal: 08:00, 09:00).` | Pass |
