# Comparison Table Testing

Dokumen ini mendokumentasikan hasil pengujian perbandingan (*Comparison Table Testing*) untuk membandingkan parameter masukan permintaan (*request inputs*) dengan status data internal di database untuk fitur pencegahan tumpang tindih (*overlap prevention*) pada repositori [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan).

---

## 1. Tabel Perbandingan Evaluasi Logika Overlap

Fungsi pengecekan tumpang tindih sewa pada sistem mendeteksi bentrok jadwal jika parameter request beririsan dengan data sewa yang berstatus `pending` atau `approved` pada lapangan dan tanggal yang sama.

Formula Logika SQL:
```sql
start_time < :end_time AND end_time > :start_time
```

| Skenario Pembanding | Nilai Input Request | Data Tersimpan di DB (Approved/Pending) | Logika Penilaian Overlap | Hasil Evaluasi Sistem |
| :--- | :--- | :--- | :--- | :--- |
| **Kasus Bentrok Total** | Tanggal: `2026-06-25`<br>Jam: `10:00 - 12:00` | Tanggal: `2026-06-25`<br>Jam: `10:00 - 12:00` | `10:00` < `12:00` AND `12:00` > `10:00` (Bentrok) | **Ditolak** (Booking Gagal) |
| **Kasus Irisan Awal** | Tanggal: `2026-06-25`<br>Jam: `09:00 - 11:00` | Tanggal: `2026-06-25`<br>Jam: `10:00 - 12:00` | `09:00` < `12:00` AND `11:00` > `10:00` (Bentrok) | **Ditolak** (Booking Gagal) |
| **Kasus Irisan Akhir** | Tanggal: `2026-06-25`<br>Jam: `11:00 - 13:00` | Tanggal: `2026-06-25`<br>Jam: `10:00 - 12:00` | `11:00` < `12:00` AND `13:00` > `10:00` (Bentrok) | **Ditolak** (Booking Gagal) |
| **Kasus Berdampingan Awal**| Tanggal: `2026-06-25`<br>Jam: `08:00 - 10:00` | Tanggal: `2026-06-25`<br>Jam: `10:00 - 12:00` | `08:00` < `10:00` AND `10:00` > `10:00` (Aman) | **Diterima** (Booking Sukses) |
| **Kasus Berdampingan Akhir**| Tanggal: `2026-06-25`<br>Jam: `12:00 - 14:00` | Tanggal: `2026-06-25`<br>Jam: `10:00 - 12:00` | `12:00` < `12:00` AND `14:00` > `12:00` (Aman) | **Diterima** (Booking Sukses) |
