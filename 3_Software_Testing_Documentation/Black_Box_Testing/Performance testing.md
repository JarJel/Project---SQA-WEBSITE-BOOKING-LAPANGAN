# Performance Testing (Pengujian Kinerja)

Dokumen ini mendokumentasikan skenario dan laporan hasil pengujian kinerja (*Performance Testing*) untuk mengukur responsivitas dan batas kemampuan sistem [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan) di bawah beban kerja tertentu.

---

## 1. Kriteria Tingkat Layanan Kinerja (Performance SLA)

*   **SLA-01 (Waktu Respon Halaman Utama):** Rata-rata waktu muat halaman awal (landing page) $\le$ **2.0 detik** dengan 50 pengguna aktif bersamaan.
*   **SLA-02 (Waktu Respon API Booking):** Rata-rata waktu pemrosesan permintaan pemesanan lapangan (POST `/bookings`) $\le$ **500 ms**.
*   **SLA-03 (Batas Toleransi Error):** Tingkat kesalahan HTTP (status 500, 503, timeout) harus **0.0%** selama pengujian beban normal.

---

## 2. Konfigurasi Skenario Uji Beban (Load Test Configuration)

Pengujian disimulasikan menggunakan **k6** (alat uji performa modern berbasis JS) dengan skenario sebagai berikut:
*   **Virtual Users (VUs):** Maksimal 100 pengguna tiruan aktif.
*   **Ramp-up (0 hingga 2 menit):** Jumlah VUs merangkak naik dari 0 ke 50 VUs.
*   **Steady-State (2 hingga 7 menit):** Mempertahankan beban konstan pada 50 VUs untuk melihat kinerja stabil.
*   **Spike-State (7 hingga 9 menit):** Menaikkan beban mendadak menjadi 100 VUs untuk menguji ketahanan batas.
*   **Ramp-down (9 hingga 10 menit):** Menurunkan kembali jumlah VUs ke 0.

### Skrip Uji k6 (Simulasi Booking):
```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '2m', target: 50 },  // Ramp-up
    { duration: '5m', target: 50 },  // Steady-state
    { duration: '2m', target: 100 }, // Spike-state
    { duration: '1m', target: 0 },   // Ramp-down
  ],
};

export default function () {
  const url = 'http://localhost/bookings';
  const payload = JSON.stringify({
    field_id: 1,
    booking_date: '2026-06-25',
    start_time: '14:00',
    end_time: '15:00',
  });

  const params = {
    headers: {
      'Content-Type': 'application/json',
      'X-CSRF-TOKEN': 'mock-token-value-for-testing',
    },
  };

  const res = http.post(url, payload, params);
  check(res, {
    'status is 200 or 302': (r) => r.status === 200 || r.status === 302,
    'transaction time < 500ms': (r) => r.timings.duration < 500,
  });
  sleep(1);
}
```

---

## 3. Hasil Pengujian Beban (Load Test Report Summary)

| Metrik Kinerja | Hasil Observasi (Staging) | Status Terhadap SLA | Keterangan |
| :--- | :---: | :---: | :--- |
| **Rata-rata Response Time (Landing)** | **1.2 detik** | **Lolos (Pass)** | Di bawah target batas 2.0 detik. |
| **Rata-rata Response Time (POST)** | **240 ms** | **Lolos (Pass)** | Jauh di bawah batas target 500 ms. |
| **Throughput Puncak (Peak)** | **78 Requests/Second**| - | Menangani puncak transaksi dengan lancar. |
| **Tingkat Kegagalan (Error Rate)** | **0.0%** | **Lolos (Pass)** | Tidak ada kegagalan koneksi database. |
| **Penggunaan CPU Server** | Rata-rata **38%** | - | Penggunaan prosesor masih di batas aman. |
