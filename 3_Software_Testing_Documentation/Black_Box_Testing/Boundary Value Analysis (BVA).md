# Boundary Value Analysis (BVA) Testing

Dokumen ini mendokumentasikan hasil analisis nilai batas (*Boundary Value Analysis*) untuk fitur **Pemesanan Lapangan (`store` method)** pada repositori [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan). Pengujian difokuskan pada validasi input tanggal sewa dan jam operasional.

---

## 1. Spesifikasi Batas Input Bisnis
Berdasarkan kode program di [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php):
*   **Tanggal Pemesanan (`booking_date`):** Harus hari ini atau hari esok (setelah atau sama dengan hari ini).
*   **Jam Operasional:** Lapangan hanya dapat dipesan dari pukul **07:00 hingga 22:00**.
*   **Menit Pemesanan:** Harus tepat kelipatan jam genap (menit = `00`, misal: `08:00`, `09:00`).

---

## 2. Parameter Nilai Batas

### Batas Jam Operasional (07:00 - 22:00)
*   Batas bawah: `07:00`
*   Batas atas: `22:00`

### Batas Tanggal (Hari ini = $T_0$)
*   Hari kemarin: $T_{-1}$
*   Hari ini: $T_0$
*   Besok: $T_{+1}$

---

## 3. Matriks Kasus Uji BVA

| ID Uji | Parameter | Nilai Input | Tipe Batas | Hasil yang Diharapkan | Status |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **TC-BVA-001** | `booking_date` | Hari Kemarin ($T_{-1}$) | Di luar batas bawah (Invalid) | Ditolak (Error: `booking_date harus setelah atau sama dengan today`) | Pass |
| **TC-BVA-002** | `booking_date` | Hari Ini ($T_0$) | Batas bawah (Valid) | Diterima | Pass |
| **TC-BVA-003** | `booking_date` | Besok ($T_{+1}$) | Di dalam batas (Valid) | Diterima | Pass |
| **TC-BVA-004** | `start_time` | `06:00` | Di luar batas bawah (Invalid) | Ditolak (Error: `Pemesanan harus di dalam jam operasional`) | Pass |
| **TC-BVA-005** | `start_time` | `07:00` | Batas bawah (Valid) | Diterima | Pass |
| **TC-BVA-006** | `end_time` | `22:00` | Batas atas (Valid) | Diterima | Pass |
| **TC-BVA-007** | `end_time` | `23:00` | Di luar batas atas (Invalid) | Ditolak (Error: `Pemesanan harus di dalam jam operasional`) | Pass |
