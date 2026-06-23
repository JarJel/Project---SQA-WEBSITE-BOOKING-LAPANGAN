# Comparison Table Testing

Pengujian Tabel Perbandingan (*Comparison Table Testing*) dilakukan untuk mengevaluasi dua aspek perbandingan utama pada sistem booking lapangan:
1.  **Evaluasi Logika Bisnis (Overlap vs Database):** Membandingkan data parameter permintaan (*request parameters*) dengan record database untuk memvalidasi algoritma pencegahan jadwal bentrok.
2.  **Kompatibilitas Tampilan Lintas Peramban (Cross-Browser):** Membandingkan konsistensi rendering antarmuka pengguna (UI/CSS) di berbagai browser desktop dan mobile.

---

## 1. Perbandingan Evaluasi Logika Overlap Basis Data

Formula kueri sql overlap pada Laravel:
```php
$query->where('start_time', '<', $endTime)
      ->where('end_time', '>', $startTime);
```

Tabel di bawah ini menguji berbagai kemungkinan irisan waktu pemesanan baru terhadap data pemesanan yang telah disetujui (`approved`/`pending`) di database untuk tanggal yang sama:

| No. Kasus | Hubungan Waktu Uji | Jam Request Baru | Jam Eksis di DB | Evaluasi Formula Logika | Hasil Evaluasi Sistem |
| :---: | :--- | :---: | :---: | :--- | :---: |
| **1** | **Bentrok Total (Identik)** | `10:00 - 12:00` | `10:00 - 12:00` | `10:00 < 12:00` (True) AND `12:00 > 10:00` (True) | **Ditolak (Bentrok)** |
| **2** | **Irisan Awal (Partial Start)** | `09:00 - 11:00` | `10:00 - 12:00` | `09:00 < 12:00` (True) AND `11:00 > 10:00` (True) | **Ditolak (Bentrok)** |
| **3** | **Irisan Akhir (Partial End)** | `11:00 - 13:00` | `10:00 - 12:00` | `11:00 < 12:00` (True) AND `13:00 > 10:00` (True) | **Ditolak (Bentrok)** |
| **4** | **Irisan Penuh (Melingkupi)** | `09:00 - 13:00` | `10:00 - 12:00` | `09:00 < 12:00` (True) AND `13:00 > 10:00` (True) | **Ditolak (Bentrok)** |
| **5** | **Irisan Internal (Di Dalam)** | `10:30 - 11:30` | `10:00 - 12:00` | `10:30 < 12:00` (True) AND `11:30 > 10:00` (True) | **Ditolak (Bentrok)** |
| **6** | **Berdampingan Sebelum (Saling Menyentuh)** | `08:00 - 10:00` | `10:00 - 12:00` | `08:00 < 12:00` (True) AND `10:00 > 10:00` (False) | **Diterima (Sukses)** |
| **7** | **Berdampingan Sesudah (Saling Menyentuh)** | `12:00 - 14:00` | `10:00 - 12:00` | `12:00 < 12:00` (False) AND `14:00 > 10:00` (True) | **Diterima (Sukses)** |
| **8** | **Jadwal Terpisah Jauh** | `15:00 - 17:00` | `10:00 - 12:00` | `15:00 < 12:00` (False) AND `17:00 > 10:00` (True) | **Diterima (Sukses)** |

---

## 2. Perbandingan Kompatibilitas Antarmuka (Cross-Browser Layout)

Membandingkan konsistensi visual dan interaksi elemen JS (calendar picker, upload modal, sidebar) di berbagai peramban:

| Fitur / Elemen UI | Chrome v120+ (Win/Mac) | Firefox v119+ (Win/Linux) | Safari iOS 17 (Mobile) | Chrome Mobile (Android) |
| :--- | :--- | :--- | :--- | :--- |
| **DatePicker Calendar** | Tampil native dialog kalender Chrome. CSS responsif. | Tampil native dialog kalender Firefox. CSS responsif. | Tampil roda pemilih (*wheel picker*) native iOS. | Tampil dialog kalender pop-up default Android. |
| **Tabel Grid Jadwal** | Lebar kolom menyesuaikan layar secara dinamis (Flexbox). | Grid rendering sempurna. | Render normal. Terkadang muncul scroll horizontal jika baris panjang. | Otomatis berganti ke kartu vertikal (*card list*) untuk layar kecil. |
| **Tombol Unggah Bukti**| Animasi transisi hover/focus lancar (0.2s transition). | Render normal. | Render normal, mendukung input kamera langsung untuk foto bukti. | Render normal, mendukung pemilih file gambar/kamera. |
| **Sidebar Admin Menu** | Sidebar tetap di kiri (`fixed-left`). | Sidebar rendering normal. | Sidebar otomatis tersembunyi, terbuka via tombol hamburger. | Sidebar tersembunyi, terbuka via swipe/hamburger menu. |
