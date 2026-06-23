# Basis Path Testing

Pengujian Jalur Basis (*Basis Path Testing*) digunakan untuk menentukan jumlah jalur independen secara linier melalui kode program metode `store(Request $request)` di [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php) dan merancang kasus uji untuk mencakup setiap jalur tersebut.

---

## 1. Flow Graph dan Node Alur Kontrol (Control Flow Nodes)

Berikut adalah pemetaan alur kontrol dari metode `store`:

*   **Node 1:** Validasi masukan request (`$request->validate(...)`).
*   **Node 2:** Query data lapangan (`$field = Field::findOrFail(...)`).
*   **Node 3 (Keputusan 1):** Apakah status lapangan tidak aktif? `if ($field->status !== 'active')`
*   **Node 4 (Exit 1):** Return back dengan error status lapangan tidak aktif.
*   **Node 5:** Inisialisasi variabel tanggal, parsing Carbon waktu mulai dan waktu selesai.
*   **Node 6 (Keputusan 2):** Apakah menit waktu mulai $\neq 0$ OR menit waktu selesai $\neq 0$? `if ($start->minute !== 0 || $end->minute !== 0)`
*   **Node 7 (Exit 2):** Return back dengan error kelipatan jam genap.
*   **Node 8 (Keputusan 3):** Apakah jam mulai $< 7$ OR jam selesai $> 22$? `if ($start->hour < 7 || $end->hour > 22)`
*   **Node 9 (Exit 3):** Return back dengan error luar jam operasional.
*   **Node 10:** Query pengecekan jadwal tumpang tindih ke database (`$overlap = Booking::where(...)`).
*   **Node 11 (Keputusan 4):** Apakah jadwal bentrok (overlap)? `if ($overlap)`
*   **Node 12 (Exit 4):** Return back dengan error slot waktu telah dipesan.
*   **Node 13:** Hitung durasi jam sewa, hitung total harga sewa, dan eksekusi `Booking::create(...)`.
*   **Node 14 (Exit Sukses):** Pengalihan sukses ke daftar booking pengguna (`return redirect()`).

---

## 2. Perhitungan Kompleksitas Siklomatis (Cyclomatic Complexity)

Menggunakan rumus logika short-circuit OR (`||`) yang dievaluasi secara independen oleh kompilator PHP, kita membagi kondisi majemuk menjadi titik keputusan individual:
1.  P1: `status !== 'active'`
2.  P2: `start->minute !== 0`
3.  P3: `end->minute !== 0`
4.  P4: `start->hour < 7`
5.  P5: `end->hour > 22`
6.  P6: `overlap == true`

Maka jumlah titik keputusan $P = 6$.  
Kompleksitas Siklomatis $V(G) = P + 1 = 6 + 1 = 7$.  
Ini menunjukkan terdapat **7 jalur independen secara linier** yang harus diuji.

---

## 3. Jalur Independen Teridentifikasi (Linearly Independent Paths)

*   **Path 1 (Error Lapangan Inaktif):** Node 1 $\to$ 2 $\to$ 3 (True) $\to$ 4 (Exit 1)
    *   *Input:* `field_id` lapangan inaktif (`status = 'maintenance'`).
    *   *Output Diharapkan:* Redirect back dengan error "Lapangan sedang tidak aktif".
*   **Path 2 (Error Menit Mulai Ganjil):** Node 1 $\to$ 2 $\to$ 3 (False) $\to$ 5 $\to$ 6 (True pada `start->minute !== 0`) $\to$ 7 (Exit 2)
    *   *Input:* `start_time = '08:15'`, `end_time = '10:00'`.
    *   *Output Diharapkan:* Redirect back dengan error "Pemesanan harus dalam kelipatan jam genap".
*   **Path 3 (Error Menit Selesai Ganjil):** Node 1 $\to$ 2 $\to$ 3 (False) $\to$ 5 $\to$ 6 (True pada `end->minute !== 0`) $\to$ 7 (Exit 2)
    *   *Input:* `start_time = '08:00'`, `end_time = '10:15'`.
    *   *Output Diharapkan:* Redirect back dengan error "Pemesanan harus dalam kelipatan jam genap".
*   **Path 4 (Error Jam Mulai Operasional):** Node 1 $\to$ 2 $\to$ 3 (False) $\to$ 5 $\to$ 6 (False) $\to$ 8 (True pada `start->hour < 7`) $\to$ 9 (Exit 3)
    *   *Input:* `start_time = '06:00'`, `end_time = '08:00'`.
    *   *Output Diharapkan:* Redirect back dengan error "Pemesanan harus di dalam jam operasional".
*   **Path 5 (Error Jam Selesai Operasional):** Node 1 $\to$ 2 $\to$ 3 (False) $\to$ 5 $\to$ 6 (False) $\to$ 8 (True pada `end->hour > 22`) $\to$ 9 (Exit 3)
    *   *Input:* `start_time = '21:00'`, `end_time = '23:00'`.
    *   *Output Diharapkan:* Redirect back dengan error "Pemesanan harus di dalam jam operasional".
*   **Path 6 (Error Jadwal Bentrok/Overlap):** Node 1 $\to$ 2 $\to$ 3 (False) $\to$ 5 $\to$ 6 (False) $\to$ 8 (False) $\to$ 10 $\to$ 11 (True) $\to$ 12 (Exit 4)
    *   *Input:* Lapangan dan tanggal yang sama dengan pemesanan lain, jam `10:00 - 12:00` (bentrok).
    *   *Output Diharapkan:* Redirect back dengan error "Slot waktu sudah dipesan".
*   **Path 7 (Alur Sukses):** Node 1 $\to$ 2 $\to$ 3 (False) $\to$ 5 $\to$ 6 (False) $\to$ 8 (False) $\to$ 10 $\to$ 11 (False) $\to$ 13 $\to$ 14 (Exit Sukses)
    *   *Input:* Lapangan aktif, jam valid, tidak bentrok (misal: `08:00 - 10:00`).
    *   *Output Diharapkan:* Record tersimpan di DB, redirect ke `bookings.my` dengan pesan sukses.
