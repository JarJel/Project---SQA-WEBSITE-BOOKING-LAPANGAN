# Basis Path Testing

Pengujian Jalur Basis (*Basis Path Testing*) adalah teknik pengujian White Box yang diajukan oleh Tom McCabe. Teknik ini memungkinkan perancang kasus uji untuk mengukur kompleksitas logis dari kode program dan mendefinisikan rangkaian jalur independen (*independent paths*) yang harus dieksekusi selama pengujian.

---

## 1. Kode yang Diuji: Validasi Kupon Diskon (`validateCoupon`)

```php
public function validateCoupon($code, $subtotal) {
    $discount = 0;
    if ($code == "PROMO10") {          // Node 1 (Kondisi 1)
        if ($subtotal >= 100000) {     // Node 2 (Kondisi 2)
            $discount = 10000;         // Node 3 (Aksi 1)
        } else {
            $discount = 0;             // Node 4 (Aksi 2)
        }
    } else {
        $discount = 0;                 // Node 5 (Aksi 3)
    }
    return $discount;                  // Node 6 (Selesai)
}
```

---

## 2. Diagram Alir Kontrol (Control Flow Graph - CFG)

Representasi graf dari kode di atas adalah sebagai berikut:
```text
       [Node 1] (code == "PROMO10")
        /     \
     (Ya)     (Tidak)
      /         \
  [Node 2]    [Node 5] (discount = 0)
 (subtotal     |
  >=100k)      |
   /   \       |
 (Ya) (Tidak)  |
  /     \      |
[Node 3] [Node 4] |
   \     /     /
  [  Node 6  ] (return discount)
```

---

## 3. Perhitungan Kompleksitas Siklomatis (Cyclomatic Complexity)

Rumus Kompleksitas Siklomatis $V(G)$ berdasarkan jumlah Predikat (Keputusan) $P$:
$$V(G) = P + 1$$

Di mana $P$ adalah jumlah node keputusan ($P = 2$, yaitu Node 1 dan Node 2):
$$V(G) = 2 + 1 = 3$$

Kompleksitas Siklomatis bernilai **3**, yang berarti terdapat **3 jalur independen** yang harus diuji secara terpisah untuk menjamin 100% path coverage.

---

## 4. Jalur Independen dan Kasus Uji

*   **Jalur 1 (Path 1):** Node 1 ➔ Node 5 ➔ Node 6
    *   *Input:* `code = "SALAH"`, `subtotal = 150000`
    *   *Output Diharapkan:* `discount = 0`
*   **Jalur 2 (Path 2):** Node 1 ➔ Node 2 ➔ Node 4 ➔ Node 6
    *   *Input:* `code = "PROMO10"`, `subtotal = 50000` (di bawah 100rb)
    *   *Output Diharapkan:* `discount = 0`
*   **Jalur 3 (Path 3):** Node 1 ➔ Node 2 ➔ Node 3 ➔ Node 6
    *   *Input:* `code = "PROMO10"`, `subtotal = 150000` (memenuhi syarat)
    *   *Output Diharapkan:* `discount = 10000`
