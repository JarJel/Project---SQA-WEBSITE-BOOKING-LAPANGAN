# Decision Table Testing (Tabel Keputusan)

Pengujian Tabel Keputusan (*Decision Table Testing*) digunakan untuk memetakan kombinasi masukan logika biner (Benar/Salah) untuk memverifikasi keakuratan keputusan bisnis yang diambil oleh sistem booking lapangan.

---

## 1. Aturan Validasi Pemesanan Lapangan

| Kondisi / Masukan (Conditions) | Aturan 1 | Aturan 2 | Aturan 3 | Aturan 4 | Aturan 5 |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Apakah Pengguna Sudah Login?** | Y | N | Y | Y | Y |
| **Apakah Slot Jadwal Kosong/Tersedia?** | Y | - | N | Y | Y |
| **Apakah Status Lapangan Aktif?** | Y | - | Y | N | Y |
| **Apakah Pembayaran Berhasil Dikonfirmasi?**| Y | - | - | - | N |
| **Hasil / Aksi Sistem (Actions)** | | | | | |
| **Pemesanan Diterima & Disetujui (Approved)**| **X** | | | | |
| **Pemesanan Ditolak & Tampil Pesan Error** | | **X** | **X** | **X** | |
| **Pemesanan Ditangguhkan (Pending/Expired)**| | | | | **X** |

---

## 2. Keterangan Aturan (Rule Description)
*   **Aturan 1 (Pemesanan Berhasil):** Pengguna login, lapangan aktif, slot waktu kosong, pembayaran valid. Hasil: Lapangan resmi dibooking (Status: Approved).
*   **Aturan 2 (Akses Ditolak):** Pengguna belum login namun mencoba melakukan pemesanan. Hasil: Diarahkan ke halaman login dengan pesan kesalahan.
*   **Aturan 3 (Bentrok Jadwal):** Lapangan aktif dan pengguna login, tetapi jam tersebut sudah dibooking orang lain. Hasil: Sistem memblokir formulir pemesanan dengan peringatan bentrok.
*   **Aturan 4 (Lapangan Nonaktif):** Pengguna login dan slot kosong, namun lapangan dalam status perawatan (*Maintenance*). Hasil: Lapangan tidak dapat diklik atau dipesan.
*   **Aturan 5 (Gagal Bayar):** Pengguna login, lapangan aktif, slot kosong, namun tidak ada konfirmasi pembayaran masuk dalam 1 jam. Hasil: Pemesanan kadaluwarsa (Expired).
