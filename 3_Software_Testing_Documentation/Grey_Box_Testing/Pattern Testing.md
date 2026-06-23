# Pattern Testing (Pengujian Pola)

Pengujian Pola (*Pattern Testing*) pada pengujian Grey Box berfokus pada analisis pola perilaku sistem, pola arsitektur kode (misalnya MVC/Repository), format respon data (seperti skema JSON), dan pola ancaman keamanan.

---

## 1. Pengujian Pola Struktur Respon API (JSON Schema Testing)

Pada pengujian ini, tim QA memeriksa apakah API sistem secara konsisten mengembalikan pola struktur data yang seragam untuk setiap endpoint.

### Contoh Pola Struktur Respon Sukses:
Setiap endpoint API sukses harus mengembalikan pola schema berikut:
```json
{
  "success": true,
  "message": "Pesan sukses operasi",
  "data": { ... }
}
```

### Contoh Pola Struktur Respon Error:
Setiap endpoint API yang memicu validasi error harus mengembalikan pola:
```json
{
  "success": false,
  "message": "Validasi gagal",
  "errors": {
    "nama_field": [
      "Pesan detail kesalahan"
    ]
  }
}
```

---

## 2. Pengujian Pola Log Aktivitas Keamanan (Log Pattern Testing)

Penguji memiliki akses sebagian ke file log server (`storage/logs/laravel.log` atau `access.log`) untuk memvalidasi pola pesan error:
*   **Pola Uji:** Melakukan request login salah sebanyak 5 kali berturut-turut.
*   **Verifikasi Log:** Memastikan tercatat baris log dengan pola tingkat peringatan (Warning/Notice) berisi teks: `[Security] Suspicious login attempt for email: user@email.com - IP: 192.168.1.50`.
