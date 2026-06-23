# Equivalence Partitioning Testing - Fitur Pemesanan Lapangan

Dokumen ini mendokumentasikan hasil analisis pembagian kelas ekivalen (*Equivalence Partitioning*) untuk parameter input metode `store(Request $request)` pada [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php) secara lengkap dan sistematis.

---

## 1. Analisis Pembagian Kelas Parameter Input

Di bawah ini adalah pembagian partisi ekivalen valid (EV) dan invalid (EI) untuk setiap field input request:

### A. Field: `field_id` (ID Lapangan)
*   **Partisi Valid (EV-1):** ID berupa bilangan bulat positif yang eksis di tabel `fields` dan memiliki status `'active'`.
*   **Partisi Invalid (EI-1):** ID bernilai kosong (*null*).
*   **Partisi Invalid (EI-2):** ID berupa angka tetapi tidak eksis di database (contoh: `9999`).
*   **Partisi Invalid (EI-3):** ID eksis di database tetapi status lapangan bernilai `'maintenance'`.

### B. Field: `booking_date` (Tanggal Pemesanan)
*   **Partisi Valid (EV-2):** Tanggal berformat `YYYY-MM-DD` dan bernilai $\ge$ hari ini.
*   **Partisi Invalid (EI-4):** Tanggal berformat `YYYY-MM-DD` tetapi bernilai lampau (< hari ini).
*   **Partisi Invalid (EI-5):** Nilai kosong (*null*).
*   **Partisi Invalid (EI-6):** Format tanggal salah (contoh: `25-06-2026` atau `string`).

### C. Field: `start_time` & `end_time` (Waktu Sewa)
*   **Partisi Valid (EV-3):** Format waktu `H:i` dengan menit = `00` (contoh: `08:00`, `15:00`).
*   **Partisi Valid (EV-4):** Jam sewa berada pada rentang operasional `07:00` s.d `22:00`.
*   **Partisi Valid (EV-5):** Nilai `end_time` secara kronologis setelah `start_time`.
*   **Partisi Invalid (EI-7):** Menit tidak bulat `00` (contoh: `08:30`, `10:15`).
*   **Partisi Invalid (EI-8):** Jam di luar operasional (contoh: `05:00` atau `23:00`).
*   **Partisi Invalid (EI-9):** Nilai `end_time` sebelum atau sama dengan `start_time` (contoh: mulai `10:00`, selesai `09:00`).
*   **Partisi Invalid (EI-10):** Format waktu salah (contoh: `8 AM` atau `string`).
*   **Partisi Invalid (EI-11):** Nilai kosong (*null*).

### D. Keadaan Database: Bentrok Jadwal (Overlap)
*   **Partisi Valid (EV-6):** Slot waktu yang dipilih tidak tumpang tindih dengan pesanan berstatus `pending` atau `approved`.
*   **Partisi Invalid (EI-12):** Slot waktu beririsan dengan pesanan `pending` atau `approved` pada lapangan dan tanggal yang sama.

---

## 2. Matriks Pengujian Ekivalen Detail

| ID Uji | Kelas Input Diuji | Data Payload Request (JSON) | Hasil yang Diharapkan | Status |
| :--- | :--- | :--- | :--- | :---: |
| **TC-EP-001** | **EV-1, EV-2, EV-3, EV-4, EV-5, EV-6** (Valid Penuh) | `{ "field_id": 1, "booking_date": "2026-06-25", "start_time": "08:00", "end_time": "10:00" }` | **Lolos Validasi**<br>Data tersimpan di DB, status pending, redirect ke riwayat dengan pesan sukses. | Pass |
| **TC-EP-002** | **EI-1** (`field_id` kosong) | `{ "field_id": null, "booking_date": "2026-06-25", "start_time": "08:00", "end_time": "10:00" }` | **Ditolak (Validator)**<br>Error: `The field id field is required.` | Pass |
| **TC-EP-003** | **EI-2** (`field_id` tidak eksis) | `{ "field_id": 9999, "booking_date": "2026-06-25", "start_time": "08:00", "end_time": "10:00" }` | **Ditolak (Validator)**<br>Error: `The selected field id is invalid.` | Pass |
| **TC-EP-004** | **EI-3** (Lapangan nonaktif) | `{ "field_id": 2, ... }` (Asumsi lapangan 2 status 'maintenance') | **Ditolak (Controller Logic)**<br>Error: `Lapangan sedang tidak aktif atau dalam perawatan.` | Pass |
| **TC-EP-005** | **EI-4** (Tanggal lampau) | `{ "field_id": 1, "booking_date": "2026-06-01", ... }` | **Ditolak (Validator)**<br>Error: `The booking date field must be a date after or equal to today.` | Pass |
| **TC-EP-006** | **EI-6** (Format tanggal salah) | `{ "field_id": 1, "booking_date": "tulisan-tanggal", ... }` | **Ditolak (Validator)**<br>Error: `The booking date field must be a valid date.` | Pass |
| **TC-EP-007** | **EI-7** (Menit ganjil) | `{ "field_id": 1, "booking_date": "2026-06-25", "start_time": "08:30", "end_time": "10:00" }` | **Ditolak (Controller Logic)**<br>Error: `Pemesanan harus dalam kelipatan jam genap (misal: 08:00, 09:00).` | Pass |
| **TC-EP-008** | **EI-8** (Luar operasional) | `{ "field_id": 1, "booking_date": "2026-06-25", "start_time": "06:00", "end_time": "08:00" }` | **Ditolak (Controller Logic)**<br>Error: `Pemesanan harus di dalam jam operasional (07:00 - 22:00).` | Pass |
| **TC-EP-009** | **EI-9** (Waktu terbalik) | `{ "field_id": 1, "booking_date": "2026-06-25", "start_time": "10:00", "end_time": "08:00" }` | **Ditolak (Validator)**<br>Error: `The end time field must be after start time.` | Pass |
| **TC-EP-010** | **EI-12** (Overlap/Bentrok) | `{ "field_id": 1, "booking_date": "2026-06-25", "start_time": "08:00", "end_time": "10:00" }` (Setelah TC-EP-001 sukses dipesan) | **Ditolak (Controller Logic)**<br>Error: `Slot waktu yang Anda pilih sudah dipesan oleh pengguna lain.` | Pass |
