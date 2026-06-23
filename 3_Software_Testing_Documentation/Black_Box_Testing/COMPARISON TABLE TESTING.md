# Comparison Table Testing

Pengujian Tabel Perbandingan (*Comparison Table Testing*) dilakukan untuk membandingkan kompatibilitas, performa, dan perilaku aplikasi booking lapangan olahraga di berbagai lingkungan browser dan perangkat yang berbeda (cross-browser / cross-platform testing).

---

## 1. Matriks Pengujian Lintas Browser (Cross-Browser)

| Peramban (Browser)                | Sistem Operasi | Tampilan Layout (CSS) | Fungsi Utama (JS) | Kinerja (Kecepatan) | Status Uji |
| :-------------------------------- | :------------- | :-------------------: | :---------------: | :-----------------: | :--------: |
| **Google Chrome (v120+)**   | Windows 11     |       Sempurna       |      Lancar      |    Sangat Cepat    |    Pass    |
| **Mozilla Firefox (v119+)** | Windows 11     |       Sempurna       |      Lancar      |        Cepat        |    Pass    |
| **Safari Mobile (iOS 17)**  | iOS (iPhone)   |       Responsif       |      Lancar      |    Sangat Cepat    |    Pass    |
| **Google Chrome Mobile**    | Android        |       Responsif       |      Lancar      |        Cepat        |    Pass    |
| **Microsoft Edge (v120+)**  | Windows 11     |       Sempurna       |      Lancar      |        Cepat        |    Pass    |

---

## 2. Parameter Detail Perbandingan Antar Fitur

| Fitur Utama                     | Ekspektasi Tampilan Desktop                             | Ekspektasi Tampilan Mobile                         | Perilaku Responsif                                       |
| :------------------------------ | :------------------------------------------------------ | :------------------------------------------------- | :------------------------------------------------------- |
| **Kalender Jadwal**       | Grid penuh (1 minggu tampak langsung)                   | List gulir samping (swipeable horizontal calendar) | Berubah layout otomatis berdasarkan lebar layar          |
| **Sidebar Menu Admin**    | Sidebar tetap di sisi kiri layar                        | Menu hamburger tersembunyi (drawer menu)           | Tombol toggle tampil di layar kecil (< 768px)            |
| **Tabel Riwayat Booking** | Kolom lengkap: ID, Lapangan, Tanggal, Jam, Status, Aksi | Kartu ringkas (card layout) per baris pemesanan    | Menyederhanakan detail kolom agar tidak terjadi overflow |
