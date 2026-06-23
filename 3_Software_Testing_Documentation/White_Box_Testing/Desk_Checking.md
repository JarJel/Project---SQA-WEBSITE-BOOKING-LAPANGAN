# Desk Checking (Dry Run) - White Box Testing

Dokumen ini mendokumentasikan hasil simulasi manual penelusuran logika program (*Desk Checking / Trace Table*) pada metode `store` di [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php) untuk empat skenario kasus uji yang berbeda.

---

## 1. Kasus Uji A: Input Menit Tidak Bulat (:00)
*   **Input Request:** `field_id = 1`, `booking_date = 2026-06-25`, `start_time = 08:30`, `end_time = 10:00`.
*   **Database Mock:** Lapangan ID `1` status `'active'`.
*   **Trace Table:**

| Langkah | Baris Kode | Evaluasi Ekspresi / Kondisi | Nilai Variabel | Catatan |
| :---: | :--- | :--- | :--- | :--- |
| 1 | Baris 14 | `$request->validate(...)` | Lolos | Format data valid. |
| 2 | Baris 23 | `$field = Field::findOrFail(1)` | `$field->status = 'active'` | Lapangan aktif ditemukan. |
| 3 | Baris 25 | `if ($field->status !== 'active')` | `'active' !== 'active'` (False) | Dilewati (Lolos). |
| 4 | Baris 34 | `$start = Carbon::parse('08:30')` | `$start->minute = 30` | Parsed. |
| 5 | Baris 35 | `$end = Carbon::parse('10:00')` | `$end->minute = 0` | Parsed. |
| 6 | Baris 37 | `if ($start->minute !== 0 \|\| ...)` | `30 !== 0` (True) | Kondisi IF = **True**. |
| 7 | Baris 38 | `return back()->withErrors(...)` | Mengembalikan error menit | Alur keluar di Node 7 (Exit 2). |

---

## 2. Kasus Uji B: Pemesanan Berhasil (Sukses)
*   **Input Request:** `field_id = 1`, `booking_date = 2026-06-25`, `start_time = 08:00`, `end_time = 10:00`.
*   **Database Mock:** Lapangan ID `1` status `'active'`, tidak ada data overlap di basis data.
*   **Trace Table:**

| Langkah | Baris Kode | Evaluasi Ekspresi / Kondisi | Nilai Variabel | Catatan |
| :---: | :--- | :--- | :--- | :--- |
| 1 | Baris 23 | `$field = Field::findOrFail(1)` | `$field->price_per_hour = 150000`| Lapangan aktif. |
| 2 | Baris 34-35 | `$start = Carbon::parse(...)` | `$start->hour=8`, `$end->hour=10`| Menit `00` untuk keduanya. |
| 3 | Baris 37 | `if ($start->minute !== 0 \|\| ...)` | `0 !== 0 \|\| 0 !== 0` (False) | Lolos validasi menit. |
| 4 | Baris 42 | `if ($start->hour < 7 \|\| ...)` | `8 < 7 \|\| 10 > 22` (False) | Lolos jam operasional. |
| 5 | Baris 47 | `$overlap = Booking::where(...)...` | `$overlap = false` | Tidak ada bentrok. |
| 6 | Baris 56 | `if ($overlap)` | False | Lolos validasi overlap. |
| 7 | Baris 60 | `$duration = $end->diffInHours($start)`| `$duration = 2` | Selisih 2 jam. |
| 8 | Baris 61 | `$totalPrice = 2 * 150000` | `$totalPrice = 300000` | Hitung total harga. |
| 9 | Baris 63 | `Booking::create(...)` | Record tersimpan | Transaksi berhasil masuk DB. |
| 10 | Baris 73 | `return redirect()->route(...)` | Redirect sukses | Sukses keluar di Node 14. |

---

## 3. Kasus Uji C: Terjadi Bentrok Jadwal (Overlap Conflict)
*   **Input Request:** `field_id = 1`, `booking_date = 2026-06-25`, `start_time = 09:00`, `end_time = 11:00`.
*   **Database Mock:** Lapangan ID `1` status `'active'`. Terdapat booking terdaftar jam `10:00 - 12:00` status `approved`.
*   **Trace Table:**

| Langkah | Baris Kode | Evaluasi Ekspresi / Kondisi | Nilai Variabel | Catatan |
| :---: | :--- | :--- | :--- | :--- |
| 1 | Baris 23 | `$field = Field::findOrFail(1)` | `$field->status = 'active'` | Lapangan ditemukan. |
| 2 | Baris 37-42 | Pengecekan menit & jam operasional | Lolos (False) | Masukan jam valid. |
| 3 | Baris 47 | `$overlap = Booking::where(...)` | Kueri SQL mengembalikan `true` | Terjadi irisan waktu (`09:00 < 12:00` AND `11:00 > 10:00`). |
| 4 | Baris 56 | `if ($overlap)` | True | Kondisi overlap terpenuhi. |
| 5 | Baris 57 | `return back()->withErrors(...)` | Mengembalikan error bentrok | Aliran dihentikan di Node 12 (Exit 4). |

---

## 4. Kasus Uji D: Pelanggaran Batas Jam Operasional Atas
*   **Input Request:** `field_id = 1`, `booking_date = 2026-06-25`, `start_time = 21:00`, `end_time = 23:00`.
*   **Database Mock:** Lapangan ID `1` status `'active'`.
*   **Trace Table:**

| Langkah | Baris Kode | Evaluasi Ekspresi / Kondisi | Nilai Variabel | Catatan |
| :---: | :--- | :--- | :--- | :--- |
| 1 | Baris 34-35 | `$start = Carbon::parse(...)` | `$start->hour=21`, `$end->hour=23`| Menit genap `:00`. |
| 2 | Baris 37 | `if ($start->minute !== 0 \|\| ...)` | False | Lolos validasi menit. |
| 3 | Baris 42 | `if ($start->hour < 7 \|\| $end->hour > 22)`| `21 < 7` (False) `\|\|` `23 > 22` (True) | Hasil ekspresi adalah **True**. |
| 4 | Baris 43 | `return back()->withErrors(...)` | Mengembalikan error operasional| Aliran dihentikan di Node 9 (Exit 3). |
