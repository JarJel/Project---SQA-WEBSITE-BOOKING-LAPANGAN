# Desk Checking (Dry Run)

*Desk Checking* atau sering disebut *Dry Run* adalah metode peninjauan manual yang dilakukan oleh developer secara mandiri dengan membaca kode program secara perlahan dan menyimulasikan nilai variabel di kertas atau papan tulis berdasarkan baris instruksi logika program.

---

## 1. Proses Desk Checking

Langkah simulasi manual:
1.  Pilih fungsi logika kritis yang rawan bug.
2.  Tuliskan daftar variabel yang dideklarasikan pada kolom tabel.
3.  Simulasikan nilai variabel baris demi baris berdasarkan parameter masukan tertentu.

---

## 2. Simulasi Kueri Pengecekan Slot Bentrok (`checkCollision`)

Berikut adalah fungsi pengecekan tumpang tindih waktu sewa lapangan:

```php
function checkCollision($start_a, $end_a, $start_b, $end_b) {
    if ($start_a < $end_b && $end_a > $start_b) {
        return true; // Terjadi bentrok
    }
    return false; // Aman
}
```

### Tabel Penelusuran Variabel (Dry Run Table)

#### Kasus Uji 1: Pemesanan A (14:00 - 16:00), Pemesanan B (15:00 - 17:00)
*   **Masukan:** `$start_a = 14`, `$end_a = 16`, `$start_b = 15`, `$end_b = 17`

| Langkah | Baris Kode | Evaluasi Kondisi / Ekspresi | Nilai Return | Keterangan |
| :---: | :--- | :--- | :---: | :--- |
| 1 | `if ($start_a < $end_b && $end_a > $start_b)` | `14 < 17` (True) `&&` `16 > 15` (True) | - | Kondisi bernilai True |
| 2 | `return true;` | - | `true` | Program menghentikan eksekusi, mengembalikan True (Bentrok) |

#### Kasus Uji 2: Pemesanan A (14:00 - 15:00), Pemesanan B (15:00 - 16:00)
*   **Masukan:** `$start_a = 14`, `$end_a = 15`, `$start_b = 15`, `$end_b = 16`

| Langkah | Baris Kode | Evaluasi Kondisi / Ekspresi | Nilai Return | Keterangan |
| :---: | :--- | :--- | :---: | :--- |
| 1 | `if ($start_a < $end_b && ...)` | `14 < 16` (True) `&&` `15 > 15` (False) | - | Kondisi bernilai False |
| 2 | `return false;` | - | `false` | Program melewati blok if, mengembalikan False (Aman/Tidak bentrok) |
