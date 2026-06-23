# Basis Path Testing - White Box Testing

Dokumen ini mendokumentasikan hasil pengujian jalur basis (*Basis Path Testing*) untuk metode `store(Request $request)` pada [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php) secara lengkap dan mendalam.

---

## 1. Source Code yang Diuji (store Method)

```php
    public function store(Request $request)
    {
        $request->validate([
            'field_id' => 'required|exists:fields,id',
            'booking_date' => 'required|date|after_or_equal:today',
            'start_time' => 'required|date_format:H:i',
            'end_time' => 'required|date_format:H:i|after:start_time',
        ]);

        $field = Field::findOrFail($request->field_id);
        
        if ($field->status !== 'active') {
            return back()->withErrors(['field_id' => 'Lapangan sedang tidak aktif atau dalam perawatan.'])->withInput();
        }

        $bookingDate = $request->booking_date;
        $startTime = $request->start_time;
        $endTime = $request->end_time;

        $start = Carbon::parse($startTime);
        $end = Carbon::parse($endTime);
        
        if ($start->minute !== 0 || $end->minute !== 0) {
            return back()->withErrors(['start_time' => 'Pemesanan harus dalam kelipatan jam genap (misal: 08:00, 09:00).'])->withInput();
        }

        if ($start->hour < 7 || $end->hour > 22) {
            return back()->withErrors(['start_time' => 'Pemesanan harus di dalam jam operasional (07:00 - 22:00).'])->withInput();
        }

        $overlap = Booking::where('field_id', $field->id)
            ->whereDate('booking_date', $bookingDate)
            ->whereIn('status', ['pending', 'approved'])
            ->where(function ($query) use ($startTime, $endTime) {
                $query->where('start_time', '<', $endTime)
                      ->where('end_time', '>', $startTime);
            })
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

        return redirect()->route('bookings.my')->with('success', 'Booking berhasil diajukan! Silakan unggah bukti pembayaran.');
    }
```

---

## 2. Perhitungan Kompleksitas Siklomatis (Cyclomatic Complexity Math Proof)

Kompleksitas Siklomatis $V(G)$ dapat dihitung menggunakan tiga metode pembuktian matematika yang saling memvalidasi:

### Metode 1: Berdasarkan Region pada Graph ($V(G) = R$)
Jika diagram alir kontrol digambarkan sebagai graf bidang datar, maka jumlah region (area tertutup + area luar terluar) bernilai **7**.

### Metode 2: Berdasarkan Node dan Edge ($V(G) = E - N + 2P$)
*   Jumlah Edges ($E$) = 19
*   Jumlah Nodes ($N$) = 14
*   Jumlah Connected Components ($P$) = 1 (Graf tunggal)
*   *Perhitungan:*
    $$V(G) = 19 - 14 + 2(1) = 7$$

### Metode 3: Berdasarkan Titik Keputusan ($V(G) = P_{dec} + 1$)
Jumlah predikat keputusan tunggal di dalam kode (mengurai ekspresi `||` / `OR` sebagai keputusan terpisah):
1.  P1: `status !== 'active'` (Line 25)
2.  P2: `start->minute !== 0` (Line 37)
3.  P3: `end->minute !== 0` (Line 37)
4.  P4: `start->hour < 7` (Line 42)
5.  P5: `end->hour > 22` (Line 42)
6.  P6: `overlap == true` (Line 56)
*   *Perhitungan:*
    $$V(G) = 6 \text{ keputusan} + 1 = 7$$

Ketiga metode membuktikan secara konsisten bahwa terdapat **7 jalur independen linier** yang harus dilewati oleh rangkaian uji.

---

## 3. Implementasi Kode Unit Test PHPUnit untuk 7 Jalur Basis

Berikut adalah kode pengujian unit (*Unit Testing*) lengkap menggunakan framework PHPUnit Laravel untuk menguji setiap jalur basis secara terisolasi:

