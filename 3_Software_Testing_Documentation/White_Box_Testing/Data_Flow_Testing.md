# Data Flow Testing

Pengujian Alur Data (*Data Flow Testing*) mengidentifikasi hubungan deklarasi (*Definition - d*) dan penggunaan (*Use - u*) variabel penting pada metode `store(Request $request)` di [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php).

---

## 1. Tabel Def-Use (DU) Chain Variabel Utama

| Nama Variabel | Baris Deklarasi (Definition - d) | Baris Penggunaan (Use - u) | Jenis Penggunaan | Keterangan |
| :--- | :---: | :---: | :---: | :--- |
| **`$field`** | 23 | 25 | p-use | Memeriksa `$field->status !== 'active'` |
| | 23 | 47 | c-use | Digunakan sebagai parameter `where('field_id', $field->id)` |
| | 23 | 61 | c-use | Mengambil `$field->price_per_hour` untuk hitung tarif |
| | 23 | 65 | c-use | Menyimpan `$field->id` ke record booking baru |
| **`$start`** | 34 | 37 | p-use | Memeriksa `$start->minute !== 0` |
| | 34 | 42 | p-use | Memeriksa `$start->hour < 7` |
| | 34 | 60 | c-use | Menghitung durasi `$end->diffInHours($start)` |
| **`$overlap`** | 47 | 56 | p-use | Memeriksa `if ($overlap)` |
| **`$duration`** | 60 | 61 | c-use | Menghitung `$totalPrice = $duration * ...` |
| **`$totalPrice`**| 61 | 67 | c-use | Menyimpan total harga ke database |

---

## 2. Analisis DU-Path & Uji Integritas Data

Penguji menguji aliran data dari titik deklarasi hingga penggunaan akhir:
*   **DU-Path (`$field`, 23 ➔ 61):** Memastikan tarif per jam yang dibaca dari tabel `fields` pada baris 23 berhasil dikalikan dengan durasi pada baris 61 tanpa ada modifikasi nilai di tengah jalan.
*   **DU-Path (`$totalPrice`, 61 ➔ 67):** Memastikan hasil perkalian tarif yang disimpan di `$totalPrice` benar-benar masuk ke database di kolom `total_price` saat pembuatan data booking.
