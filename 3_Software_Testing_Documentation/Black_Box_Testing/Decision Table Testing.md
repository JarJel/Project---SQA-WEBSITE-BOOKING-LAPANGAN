# Decision Table Testing (Tabel Keputusan) - Fitur Pemesanan Lapangan

Dokumen ini mendokumentasikan rancangan pengujian tabel keputusan (*Decision Table Testing*) untuk menangani berbagai skenario kombinasi masukan logika bisnis pada metode `store` di [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php).

---

## 1. Tabel Keputusan Pengujian (8 Rules Matrix)

Berikut adalah matriks keputusan logika bisnis yang merepresentasikan aliran data dan prioritas validasi di dalam controller:

| Kondisi / Masukan (Conditions) | R-1 | R-2 | R-3 | R-4 | R-5 | R-6 | R-7 | R-8 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **C1: Tanggal Valid ($\ge$ today)?** | N | Y | Y | Y | Y | Y | Y | Y |
| **C2: Menit Genap (`:00`)?** | - | N | Y | Y | Y | Y | Y | Y |
| **C3: Di Dalam Jam Operasional (07:00-22:00)?**| - | - | N | Y | Y | Y | Y | Y |
| **C4: Bebas Overlap (Belum terisi)?** | - | - | - | N | Y | Y | Y | Y |
| **C5: Lapangan Berstatus Aktif ('active')?** | - | - | - | - | N | Y | Y | Y |
| **C6: Bukti Pembayaran Diunggah?** | - | - | - | - | - | N | Y | Y |
| **C7: Batas Waktu Bayar Terlewati (>1 Jam)?**| - | - | - | - | - | - | N | Y |
| **Hasil / Tindakan Sistem (Actions)** | | | | | | | | |
| **A1: Tolak - Error Validasi Input** | **X** | **X** | **X** | | | | | |
| **A2: Tolak - Error Overlap** | | | | **X** | | | | |
| **A3: Tolak - Error Status Lapangan** | | | | | **X** | | | |
| **A4: Buat Booking (Status: pending)** | | | | | | **X** | **X** | |
| **A5: Ubah Status Booking -> Approved** | | | | | | | **X** | |
| **A6: Ubah Status Booking -> Expired** | | | | | | | | **X** |

---

## 2. Analisis Kasus Berdasarkan Aturan (Rules Analysis)

*   **Rule 1 (R-1):** Tanggal pemesanan tidak valid (lampau). Sistem langsung menghentikan proses di tingkat validator (A1), mengabaikan pengecekan menit, jam, dan status lainnya.
*   **Rule 2 (R-2):** Tanggal valid, tetapi format menit tidak bulat `:00` (contoh: `08:15`). Sistem menghentikan aliran di tingkat validasi menit (A1).
*   **Rule 3 (R-3):** Tanggal & menit valid, tetapi jam di luar operasional (contoh: `05:00 - 07:00`). Sistem menghentikan aliran di tingkat validasi jam operasional (A1).
*   **Rule 4 (R-4):** Seluruh input valid, tetapi jam sewa beririsan dengan pesanan orang lain. Sistem menghentikan aliran di tingkat kueri database (A2).
*   **Rule 5 (R-5):** Seluruh input valid & slot kosong, tetapi lapangan sedang berstatus `'maintenance'`. Sistem menolak pemesanan di pengecekan status (A3).
*   **Rule 6 (R-6):** Seluruh input & database valid. Booking terbuat dengan status awal `pending` (A4). Pengguna belum mengunggah bukti pembayaran.
*   **Rule 7 (R-7):** Booking dibuat (pending), pengguna mengunggah bukti transfer valid dalam waktu 1 jam. Admin menyetujui transaksi (A5). Status menjadi `approved`.
*   **Rule 8 (R-8):** Booking dibuat (pending), pengguna membiarkan transaksi tanpa pembayaran melebihi 1 jam. Sistem mengubah status transaksi menjadi `expired` (A6).
