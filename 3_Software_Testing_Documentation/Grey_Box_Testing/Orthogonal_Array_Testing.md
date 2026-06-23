# Orthogonal Array Testing (OATS) - Grey Box Testing

Dokumen ini mendokumentasikan penerapan teknik pengujian larik ortogonal (*Orthogonal Array Testing*) pada sistem [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan). OATS digunakan untuk menyederhanakan kombinasi pengujian multi-variabel (tipe lapangan, metode pembayaran, dan jenis hak akses pengguna) agar didapatkan hasil optimal dengan jumlah kasus uji yang minim.

---

## 1. Analisis Faktor dan Level Pengujian

Ketika merancang pemesanan lapangan, terdapat 3 variabel (Faktor) yang saling mempengaruhi alur transaksi, di mana masing-masing faktor memiliki 3 nilai pilihan (Level):

1.  **Faktor A: Jenis Lapangan Olahraga**
    *   Level 0: Futsal (Tarif: Rp 150.000 / Jam)
    *   Level 1: Badminton (Tarif: Rp 50.000 / Jam)
    *   Level 2: Basket (Tarif: Rp 100.000 / Jam)
2.  **Faktor B: Metode Pembayaran yang Digunakan**
    *   Level 0: Transfer Bank (Manual)
    *   Level 1: E-Wallet / Qris (Otomatis)
    *   Level 2: Kartu Kredit (Instant Payment)
3.  **Faktor C: Peran & Jenis Pengguna (Role / Membership)**
    *   Level 0: Pengguna Biasa / Tamu (Tanpa Diskon)
    *   Level 1: Member Biasa (Diskon Otomatis 5% di Backend)
    *   Level 2: Member VIP (Diskon Otomatis 10% di Backend)

Jika diuji secara permutasi penuh, total uji adalah $3^3 = 27$ kasus uji. Menggunakan rancangan larik ortogonal standar **$L_9(3^3)$**, kasus uji dipotong secara statistik menjadi hanya **9 kasus uji** namun tetap menjamin seluruh interaksi berpasangan (*pairwise coverage*).

---

## 2. Tabel Kasus Uji Larik Ortogonal $L_9(3^3)$

Tabel pemetaan kombinasi faktor-level masukan beserta evaluasi hitung backend dan integritas data database:

| ID Uji | Faktor A (Lapangan) | Faktor B (Pembayaran) | Faktor C (Pengguna) | Input Durasi Uji | Perhitungan Tarif Backend | Expected Database Record (`bookings`) |
| :--- | :--- | :--- | :--- | :---: | :--- | :--- |
| **TC-OAT-001** | Futsal (L-0) | Transfer Bank (L-0) | Tamu (L-0) | 2 Jam | $2 \times 150k \times 1.0$ | `total_price = 300000`<br>`status = 'pending'` |
| **TC-OAT-002** | Futsal (L-0) | E-Wallet (L-1) | Member Biasa (L-1) | 2 Jam | $2 \times 150k \times 0.95$ | `total_price = 285000`<br>`status = 'pending'` |
| **TC-OAT-003** | Futsal (L-0) | Kartu Kredit (L-2) | Member VIP (L-2) | 2 Jam | $2 \times 150k \times 0.9$ | `total_price = 270000`<br>`status = 'pending'` |
| **TC-OAT-004** | Badminton (L-1) | Transfer Bank (L-0) | Member Biasa (L-1) | 2 Jam | $2 \times 50k \times 0.95$ | `total_price = 95000`<br>`status = 'pending'` |
| **TC-OAT-005** | Badminton (L-1) | E-Wallet (L-1) | Member VIP (L-2) | 2 Jam | $2 \times 50k \times 0.9$ | `total_price = 90000`<br>`status = 'pending'` |
| **TC-OAT-006** | Badminton (L-1) | Kartu Kredit (L-2) | Tamu (L-0) | 2 Jam | $2 \times 50k \times 1.0$ | `total_price = 100000`<br>`status = 'pending'` |
| **TC-OAT-007** | Basket (L-2) | Transfer Bank (L-0) | Member VIP (L-2) | 2 Jam | $2 \times 100k \times 0.9$ | `total_price = 180000`<br>`status = 'pending'` |
| **TC-OAT-008** | Basket (L-2) | E-Wallet (L-1) | Tamu (L-0) | 2 Jam | $2 \times 100k \times 1.0$ | `total_price = 200000`<br>`status = 'pending'` |
| **TC-OAT-009** | Basket (L-2) | Kartu Kredit (L-2) | Member Biasa (L-1) | 2 Jam | $2 \times 100k \times 0.95$ | `total_price = 190000`<br>`status = 'pending'` |

---

## 3. Prosedur Pengujian Larik Ortogonal
1.  **Eksekusi Request:** Kirim HTTP Request menggunakan Postman runner dengan payload yang didefinisikan pada setiap baris TC-OAT.
2.  **Pemeriksaan Respon API:** Pastikan respon JSON mengembalikan total harga (`total_price`) yang sesuai dengan kolom "Perhitungan Tarif Backend".
3.  **Pemeriksaan Database:** Jalankan kueri SQL `SELECT total_price, status FROM bookings WHERE user_id = ? ORDER BY id DESC LIMIT 1;` untuk mengonfirmasi nilai yang tertulis di database.
4.  **Tingkat Kelulusan:** 100% dari 9 kasus uji harus menghasilkan data presisi di tingkat API dan Database.
