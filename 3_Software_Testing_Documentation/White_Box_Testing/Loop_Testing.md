# Loop Testing (Pengujian Perulangan)

Pengujian Perulangan (*Loop Testing*) adalah teknik pengujian White Box yang berfokus sepenuhnya pada keabsahan logika struktur perulangan (`for`, `while`, `do-while`, `foreach`) di dalam program. Pengujian ini merancang kasus uji untuk mendeteksi bug terkait batasan iterasi (*off-by-one errors*, *infinite loops*).

---

## 1. Jenis Perulangan dan Strategi Uji

### A. Perulangan Sederhana (Simple Loops)
Misalkan perulangan dengan jumlah iterasi maksimum $N$ kali:
*   **Kasus Uji 1:** Lewati perulangan seluruhnya (0 iterasi).
*   **Kasus Uji 2:** Hanya lakukan 1 iterasi.
*   **Kasus Uji 3:** Lakukan 2 iterasi.
*   **Kasus Uji 4:** Jalankan iterasi sebanyak $M$ kali (di mana $M < N$).
*   **Kasus Uji 5:** Jalankan $N-1$ iterasi.
*   **Kasus Uji 6:** Jalankan $N$ iterasi.
*   **Kasus Uji 7:** Jalankan $N+1$ iterasi (jika logika perulangan memungkinkan).

---

## 2. Kode Contoh: Pengulangan Cek Ketersediaan Slot Hari Kerja

Fungsi di bawah ini memeriksa jadwal lapangan selama 5 hari kerja berturut-turut untuk menyaring ketersediaan:

```php
public function getAvailableDays($fieldId) {
    $availableDays = [];
    for ($day = 1; $day <= 5; $day++) { // Loop Sederhana (N = 5)
        if ($this->isFieldAvailable($fieldId, $day)) {
            $availableDays[] = $day;
        }
    }
    return $availableDays;
}
```

---

## 3. Matriks Kasus Uji Loop Testing

| ID Uji | Nilai Iterasi ($day$) | Detail Uji | Hasil Diharapkan | Status |
| :--- | :---: | :--- | :--- | :---: |
| **TC-LP-001** | 0 | Batalkan perulangan segera (inisialisasi `$day = 6`) | Loop dilewati, return array kosong | Belum Uji |
| **TC-LP-002** | 1 | Batasi loop agar hanya berputar 1 kali | Mengevaluasi hari ke-1 saja | Belum Uji |
| **TC-LP-003** | 2 | Jalankan loop tepat 2 kali | Mengevaluasi hari ke-1 dan hari ke-2 | Belum Uji |
| **TC-LP-004** | 4 ($N-1$) | Jalankan loop sebanyak 4 kali | Mengevaluasi hari ke-1 s.d hari ke-4 | Belum Uji |
| **TC-LP-005** | 5 ($N$) | Jalankan loop penuh 5 kali | Mengevaluasi seluruh hari kerja | Belum Uji |
