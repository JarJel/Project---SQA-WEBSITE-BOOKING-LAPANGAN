# Equivalence Partitioning Testing

Metode Pembagian Ekivalen (*Equivalence Partitioning*) membagi input data ke dalam beberapa partisi (kelas data) yang dianggap memiliki perilaku pengujian yang sama. Pengujian hanya perlu dilakukan pada satu perwakilan nilai dari masing-masing partisi untuk mengefisiensikan jumlah kasus uji.

---

## 1. Partisi Kelas Input Aplikasi Booking

### Input Field: Alamat Email (Login / Register)
*   **Partisi Valid:**
    1.  Email dengan format standar `[string]@[domain].[ekstensi]` (Contoh: `user@gmail.com`).
*   **Partisi Invalid:**
    2.  Tanpa karakter `@` (Contoh: `usergmail.com`).
    3.  Tanpa domain/ekstensi (Contoh: `user@gmail`).
    4.  Input kosong (Null).

### Input Field: Nomor Handphone (Registrasi)
*   **Partisi Valid:**
    1.  Angka saja, panjang 10 s.d 13 digit (Contoh: `081234567890`).
*   **Partisi Invalid:**
    2.  Mengandung karakter alfabet/khusus (Contoh: `0812-abc-909`).
    3.  Kurang dari 10 digit (Contoh: `0812345`).
    4.  Lebih dari 13 digit (Contoh: `0812345678901234`).

---

## 2. Rincian Kasus Uji Equivalence Partitioning

| ID Uji | Parameter Input | Nilai Pengujian | Kelas Partisi | Hasil Diharapkan | Status |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **TC-EQP-EM-001** | Email | `budi@test.com` | Valid (Format email standar) | Diterima oleh validasi | Belum Uji |
| **TC-EQP-EM-002** | Email | `buditest.com` | Invalid (Tanpa karakter `@`) | Ditolak, muncul pesan error | Belum Uji |
| **TC-EQP-EM-003** | Email | `budi@test` | Invalid (Tanpa domain lengkap)| Ditolak, muncul pesan error | Belum Uji |
| **TC-EQP-HP-001** | No. HP | `081234567890` | Valid (Angka saja, 12 digit) | Diterima oleh validasi | Belum Uji |
| **TC-EQP-HP-002** | No. HP | `0812abc` | Invalid (Mengandung huruf) | Ditolak, muncul pesan error | Belum Uji |
| **TC-EQP-HP-003** | No. HP | `08123` | Invalid (Terlalu pendek) | Ditolak, muncul pesan error | Belum Uji |
