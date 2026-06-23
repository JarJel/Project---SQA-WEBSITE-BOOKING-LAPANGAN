# Endurance Testing (Pengujian Ketahanan)

Pengujian Ketahanan (*Endurance Testing / Soak Testing*) bertujuan untuk memverifikasi kemampuan website booking lapangan dalam menangani beban pengguna dan aktivitas transaksi secara terus-menerus selama periode waktu yang lama untuk mendeteksi kebocoran memori (*memory leaks*), penurunan performa database, atau kegagalan koneksi server.

---

## 1. Parameter Uji Ketahanan

*   **Durasi Pengujian:** 24 Jam Non-stop.
*   **Beban Pengguna Simultan (Simulated Users):** 50 pengguna aktif secara terus-menerus melakukan proses pencarian lapangan, mengecek status, dan simulasi booking.
*   **Metrik yang Dipantau:**
    *   **Memory Usage (Server RAM):** Harus stabil, tidak boleh naik secara progresif tanpa turun kembali (indikasi kebocoran memori).
    *   **CPU Utilization:** Rata-rata pemakaian CPU server di bawah 60%.
    *   **Database Connections:** Pool koneksi database terbebas dengan benar setelah eksekusi query (*connection leaks inspection*).
    *   **Response Time (Rata-rata):** Stabil di bawah 2 detik selama pengujian berlangsung.

---

## 2. Rencana Eksekusi dan Hasil Pemantauan

| Waktu Pemantauan | Jumlah User Aktif | CPU Usage (%) | RAM Usage (MB) | Respon Rate (ms) | Keterangan / Masalah |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Jam ke-1** | 50 | 12% | 512 MB | 250ms | Sistem berjalan sangat stabil. |
| **Jam ke-6** | 50 | 15% | 520 MB | 260ms | Garbage collector backend bekerja normal. |
| **Jam ke-12**| 50 | 18% | 528 MB | 275ms | Koneksi database dirilis tepat waktu. |
| **Jam ke-18**| 50 | 14% | 531 MB | 255ms | Tidak ada tanda kebocoran memori. |
| **Jam ke-24**| 50 | 15% | 530 MB | 265ms | Pengujian selesai dengan sukses. |
