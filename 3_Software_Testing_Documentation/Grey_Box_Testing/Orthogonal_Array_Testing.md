# Orthogonal Array Testing (OATS)

Pengujian Larik Ortogonal (*Orthogonal Array Testing*) adalah teknik pengujian kombinatorial yang sistematis untuk menguji sistem dengan banyak kombinasi variabel input menggunakan jumlah kasus uji minimum yang optimal.

---

## 1. Parameter Kombinasi Pemesanan Lapangan

Misalkan kita ingin menguji proses pemesanan dengan 3 variabel bebas (faktor) yang masing-masing memiliki beberapa nilai pilihan (level):
1.  **Faktor A: Jenis Lapangan** (Level: Futsal, Bulutangkis, Basket) -> 3 Level
2.  **Faktor B: Cara Pembayaran** (Level: Transfer Bank, E-Wallet, Kartu Kredit) -> 3 Level
3.  **Faktor C: Tipe Pengguna** (Level: Tamu/Guest, Member Biasa, Member VIP) -> 3 Level

Jika diuji secara penuh, total kombinasi adalah $3 \times 3 \times 3 = 27$ kasus uji. Menggunakan matriks larik ortogonal $L_9(3^3)$, kita dapat memangkasnya menjadi hanya **9 kasus uji** namun tetap mencakup seluruh interaksi berpasangan (*pairwise coverage*).

---

## 2. Tabel Kasus Uji Larik Ortogonal $L_9(3^3)$

| No. Uji | Faktor A (Lapangan) | Faktor B (Pembayaran) | Faktor C (Pengguna) | Ekspektasi Respon Backend / Status |
| :---: | :--- | :--- | :--- | :--- |
| **TC-OAT-001** | Futsal | Transfer Bank | Guest | Berhasil, Tanpa Diskon |
| **TC-OAT-002** | Futsal | E-Wallet | Member Biasa | Berhasil, Diskon 5% |
| **TC-OAT-003** | Futsal | Kartu Kredit | Member VIP | Berhasil, Diskon 10% |
| **TC-OAT-004** | Bulutangkis | Transfer Bank | Member Biasa | Berhasil, Diskon 5% |
| **TC-OAT-005** | Bulutangkis | E-Wallet | Member VIP | Berhasil, Diskon 10% |
| **TC-OAT-006** | Bulutangkis | Kartu Kredit | Guest | Berhasil, Tanpa Diskon |
| **TC-OAT-007** | Basket | Transfer Bank | Member VIP | Berhasil, Diskon 10% |
| **TC-OAT-008** | Basket | E-Wallet | Guest | Berhasil, Tanpa Diskon |
| **TC-OAT-009** | Basket | Kartu Kredit | Member Biasa | Berhasil, Diskon 5% |
