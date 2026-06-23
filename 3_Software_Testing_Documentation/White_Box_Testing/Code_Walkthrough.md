# Code Walkthrough Report - BookingController

Dokumen ini mendokumentasikan hasil peninjauan kode program secara tim (*Code Walkthrough*) untuk berkas controller utama **`app/Http/Controllers/BookingController.php`** pada metode `store`.

---

## 1. Rincian Sesi Walkthrough

*   **Tanggal / Waktu:** 24 Juni 2026, pukul 10:00 - 11:30 WIB
*   **Tempat / Media:** Discord Dev Channel (Virtual Session)
*   **Peran Peserta:**
    *   *Presenter:* Lead Developer (Penulis Kode Utama)
    *   *Moderator:* Project Manager (PM)
    *   *Scribe (Pencatat):* QA Engineer
    *   *Reviewer:* Senior backend engineer

---

## 2. Temuan dan Analisis Masalah Struktur Kode

Selama penelusuran baris demi baris, tim mengidentifikasi beberapa masalah utama pada logika `store` awal:

### Temuan 1: Percabangan Bersarang yang Berlebihan (Deep Nesting Conditions)
*   *Deskripsi:* Validasi status lapangan, validasi menit, jam operasional, dan overlap jadwal diatur dalam struktur `if` bersarang berturut-turut. Hal ini meningkatkan kompleksitas kognitif dan mempersulit penulisan unit test.
*   *Rekomendasi:* Gunakan pola **Guard Clauses** (Early Return) untuk langsung menolak request di awal fungsi secara rapi tanpa bersarang.

### Temuan 2: Kerentanan Kondisi Balapan (Race Conditions)
*   *Deskripsi:* Kueri pengecekan overlap jadwal (`$overlap = Booking::where(...)->exists()`) dijalankan beberapa milidetik sebelum kueri pembuatan data sewa (`Booking::create(...)`). Di bawah beban tinggi (misal: 2 user memesan lapangan & jam yang sama pada detik yang sama), terdapat celah di mana kedua kueri overlap menghasilkan `false` dan database menyimpan dua pesanan bentrok.
*   *Rekomendasi:* Terapkan kueri transaksi dengan penguncian baris basis data (**Database Transaction with Pessimistic Locking** / `sharedLock()` atau `lockForUpdate()`) guna mencegah kondisi balapan.

### Temuan 3: Ketiadaan Penanganan Eksepsi (Try-Catch Block)
*   *Deskripsi:* `Field::findOrFail(...)` akan langsung melempar `ModelNotFoundException` jika data kosong. Jika itu terjadi pada request API, framework akan mengembalikan halaman HTML debug (pada mode lokal) atau halaman error 500 mentah ke pengguna.
*   *Rekomendasi:* Bungkus logika query dalam blok `try-catch` atau biarkan ditangani oleh exception handler global secara terstandarisasi.

---

## 3. Proposal Kode Hasil Refaktorisasi (Refactored Code Proposal)

Berdasarkan rekomendasi walkthrough, berikut draf kode perbaikan yang diusulkan untuk diterapkan:

```php
    public function store(Request $request)
    {
        $request->validate([
            'field_id' => 'required|exists:fields,id',
            'booking_date' => 'required|date|after_or_equal:today',
            'start_time' => 'required|date_format:H:i',
            'end_time' => 'required|date_format:H:i|after:start_time',
        ]);

        $bookingDate = $request->booking_date;
        $startTime = $request->start_time;
        $endTime = $request->end_time;

        $start = Carbon::parse($startTime);
        $end = Carbon::parse($endTime);

        // Guard Clause 1: Validasi kelipatan menit genap (:00)
        if ($start->minute !== 0 || $end->minute !== 0) {
            return back()->withErrors(['start_time' => 'Pemesanan harus dalam kelipatan jam genap.'])->withInput();
        }

        // Guard Clause 2: Validasi batasan jam operasional
        if ($start->hour < 7 || $end->hour > 22) {
            return back()->withErrors(['start_time' => 'Pemesanan harus di dalam jam operasional (07:00 - 22:00).'])->withInput();
        }

        // Mulai transaksi database untuk mencegah Race Condition
        return DB::transaction(function () use ($request, $start, $end, $bookingDate, $startTime, $endTime) {
            // Lock database untuk baris lapangan terkait
            $field = Field::where('id', $request->field_id)->lockForUpdate()->firstOrFail();
            
            // Guard Clause 3: Cek status operasional lapangan
            if ($field->status !== 'active') {
                return back()->withErrors(['field_id' => 'Lapangan sedang tidak aktif atau dalam perawatan.'])->withInput();
            }

            // Guard Clause 4: Cek overlap jadwal menggunakan database lock
            $overlap = Booking::where('field_id', $field->id)
                ->whereDate('booking_date', $bookingDate)
                ->whereIn('status', ['pending', 'approved'])
                ->where(function ($query) use ($startTime, $endTime) {
                    $query->where('start_time', '<', $endTime)
                          ->where('end_time', '>', $startTime);
                })
                ->lockForUpdate() // Pessimistic lock
                ->exists();

            if ($overlap) {
                return back()->withErrors(['start_time' => 'Slot waktu yang Anda pilih sudah dipesan oleh pengguna lain.'])->withInput();
            }

            $duration = $end->diffInHours($start);
            $totalPrice = $duration * $field->price_per_hour;

            Booking::create([
                'user_id' => Auth::id(),
                'field_id' => $field->id,
                'booking_date' => $bookingDate,
                'start_time' => $startTime,
                'end_time' => $endTime,
                'total_price' => $totalPrice,
                'status' => 'pending',
            ]);

            return redirect()->route('bookings.my')->with('success', 'Booking berhasil diajukan!');
        });
    }
```
*Hasil revisi di atas meningkatkan keamanan concurrency (race-conditions) dan keterbacaan kode (clean code).*
