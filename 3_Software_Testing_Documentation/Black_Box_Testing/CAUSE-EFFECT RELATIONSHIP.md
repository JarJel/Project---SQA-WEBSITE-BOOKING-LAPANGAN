# Cause-Effect Relationship Testing

Metode Cause-Effect Graphing/Relationship membantu mendokumentasikan kombinasi input (Causes) dan dampaknya terhadap output/perilaku sistem (Effects) dalam bentuk representasi logika terstruktur.

---

## 1. Daftar Sebab (Causes) & Akibat (Effects)

### Sebab (Causes):
*   **C1:** Email & Kata sandi yang dimasukkan terdaftar di database.
*   **C2:** Sesi login pengguna masih aktif (valid session).
*   **C3:** Lapangan futsal yang dipilih berstatus aktif (bukan maintenance).
*   **C4:** Slot waktu yang dipesan kosong (belum dipesan orang lain).
*   **C5:** Pengguna mengunggah bukti transfer nominal yang tepat sebelum 1 jam.

### Akibat (Effects):
*   **E1:** Login berhasil dan masuk ke dashboard.
*   **E2:** Login ditolak, pesan kesalahan ditampilkan.
*   **E3:** Pemesanan sementara berhasil dibuat (Status: Pending).
*   **E4:** Pemesanan diblokir oleh sistem (Error bentrok jadwal).
*   **E5:** Status booking berubah menjadi "Approved" oleh admin.
*   **E6:** Status booking dibatalkan secara otomatis (Expired).

---

## 2. Matriks Hubungan Cause-Effect

| Aturan (Rule) | Kondisi Sebab (Causes) | Hasil Akibat (Effects) | Deskripsi Kasus Nyata |
| :---: | :--- | :--- | :--- |
| **Rule 1** | C1 = TRUE | E1 | Login sukses menggunakan akun terdaftar. |
| **Rule 2** | C1 = FALSE | E2 | Login gagal karena email/password salah. |
| **Rule 3** | C2 = TRUE, C3 = TRUE, C4 = TRUE | E3 | Melakukan booking lapangan yang tersedia saat sedang login. |
| **Rule 4** | C2 = TRUE, C3 = TRUE, C4 = FALSE | E4 | Gagal booking karena jadwal bertabrakan. |
| **Rule 5** | C3 = TRUE, C5 = TRUE | E5 | Admin menyetujui transaksi setelah bukti transfer diverifikasi tepat waktu. |
| **Rule 6** | C5 = FALSE | E6 | Transaksi otomatis kedaluwarsa setelah 1 jam tanpa pembayaran. |
