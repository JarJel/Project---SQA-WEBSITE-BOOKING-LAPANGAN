# Software User Documentation
## Panduan Pengguna (User Guide) - Website Booking Lapangan

Selamat datang di Panduan Pengguna **Website Booking Lapangan**. Panduan ini dirancang untuk membantu Anda memahami cara menggunakan aplikasi mulai dari pendaftaran akun hingga proses pemesanan lapangan.

---

## 1. Persyaratan Sistem (System Requirements)
Untuk dapat menggunakan aplikasi ini dengan lancar, pastikan perangkat Anda memenuhi spesifikasi berikut:
*   Koneksi Internet yang stabil.
*   Browser web modern yang terinstal (Google Chrome, Mozilla Firefox, Apple Safari, atau Microsoft Edge versi terbaru).
*   Resolusi layar optimal minimal 360px (mobile) hingga 1920px (desktop).

---

## 2. Cara Memulai

### 2.1 Pendaftaran Akun (Register)
Jika Anda adalah pengguna baru, Anda harus membuat akun terlebih dahulu:
1.  Buka browser dan akses URL: `https://bookinglapangan.domain.com`.
2.  Klik tombol **Daftar** di pojok kanan atas halaman.
3.  Isi formulir pendaftaran dengan data berikut:
    *   **Nama Lengkap:** Nama Anda yang akan tercantum pada nota pemesanan.
    *   **Email:** Gunakan alamat email aktif untuk verifikasi dan menerima struk pemesanan.
    *   **Kata Sandi:** Minimal 8 karakter, disarankan kombinasi huruf dan angka.
    *   **Konfirmasi Kata Sandi:** Masukkan ulang kata sandi yang sama.
4.  Klik tombol **Daftar Sekarang**. Anda akan otomatis dialihkan ke halaman utama dengan status akun aktif.

### 2.2 Masuk ke Aplikasi (Login)
Untuk masuk kembali menggunakan akun yang sudah dibuat:
1.  Klik tombol **Masuk / Login** di pojok kanan atas.
2.  Masukkan **Email** dan **Kata Sandi** Anda.
3.  (Opsional) Centang opsi *Ingat Saya* agar sesi Anda tetap aktif di perangkat ini.
4.  Klik tombol **Login**.

---

## 3. Alur Pemesanan Lapangan (Booking Flow)

Ikuti langkah-langkah di bawah ini untuk menyewa lapangan olahraga:

```text
[Cari Lapangan] ➔ [Pilih Tanggal & Jam] ➔ [Klik Pesan] ➔ [Lakukan Pembayaran] ➔ [Konfirmasi Pembayaran]
```

### 3.1 Mencari Lapangan yang Tersedia
1.  Di Halaman Utama, gulir ke bawah ke bagian **Daftar Lapangan**.
2.  Gunakan filter pencarian berdasarkan *Jenis Lapangan* (Futsal, Bulutangkis, Basket) untuk mempersempit pencarian.
3.  Klik pada kartu lapangan untuk melihat informasi detail, foto, harga per jam, dan fasilitas yang tersedia.

### 3.2 Melakukan Pemesanan (Booking)
1.  Pada halaman detail lapangan, Anda akan melihat kalender ketersediaan jadwal.
2.  **Pilih Tanggal** pemesanan yang Anda inginkan.
3.  **Pilih Slot Waktu** (jam mulai dan jam selesai). Slot waktu yang berwarna merah berarti sudah dipesan oleh orang lain.
4.  Sistem akan menampilkan total biaya secara otomatis.
5.  Klik tombol **Pesan Sekarang**. Anda akan diarahkan ke halaman detail transaksi pemesanan.

### 3.3 Pembayaran dan Konfirmasi
1.  Transfer biaya sewa ke rekening bank yang tertera pada halaman rincian transaksi sebelum batas waktu habis (1 jam).
2.  Setelah mentransfer, ambil foto/screenshot bukti transfer.
3.  Masuk ke menu **Riwayat Pemesanan** (Booking History) di profil Anda.
4.  Klik tombol **Upload Bukti Transfer** pada pemesanan yang bersangkutan.
5.  Unggah gambar bukti transfer dan klik **Kirim**.
6.  Tunggu admin melakukan verifikasi. Jika disetujui, status pemesanan akan berubah menjadi **Disetujui / Approved** dan Anda akan menerima email konfirmasi.

---

## 4. Panduan Khusus Administrator (Admin Guide)

Jika Anda masuk menggunakan akun dengan peran **Admin**, Anda akan memiliki akses ke dashboard khusus (`/admin/dashboard`).

### 4.1 Mengelola Data Lapangan
*   **Tambah Lapangan Baru:** Buka menu *Kelola Lapangan* -> Klik *Tambah Lapangan* -> Isi nama, jenis lapangan, harga, deskripsi, dan upload foto -> Klik *Simpan*.
*   **Edit Lapangan:** Klik ikon pensil di samping nama lapangan -> Ubah data -> Klik *Update*.
*   **Nonaktifkan Lapangan:** Jika lapangan sedang direnovasi, Anda dapat mengubah statusnya menjadi *Maintenance* agar tidak dapat dipesan pengguna.

### 4.2 Memverifikasi Pemesanan Pengguna
1.  Buka menu **Kelola Pemesanan** atau **Persetujuan Pembayaran**.
2.  Anda akan melihat daftar pemesanan baru yang berstatus *Pending / Perlu Verifikasi*.
3.  Klik **Lihat Bukti** untuk memeriksa kecocokan bukti transfer pengguna.
4.  Jika dana sudah masuk dan bukti valid, klik tombol **Setujui / Approve**.
5.  Jika bukti tidak valid atau dana tidak masuk, klik **Tolak / Reject** dan masukkan alasan penolakan (misal: "Bukti transfer tidak terbaca").

---

## 5. Pertanyaan yang Sering Diajukan (FAQ)

**Tanya: Mengapa pemesanan saya ditolak?**  
*Jawab:* Pemesanan biasanya ditolak jika bukti transfer yang diunggah tidak valid, nominal tidak sesuai, atau pembayaran melewati batas waktu 1 jam. Silakan hubungi Customer Service kami jika Anda merasa terjadi kesalahan.

**Tanya: Apakah saya bisa membatalkan pemesanan yang sudah disetujui?**  
*Jawab:* Kebijakan pembatalan bergantung pada aturan pengelola lapangan. Harap lakukan pembatalan minimal 24 jam sebelum jadwal main melalui Customer Service untuk opsi pengembalian dana (refund) atau reschedule.
