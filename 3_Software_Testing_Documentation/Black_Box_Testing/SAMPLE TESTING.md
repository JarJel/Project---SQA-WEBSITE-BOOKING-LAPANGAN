# Sample Testing (Pengujian Sampel)

Dokumen ini menjelaskan strategi pemilihan sampel (*Sample Testing*) yang representatif dari sekian banyak kemungkinan populasi data input untuk pengujian fungsionalitas sistem.

---

## 1. Pemilihan Sampel Pengujian

Karena keterbatasan waktu dan sumber daya, pengujian fungsionalitas pencarian lapangan tidak perlu dilakukan pada ribuan variasi kata kunci. Kita mengambil beberapa sampel representatif berikut:

### Sampel Karakter Input Pencarian Lapangan:
1.  **Sampel Valid Baku:** Kata kunci nama lapangan yang eksis (Contoh: `"Futsal"`, `"Basket"`).
2.  **Sampel Valid Sebagian (Partial):** Karakter pencarian tidak lengkap (Contoh: `"Fut"`, `"Bas"`).
3.  **Sampel Karakter Khusus (Special Characters):** Menguji ketahanan input (Contoh: `"!@#$"`, `"Futsal_A"`).
4.  **Sampel Huruf Besar/Kecil (Case Sensitivity):** Memastikan pencarian bersifat case-insensitive (Contoh: `"fUtSaL"`, `"BASKET"`).
5.  **Sampel Kosong / Whitespace:** Pencarian tanpa mengetik apa pun atau hanya spasi (Contoh: `"   "`).

---

## 2. Tabel Kasus Uji Sampel Pencarian

| ID Uji | Jenis Sampel | Input Kata Kunci | Ekspektasi Hasil | Status Uji |
| :--- | :--- | :--- | :--- | :---: |
| **TC-SMP-001** | Valid Baku | `"Futsal A"` | Menampilkan data Lapangan Futsal A | Pass |
| **TC-SMP-002** | Valid Partial | `"Fut"` | Menampilkan daftar lapangan futsal yang cocok | Pass |
| **TC-SMP-003** | Case-Insensitive | `"fUtSaL"` | Menampilkan lapangan futsal dengan tepat | Pass |
| **TC-SMP-004** | Tidak Eksis | `"Berenang"` | Menampilkan tulisan "Hasil tidak ditemukan" | Pass |
| **TC-SMP-005** | Karakter Khusus | `"@#$%"` | Menampilkan tulisan "Hasil tidak ditemukan" | Pass |
| **TC-SMP-006** | Whitespace | `"   "` | Menampilkan seluruh daftar lapangan semula | Pass |
