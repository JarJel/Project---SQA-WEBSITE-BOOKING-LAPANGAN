# Behaviour Testing (Pengujian Perilaku)

Dokumen ini mendokumentasikan pengujian perilaku sistem (*Behaviour Testing*) untuk memastikan sistem memberikan respon yang sesuai terhadap interaksi pengguna, transisi status (*state transition*), dan alur kerja bisnis.

---

## 1. Diagram Transisi Status Booking (State Transition Table)

Fitur utama yang diuji perilakunya adalah status transaksi pemesanan lapangan:

| Status Awal | Aksi / Peristiwa (Event) | Status Akhir | Keterangan |
| :--- | :--- | :--- | :--- |
| **None** | Pengguna mengisi form & klik "Pesan" | **Pending** | Booking dibuat, menunggu pembayaran. |
| **Pending** | Batas waktu 1 jam habis tanpa pembayaran | **Expired** | Booking otomatis dibatalkan oleh sistem. |
| **Pending** | Pengguna mengunggah bukti transfer valid | **Verifying** | Menunggu verifikasi admin. |
| **Verifying** | Admin menyetujui bukti transfer | **Approved** | Lapangan resmi dipesan. |
| **Verifying** | Admin menolak bukti transfer | **Rejected** | Bukti salah, pengguna diminta re-upload. |
| **Approved** | Jadwal sewa telah selesai dilaksanakan | **Completed** | Pemakaian lapangan selesai. |

---

## 2. Kasus Uji Perilaku (Behaviour Test Cases)

### TC-BEH-001: Alur Normal Transaksi Sukses
*   **Skenario:** Pengguna melakukan pemesanan, membayar tepat waktu, dan disetujui admin.
*   **Langkah:**
    1. Lakukan pemesanan lapangan futsal untuk besok jam 10:00-11:00. (Status: **Pending**)
    2. Upload bukti transfer asli. (Status: **Verifying**)
    3. Login sebagai admin, klik "Approve". (Status: **Approved**)
*   **Hasil Diharapkan:** Transaksi berhasil diselesaikan, slot jadwal terkunci untuk pengguna lain, dan email notifikasi terkirim.

### TC-BEH-002: Transaksi Kedaluwarsa (Timeout)
*   **Skenario:** Pengguna memesan tetapi tidak membayar dalam waktu 1 jam.
*   **Langkah:**
    1. Lakukan pemesanan lapangan. (Status: **Pending**)
    2. Diamkan sistem selama > 60 menit.
*   **Hasil Diharapkan:** Status berubah menjadi **Expired**, slot waktu dibebaskan kembali untuk dipesan orang lain.
