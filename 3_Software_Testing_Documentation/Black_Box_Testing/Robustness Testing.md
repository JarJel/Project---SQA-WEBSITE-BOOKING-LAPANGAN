# Robustness Testing (Pengujian Ketangguhan)

Pengujian Ketangguhan (*Robustness Testing*) bertujuan untuk memverifikasi kemampuan website dalam menangani input yang tidak biasa, kondisi tidak terduga, atau kesalahan pengguna secara anggun (*graceful degradation*) tanpa menyebabkan sistem crash, hang, atau membocorkan data sensitif.

---

## 1. Skenario Pengujian Ketangguhan

### TC-ROB-001: Pengunggahan File Bukti Transfer Berformat Tidak Valid
*   **Skenario:** Pengguna mengunggah file bukti transfer dengan format selain gambar (misal: script berbahaya `.php` atau file `.pdf`).
*   **Hasil yang Diharapkan:** Sistem memblokir pengunggahan, menampilkan pesan kesalahan "Format file tidak didukung (Hanya diperbolehkan JPG, JPEG, PNG)", dan tidak menyimpan file tersebut ke direktori server.

### TC-ROB-002: Manipulasi URL Parameter (ID Lapangan Tidak Eksis)
*   **Skenario:** Pengguna sengaja memodifikasi parameter ID lapangan pada URL menjadi nilai yang tidak valid (misal: `https://bookinglapangan.domain.com/fields/999999` atau `https://bookinglapangan.domain.com/fields/abc`).
*   **Hasil yang Diharapkan:** Sistem tidak menampilkan error debug database, melainkan mengalihkan pengguna ke halaman kesalahan standar **404 Not Found** dengan desain yang rapi.

### TC-ROB-003: Pemutusan Koneksi Internet Mendadak saat Pemesanan
*   **Skenario:** Menurunkan/memutus jaringan internet perangkat klien tepat saat pengguna menekan tombol "Pesan Sekarang".
*   **Hasil yang Diharapkan:** Frontend menampilkan pesan indikator "Gagal terhubung ke server, silakan periksa koneksi internet Anda" tanpa membuat tombol pesanan terkunci selamanya (pengguna dapat mencoba ulang setelah koneksi kembali).
