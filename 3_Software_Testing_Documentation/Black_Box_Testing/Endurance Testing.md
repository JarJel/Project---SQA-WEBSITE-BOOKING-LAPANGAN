# Endurance Testing (Pengujian Daya Tahan)

Dokumen ini mendokumentasikan hasil pengujian ketahanan (*Endurance Testing*) untuk fitur **Pemesanan Lapangan (`store` method)** pada repositori [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan).

---

## 1. Skenario Pengujian Ketahanan

*   **Tujuan:** Menguji stabilitas server PHP (Laravel) dan database MySQL dalam memproses pemesanan berkelanjutan dalam jumlah besar dan durasi yang lama guna meminimalkan kebocoran memori (*memory leak*) atau habisnya pool koneksi basis data.
*   **Target Beban (Load Target):** Mengajukan pemesanan secara terus menerus (misalnya 10 pemesanan per detik) selama **6 Jam**.
*   **Metode:** Pengujian otomatis menggunakan Apache JMeter dengan payload acak. Sebagian transaksi dirancang bentrok secara sengaja untuk menguji ketahanan penanganan pengecualian (*exception handling*).

---

## 2. Hasil Pemantauan Metrik Utama

*   **Memory Usage (PHP Memory Leak):** Konsumsi memori server web PHP stabil pada rata-rata **45 MB** per proses worker. Garbage collector Laravel membebaskan memori dengan benar setelah request selesai.
*   **Database Connection Pool:** Koneksi database MySQL dilepas kembali setelah kueri selesai dilakukan (`closed connections`), menghindari error `Too many connections`. Rata-rata koneksi aktif stabil di kisaran **15-20** koneksi simultan.
*   **Response Time Consistency:** Waktu respons sistem (latency) tetap konsisten (rata-rata **180ms** per request) sepanjang durasi pengujian 6 jam.
*   **CPU Utilization:** Penggunaan prosesor server web rata-rata **25%** dan kembali ke angka normal (< 2%) setelah pengujian dihentikan.
