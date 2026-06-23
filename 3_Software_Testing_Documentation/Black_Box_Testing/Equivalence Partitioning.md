# Equivalence Partitioning Testing

Dokumen ini mendokumentasikan hasil pengujian pembagian kelas ekivalen (*Equivalence Partitioning*) untuk fitur **Pemesanan Lapangan (`store` method)** pada repositori [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan).

---

## 1. Pembagian Kelas Input

### Kelas Input 1: Tanggal Booking (`booking_date`)
*   **Valid (EV-1):** Tanggal $\ge$ hari ini (misal: hari ini, besok, minggu depan).
*   **Tidak Valid (EI-1):** Tanggal < hari ini (misal: kemarin, bulan lalu).

### Kelas Input 2: Format Menit (`start_time` & `end_time`)
*   **Valid (EV-2):** Menit bernilai `00` (misal: `08:00`, `14:00`).
*   **Tidak Valid (EI-2):** Menit tidak bernilai `00` (misal: `08:30`, `14:15`).

### Kelas Input 3: Rentang Waktu (Jam Operasional)
*   **Valid (EV-3):** Waktu berada di antara `07:00` s.d `22:00`.
*   **Tidak Valid (EI-3):** Waktu berada sebelum `07:00` atau sesudah `22:00` (misal: `05:00`, `23:00`).

### Kelas Input 4: Urutan Waktu
*   **Valid (EV-4):** `end_time` > `start_time`.
*   **Tidak Valid (EI-4):** `end_time` $\le$ `start_time` (misal: mulai `10:00` selesai `09:00` atau `10:00`).

### Kelas Input 5: Status Ketersediaan Jadwal (Overlap)
*   **Valid (EV-5):** Jam sewa tidak bentrok dengan sewa terkonfirmasi lain.
*   **Tidak Valid (EI-5):** Jam sewa bentrok dengan sewa terkonfirmasi lain yang berstatus `pending` atau `approved`.

---

## 2. Matriks Kasus Uji Equivalence Partitioning

| ID Uji | Kelas Input Diuji | Contoh Input | Hasil yang Diharapkan | Status |
| :--- | :--- | :--- | :--- | :---: |
| **TC-EP-001** | EV-1 (Tanggal Valid) | `2026-06-25` (Masa Depan) | Diterima | Pass |
| **TC-EP-002** | EI-1 (Tanggal Lampau) | `2026-06-01` (Lampau) | Ditolak (Error: `booking_date harus setelah atau sama dengan today`) | Pass |
| **TC-EP-003** | EV-2 (Menit Pas) | `08:00` - `09:00` | Diterima | Pass |
| **TC-EP-004** | EI-2 (Menit Ganjil) | `08:15` - `09:00` | Ditolak (Error: `Pemesanan harus dalam kelipatan jam genap`) | Pass |
| **TC-EP-005** | EI-3 (Luar Jam Operasional)| `22:00` - `23:00` | Ditolak (Error: `Pemesanan harus di dalam jam operasional`) | Pass |
| **TC-EP-006** | EI-4 (Waktu Terbalik) | `10:00` - `08:00` | Ditolak (Error: `end_time harus setelah start_time`) | Pass |
| **TC-EP-007** | EI-5 (Overlap Bentrok) | Input jam yang sama dengan booking yang sudah `approved`/`pending` | Ditolak (Error: `Slot waktu sudah dipesan oleh pengguna lain`) | Pass |
