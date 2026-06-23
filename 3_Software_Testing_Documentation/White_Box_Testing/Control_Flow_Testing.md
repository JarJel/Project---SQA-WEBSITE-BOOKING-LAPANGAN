# Control Flow Testing

Pengujian Alur Kontrol (*Control Flow Testing*) berfokus pada eksekusi terstruktur dari pernyataan kendali dalam program. Tujuannya adalah mengukur persentase cakupan logika program yang berhasil dijalankan oleh test case.

---

## 1. Tingkatan Cakupan Kontrol (Coverage Levels)

1.  **Statement Coverage:** Memastikan seluruh baris instruksi/pernyataan minimal dieksekusi sekali.
2.  **Branch / Decision Coverage:** Memastikan seluruh pilihan percabangan (`if` bernilai `true` dan `if` bernilai `false`) dieksekusi.
3.  **Path Coverage:** Memastikan seluruh kombinasi jalur yang dilewati dieksekusi secara penuh.

---

## 2. Kode Contoh: Fungsi Autentikasi Admin (`verifyAdminAccess`)

```php
public function verifyAdminAccess($user) {
    if ($user->role == 'admin') {           // Branch 1
        if ($user->status == 'active') {    // Branch 2
            return "Akses Diberikan";
        }
    }
    return "Akses Ditolak";
}
```

---

## 3. Matriks Skenario Uji Cakupan Alur

### Skenario 1: Untuk Mencapai 100% Statement Coverage
Kita hanya membutuhkan 2 kasus uji untuk memastikan semua baris kode pernah dilewati:
*   **Test Case 1:** `$user->role = 'admin'`, `$user->status = 'active'`. (Mengeksekusi baris `return "Akses Diberikan"`)
*   **Test Case 2:** `$user->role = 'user'`. (Mengeksekusi baris `return "Akses Ditolak"`)

### Skenario 2: Untuk Mencapai 100% Branch / Decision Coverage
Kita harus menguji seluruh nilai alternatif pada setiap percabangan keputusan:
*   **Test Case 1:** `$user->role = 'admin'`, `$user->status = 'active'` (Branch 1 = True, Branch 2 = True)
*   **Test Case 2:** `$user->role = 'admin'`, `$user->status = 'inactive'` (Branch 1 = True, Branch 2 = False)
*   **Test Case 3:** `$user->role = 'user'` (Branch 1 = False)
