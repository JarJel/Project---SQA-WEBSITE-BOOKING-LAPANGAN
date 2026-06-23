# Formal Inspection (Inspeksi Formal) - White Box Testing

Dokumen ini mendokumentasikan hasil inspeksi formal (*Formal Fagan Inspection*) untuk peninjauan kualitas kode program pada repositori [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan). Pengujian ini menggunakan metode inspeksi formal terstruktur untuk mengidentifikasi cacat sebelum kode digabungkan (*merged*) ke cabang produksi.

---

## 1. Peran Tim Inspeksi Formal
*   **Moderator:** Bertanggung jawab atas penjadwalan sesi, memandu rapat inspeksi, memastikan proses berjalan teratur, dan melakukan tindak lanjut (*follow-up*) perbaikan.
*   **Reader (Pembaca):** Anggota tim yang memandu pembacaan baris demi baris kode program dengan menerangkan alur logika secara logis.
*   **Recorder (Pencatat/Scribe):** Bertanggung jawab mendaftarkan setiap cacat (*defect*) yang teridentifikasi ke dalam log inspeksi.
*   **Inspectors (Inspektur/QA):** Tim penguji senior yang menganalisis kode dari sudut pandang standar pemrograman, kinerja, dan keamanan siber.

---

## 2. Metrik Kualitas Sesi Inspeksi

Untuk menjamin efektivitas inspeksi, tim menggunakan pengukuran metrik terstandarisasi:
*   **Preparation Rate (Laju Persiapan):** Maksimal 120 baris kode (LOC) per jam untuk memastikan inspektur menganalisis kode secara mendalam sebelum rapat.
*   **Defect Density (Kerapatan Cacat):** Target jumlah penemuan cacat per 1.000 baris kode (KLOC).
*   **Kriteria Keluar Sesi (Exit Criteria):**
    1.  Semua temuan berkategori *Major* (Kritis/Tinggi) telah diperbaiki (*reworked*) dan diuji ulang (*regression tested*).
    2.  Semua temuan berkategori *Minor* telah memiliki jadwal rencana tindak lanjut perbaikan.
    3.  Modul program yang diinspeksi lolos uji integrasi lokal tanpa memicu error PHP.

---

## 3. Log Inspeksi Detail (Inspection Defect Log)

**Berkas yang Diinspeksi:** `app/Http/Controllers/BookingController.php` (Metode `uploadPayment` & `cancel`)  
**Tanggal Inspeksi:** 24 Juni 2026  

| No. Cacat | Lokasi Baris | Deskripsi Detil Cacat | Kategori (Major/Minor) | Klasifikasi Cacat | Dampak Terhadap Sistem | Status Perbaikan |
| :--- | :--- | :--- | :---: | :---: | :--- | :---: |
| **DEF-001** | Baris 88 | Pengecekan otorisasi pengguna (`booking->user_id !== Auth::id()`) melempar `abort(403)`. Namun, tidak ada log audit yang mencatat adanya upaya akses tidak sah ke sistem. | Minor | Log / Audit | Mempersulit pelacakan jika terjadi serangan peretasan akun. | **Closed** (Ditambahkan audit logging). |
| **DEF-002** | Baris 97 | Aturan validasi file bukti pembayaran `proof_of_payment` hanya menggunakan `mimes:jpeg,png,jpg`. Format extension `.webp` dan `.heic` (format iOS default) belum didukung. | Minor | Fungsional | Pengguna iPhone tidak dapat mengunggah screenshot bukti transfer secara langsung. | **Closed** (Ditambahkan format `webp` & `heic`). |
| **DEF-003** | Baris 105 | Penulisan file `$file->move(public_path('uploads/proofs'), $filename)` tidak membungkus transaksi dengan penanganan kegagalan penulisan disk server (*disk write failure handling*). Jika penyimpanan server penuh, data database akan ter-update tetapi file fisik tidak tersimpan (data rusak). | Major | Keamanan / Reliabilitas | Database tidak konsisten dengan direktori file fisik. | **Closed** (Dibungkus dengan try-catch & pembatalan update DB jika fail). |
| **DEF-004** | Baris 127 | Fungsi pembatalan `cancel()` tidak memeriksa apakah pemesanan telah berada pada tanggal yang sudah lampau. Pengguna dapat membatalkan pesanan lama untuk meminta refund ilegal. | Major | Logika Bisnis | Kerugian finansial bagi pemilik lapangan. | **Closed** (Ditambahkan validasi: `booking_date` harus $\ge$ hari ini). |
