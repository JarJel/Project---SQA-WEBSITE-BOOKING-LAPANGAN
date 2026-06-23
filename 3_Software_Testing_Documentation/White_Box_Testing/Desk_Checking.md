# Desk Checking (Dry Run)

Dokumen ini mendokumentasikan pelacakan manual (*Desk Checking*) logika kode metode `store` di [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php) menggunakan nilai masukan tiruan.

---

## 1. Kasus Uji Tiruan A: Input Menit Tidak Bulat
*   **Input Request:** `field_id = 1`, `booking_date = 2026-06-25`, `start_time = 08:30`, `end_time = 10:00`.
*   **Mock Database:** Lapangan ID `1` memiliki status `'active'`.
*   **Tabel Pelacakan (Trace Table):**

| Langkah | Baris Kode | Evaluasi Kondisi / Ekspresi | Nilai Variabel | Keterangan |
| :---: | :--- | :--- | :--- | :--- |
| 1 | Baris 16-21 | `$request->validate(...)` | Lolos validasi dasar | Semua input berformat benar. |
| 2 | Baris 23 | `$field = Field::findOrFail(1)` | `$field->status = 'active'` | Lapangan ditemukan di DB. |
| 3 | Baris 25 | `if ($field->status !== 'active')` | `'active' !== 'active'` (False) | Tidak masuk ke blok IF. |
| 4 | Baris 34-35 | `$start = Carbon::parse('08:30')`<br>`$end = Carbon::parse('10:00')` | `$start->minute = 30`<br>`$end->minute = 0` | Waktu berhasil diparsing. |
| 5 | Baris 37 | `if ($start->minute !== 0 \|\| ...)` | `30 !== 0` (True) | Kondisi IF bernilai True. |
| 6 | Baris 38 | `return back()->withErrors(...)` | Mengembalikan error menit | Aliran dihentikan di sini (Exit 2). |

---

## 2. Kasus Uji Tiruan B: Pemesanan Berhasil (Sukses)
*   **Input Request:** `field_id = 1`, `booking_date = 2026-06-25`, `start_time = 08:00`, `end_time = 10:00`.
*   **Mock Database:** Lapangan ID `1` memiliki status `'active'`, tidak ada data overlap di tabel `bookings`.
*   **Tabel Pelacakan (Trace Table):**

| Langkah | Baris Kode | Evaluasi Kondisi / Ekspresi | Nilai Variabel | Keterangan |
| :---: | :--- | :--- | :--- | :--- |
| 1 | Baris 23 | `$field = Field::findOrFail(1)` | `$field->price_per_hour = 150000` | Lapangan ditemukan. |
| 2 | Baris 25 | `if ($field->status !== 'active')` | False | Status aktif. |
| 3 | Baris 34-35 | `$start = Carbon::parse(...)` | `$start->hour = 8, min = 0`<br>`$end->hour = 10, min = 0` | Berhasil diparsing. |
| 4 | Baris 37 | `if ($start->minute !== 0 \|\| ...)` | `0 !== 0 \|\| 0 !== 0` (False) | Menit valid (`00`). |
| 5 | Baris 42 | `if ($start->hour < 7 \|\| ...)` | `8 < 7 \|\| 10 > 22` (False) | Jam di dalam operasional. |
| 6 | Baris 47-54 | `$overlap = Booking::where(...)` | `$overlap = false` | Tidak ada bentrok di DB. |
| 7 | Baris 56 | `if ($overlap)` | False | Lolos validasi overlap. |
| 8 | Baris 60 | `$duration = $end->diffInHours($start)`| `$duration = 2` | Selisih 2 jam. |
| 9 | Baris 61 | `$totalPrice = 2 * 150000` | `$totalPrice = 300000` | Total harga dihitung. |
| 10 | Baris 63-71 | `Booking::create(...)` | Record baru tersimpan | Status pending, total_price 300000. |
| 11 | Baris 73 | `return redirect()->route(...)` | Redirect sukses | Aliran keluar sukses (Exit 7). |
