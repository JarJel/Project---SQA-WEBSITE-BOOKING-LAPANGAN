# Control Flow Testing

Pengujian Alur Kontrol (*Control Flow Testing*) menargetkan pemenuhan kriteria cakupan cabang (*branch coverage*) dan pernyataan (*statement coverage*) pada metode `store(Request $request)` di [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php).

---

## 1. Cakupan Pernyataan (Statement Coverage)

Untuk mencapai 100% *Statement Coverage* (mengeksekusi setiap baris kode di `store`), setidaknya kita membutuhkan 5 kasus uji utama yang memicu alur keluar:
1.  **Test Case 1 (Lolos Validasi Lapangan Inaktif):** Memicu `return back()->withErrors(['field_id' => ...])` (baris 26).
2.  **Test Case 2 (Lolos Validasi Menit Ganjil):** Memicu `return back()->withErrors(['start_time' => ...])` (baris 38 - kelipatan jam genap).
3.  **Test Case 3 (Lolos Validasi Jam Operasional):** Memicu `return back()->withErrors(['start_time' => ...])` (baris 43 - luar jam operasional).
4.  **Test Case 4 (Lolos Validasi Overlap):** Memicu `return back()->withErrors(['start_time' => ...])` (baris 57 - slot dipesan).
5.  **Test Case 5 (Alur Sukses):** Mengeksekusi kalkulasi durasi/tarif, `Booking::create(...)` (baris 61-71), dan redirect sukses (baris 73).

---

## 2. Cakupan Cabang (Branch Coverage)

Untuk mencapai 100% *Branch Coverage*, seluruh percabangan logika (baik jalur True maupun False) harus dieksekusi:

*   **Branch 1 (`$field->status !== 'active'`):**
    *   *True Case:* Lapangan status 'maintenance' (TC-01).
    *   *False Case:* Lapangan status 'active' (TC-02).
*   **Branch 2 (`$start->minute !== 0 || $end->minute !== 0`):**
    *   *True Case (Start/End minute != 0):* Masukan menit ganjil (TC-03).
    *   *False Case:* Masukan menit `00` (TC-04).
*   **Branch 3 (`$start->hour < 7 || $end->hour > 22`):**
    *   *True Case:* Jam di luar operasional (TC-05).
    *   *False Case:* Jam di dalam operasional (TC-06).
*   **Branch 4 (`$overlap`):**
    *   *True Case:* Terjadi bentrok jadwal (TC-07).
    *   *False Case:* Jadwal kosong (TC-08).
