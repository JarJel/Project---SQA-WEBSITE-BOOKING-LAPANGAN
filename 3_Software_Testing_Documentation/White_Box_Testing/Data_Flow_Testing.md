# Data Flow Testing

Pengujian Alur Data (*Data Flow Testing*) berfokus pada siklus hidup data atau variabel di dalam kode program. Siklus ini dikenal dengan istilah **Definition-Use (DU) Chain**, yang meliputi pembuatan variabel (Define), penggunaan variabel (Use), dan penghancuran/penghapusan variabel (Kill).

---

## 1. Siklus Hidup Variabel (Def-Use-Kill)

*   **Definition (d):** Nilai diberikan ke suatu variabel (inisialisasi).
*   **Use (u):** Nilai variabel dibaca atau digunakan dalam perhitungan atau keputusan:
    *   *Computation Use (c-use):* Digunakan dalam operasi hitung/penugasan nilai lain.
    *   *Predicate Use (p-use):* Digunakan dalam logika ekspresi kondisional (misal: pernyataan `if`).
*   **Kill (k):** Ruang memori variabel dilepaskan atau dialokasikan ulang.

---

## 2. Kode Contoh: Fungsi Diskon Tambahan

```php
function getFinalPrice($price, $hasDiscount) {
    $total = $price;                       // Line 1: Define total (d)
    
    if ($hasDiscount) {                    // Line 2: p-use hasDiscount
        $total = $total - 5000;            // Line 3: c-use total & redefine total (u, d)
    }
    
    return $total;                         // Line 5: c-use total (u)
}
```

---

## 3. Rencana Pengujian Data Flow (DU-Chains Testing)

Penguji menganalisis relasi `d` dan `u` untuk memastikan tidak ada anomali penggunaan data:
1.  **Pengujian Rantai `total` (Line 1 to Line 3):** Memastikan jika `$hasDiscount` bernilai True, variabel `$total` yang didefinisikan pada Line 1 berhasil diolah di Line 3.
2.  **Pengujian Rantai `total` (Line 1 to Line 5):** Memastikan jika `$hasDiscount` bernilai False, variabel `$total` langsung dikembalikan di Line 5 tanpa melalui modifikasi Line 3.

### Anomali Data Flow yang Diperiksa:
*   **UR Anomali (Undefined Read):** Menggunakan variabel sebelum di-define.
*   **DU Anomali (Defined but not Used):** Variabel di-define namun nilainya ditimpa atau tidak pernah dibaca hingga program selesai.
