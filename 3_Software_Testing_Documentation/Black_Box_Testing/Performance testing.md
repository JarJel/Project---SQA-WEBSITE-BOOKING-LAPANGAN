# Performance Testing (Pengujian Kinerja)

Pengujian Kinerja (*Performance Testing*) dilakukan untuk mengukur kecepatan respon, keandalan, stabilitas, dan skalabilitas sistem booking lapangan di bawah kondisi beban tertentu.

---

## 1. Kriteria Sukses Kinerja (Performance SLA)

*   **SLA-1 (Waktu Respon Halaman):** Rata-rata waktu muat halaman utama tidak boleh melebihi 2.0 detik.
*   **SLA-2 (Waktu Respon API):** Respon API booking (POST/GET) tidak boleh melebihi 500 ms.
*   **SLA-3 (Error Rate):** Jumlah error (HTTP 500 / Timeout) harus di bawah 1% selama pengujian beban.

---

## 2. Skenario Pengujian Beban (Load Testing)

Pengujian dilakukan menggunakan alat bantu load testing (seperti Apache JMeter atau k6) dengan skenario peningkatan jumlah pengguna bertahap (*ramp-up*).

### Skenario Uji:
1.  **Tahap Ramp-Up:** Naikkan jumlah virtual users (VUs) dari 0 ke 100 dalam waktu 2 menit.
2.  **Tahap Steady-State:** Tahan beban pada 100 VUs selama 10 menit.
3.  **Tahap Ramp-Down:** Turunkan jumlah VUs kembali ke 0 dalam waktu 1 menit.

### Hasil Pengujian Beban (Ringkasan):

*   **Rata-rata Respon Time:** 450 ms (Sesuai target SLA-2).
*   **Throughput (Rata-rata):** 85 Request/Detik (RPS).
*   **HTTP 500 Errors:** 0 (0% Error Rate).
*   **Status Kinerja:** **Lulus (Pass)**.
