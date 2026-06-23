# Decision Table Testing (Tabel Keputusan)

Dokumen ini mendokumentasikan hasil pengujian tabel keputusan (*Decision Table Testing*) untuk fitur **Pemesanan Lapangan (`store` method)** pada repositori [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan).

---

## 1. Tabel Keputusan Kombinasi Logika Bisnis

| Kondisi / Masukan (Conditions) | Rule 1 | Rule 2 | Rule 3 | Rule 4 | Rule 5 |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **C1: Tanggal Valid? ($\ge$ hari ini)** | YA | TIDAK | YA | YA | YA |
| **C2: Menit Genap? (Menit = `00`)** | YA | YA/TIDAK | TIDAK | YA | YA |
| **C3: Jam Operasional? (07:00 - 22:00)** | YA | YA/TIDAK | YA/TIDAK | TIDAK | YA |
| **C4: Bebas Overlap? (Tidak bentrok)** | YA | YA/TIDAK | YA/TIDAK | YA/TIDAK | TIDAK |
| **Tindakan / Aksi Sistem (Actions)** | | | | | |
| **A1: Simpan ke DB (Status: pending)** | **X** | | | | |
| **A2: Tampilkan Error Validasi Input** | | **X** | **X** | **X** | |
| **A3: Tampilkan Error Konflik Jadwal** | | | | | **X** |

---

## 2. Keterangan Aturan (Rule Description)
*   **Rule 1 (Pemesanan Sukses):** Seluruh input valid dan slot jadwal kosong. Sistem menyimpan data pemesanan ke DB dengan status `pending` dan mengalihkan pengguna ke halaman pembayaran.
*   **Rule 2 (Tanggal Lampau):** Tanggal pemesanan yang dimasukkan lebih kecil dari hari ini. Sistem langsung memblokir dan menampilkan error validasi.
*   **Rule 3 (Format Menit Salah):** Waktu mulai atau selesai tidak pas di menit `00` (contoh: `08:30`). Sistem menampilkan error kelipatan jam genap.
*   **Rule 4 (Di Luar Jam Operasional):** Jam mulai < 07:00 atau jam selesai > 22:00. Sistem menolak pemesanan.
*   **Rule 5 (Overlap/Bentrok Jadwal):** Lapangan dan tanggal sama, tetapi jam sewa beririsan dengan pemesanan lain yang berstatus `pending` atau `approved`. Sistem menampilkan error konflik jadwal.
