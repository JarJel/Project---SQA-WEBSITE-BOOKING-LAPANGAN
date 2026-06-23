# Boundary Value Analysis (BVA) Testing

Dokumen ini memuat pengujian nilai batas (*Boundary Value Analysis*) untuk field input kritis pada sistem booking lapangan. Pengujian berfokus pada nilai ekstrim (batas minimum, batas maksimum, tepat di batas, dan di luar batas).

---

## 1. Analisis Nilai Batas Input

### Field: Jam Sewa (Durasi)
*   **Batas Valid:** 1 jam s.d. 12 jam (per pemesanan).
*   **Nilai Batas yang Diuji:**
    *   Di bawah batas bawah (invalid): `0` jam, `-1` jam.
    *   Batas bawah (valid): `1` jam.
    *   Tepat di atas batas bawah (valid): `2` jam.
    *   Tepat di bawah batas atas (valid): `11` jam.
    *   Batas atas (valid): `12` jam.
    *   Di atas batas atas (invalid): `13` jam.

### Field: Nama Lengkap Pengguna (Registrasi)
*   **Batas Valid:** 3 s.d. 50 karakter.
*   **Nilai Batas yang Diuji:**
    *   Di bawah batas bawah (invalid): 2 karakter (misal: "Ab").
    *   Batas bawah (valid): 3 karakter (misal: "Adi").
    *   Batas atas (valid): 50 karakter.
    *   Di atas batas atas (invalid): 51 karakter.

---

## 2. Tabel Kasus Uji BVA

| ID Uji | Parameter Input | Nilai Input | Kategori | Hasil yang Diharapkan | Status |
| :--- | :--- | :---: | :--- | :--- | :---: |
| **TC-BVA-DUR-001** | Durasi Sewa | `-1` | Invalid (Out of bound) | Sistem memblokir & memunculkan error | Belum Uji |
| **TC-BVA-DUR-002** | Durasi Sewa | `0` | Invalid (Out of bound) | Sistem memblokir & memunculkan error | Belum Uji |
| **TC-BVA-DUR-003** | Durasi Sewa | `1` | Valid (Min) | Berhasil memesan | Belum Uji |
| **TC-BVA-DUR-004** | Durasi Sewa | `12` | Valid (Max) | Berhasil memesan | Belum Uji |
| **TC-BVA-DUR-005** | Durasi Sewa | `13` | Invalid (Out of bound) | Sistem memblokir & memunculkan error | Belum Uji |
| **TC-BVA-NAME-001**| Nama Pengguna | "Ab" (2 char) | Invalid (Out of bound) | Registrasi gagal, muncul error nama | Belum Uji |
| **TC-BVA-NAME-002**| Nama Pengguna | "Adi" (3 char)| Valid (Min) | Registrasi berhasil | Belum Uji |