```php
<?php

namespace Tests\Feature;

use Tests\TestCase;
use App\Models\User;
use App\Models\Field;
use App\Models\Booking;
use Illuminate\Foundation\Testing\RefreshDatabase;
use Carbon\Carbon;

class BookingControllerTest extends TestCase
{
    use RefreshDatabase;

    protected $user;
    protected $activeField;
    protected $inactiveField;

    protected function setUp(): void
    {
        parent::setUp();
        $this->user = User::factory()->create(['role' => 'user']);
        $this->activeField = Field::create([
            'name' => 'Lapangan Futsal A',
            'type' => 'Futsal',
            'price_per_hour' => 150000,
            'status' => 'active'
        ]);
        $this->inactiveField = Field::create([
            'name' => 'Lapangan Badminton 1',
            'type' => 'Badminton',
            'price_per_hour' => 50000,
            'status' => 'maintenance'
        ]);
    }

    /** Path 1: Lapangan tidak aktif */
    public function test_path_1_field_inactive()
    {
        $response = $this->actingAs($this->user)->post(route('bookings.store'), [
            'field_id' => $this->inactiveField->id,
            'booking_date' => Carbon::today()->toDateString(),
            'start_time' => '08:00',
            'end_time' => '10:00'
        ]);

        $response->assertSessionHasErrors(['field_id']);
        $this->assertDatabaseCount('bookings', 0);
    }

    /** Path 2: Menit mulai ganjil */
    public function test_path_2_start_minute_not_zero()
    {
        $response = $this->actingAs($this->user)->post(route('bookings.store'), [
            'field_id' => $this->activeField->id,
            'booking_date' => Carbon::today()->toDateString(),
            'start_time' => '08:30', // Menit 30
            'end_time' => '10:00'
        ]);

        $response->assertSessionHasErrors(['start_time']);
        $this->assertDatabaseCount('bookings', 0);
    }

    /** Path 3: Menit selesai ganjil */
    public function test_path_3_end_minute_not_zero()
    {
        $response = $this->actingAs($this->user)->post(route('bookings.store'), [
            'field_id' => $this->activeField->id,
            'booking_date' => Carbon::today()->toDateString(),
            'start_time' => '08:00',
            'end_time' => '10:15' // Menit 15
        ]);

        $response->assertSessionHasErrors(['start_time']);
        $this->assertDatabaseCount('bookings', 0);
    }

    /** Path 4: Jam mulai sebelum operasional */
    public function test_path_4_start_hour_before_seven()
    {
        $response = $this->actingAs($this->user)->post(route('bookings.store'), [
            'field_id' => $this->activeField->id,
            'booking_date' => Carbon::today()->toDateString(),
            'start_time' => '06:00', // Di bawah 07:00
            'end_time' => '08:00'
        ]);

        $response->assertSessionHasErrors(['start_time']);
        $this->assertDatabaseCount('bookings', 0);
    }

    /** Path 5: Jam selesai setelah operasional */
    public function test_path_5_end_hour_after_twenty_two()
    {
        $response = $this->actingAs($this->user)->post(route('bookings.store'), [
            'field_id' => $this->activeField->id,
            'booking_date' => Carbon::today()->toDateString(),
            'start_time' => '21:00',
            'end_time' => '23:00' // Di atas 22:00
        ]);

        $response->assertSessionHasErrors(['start_time']);
        $this->assertDatabaseCount('bookings', 0);
    }

    /** Path 6: Terjadi bentrok jadwal sewa (Overlap) */
    public function test_path_6_schedule_overlap()
    {
        // Buat booking eksis yang disetujui
        Booking::create([
            'user_id' => $this->user->id,
            'field_id' => $this->activeField->id,
            'booking_date' => Carbon::tomorrow()->toDateString(),
            'start_time' => '10:00',
            'end_time' => '12:00',
            'total_price' => 300000,
            'status' => 'approved'
        ]);

        // Request baru yang beririsan
        $response = $this->actingAs($this->user)->post(route('bookings.store'), [
            'field_id' => $this->activeField->id,
            'booking_date' => Carbon::tomorrow()->toDateString(),
            'start_time' => '09:00',
            'end_time' => '11:00' // Bentrok pada 10:00-11:00
        ]);

        $response->assertSessionHasErrors(['start_time']);
        $this->assertDatabaseCount('bookings', 1); // Hanya ada booking awal
    }

    /** Path 7: Alur sukses */
    public function test_path_7_booking_success()
    {
        $response = $this->actingAs($this->user)->post(route('bookings.store'), [
            'field_id' => $this->activeField->id,
            'booking_date' => Carbon::tomorrow()->toDateString(),
            'start_time' => '08:00',
            'end_time' => '10:00'
        ]);

        $response->assertRedirect(route('bookings.my'));
        $response->assertSessionHas('success');
        $this->assertDatabaseHas('bookings', [
            'user_id' => $this->user->id,
            'field_id' => $this->activeField->id,
            'booking_date' => Carbon::tomorrow()->toDateString(),
            'start_time' => '08:00',
            'end_time' => '10:00',
            'total_price' => 300000,
            'status' => 'pending'
        ]);
    }
}
```
