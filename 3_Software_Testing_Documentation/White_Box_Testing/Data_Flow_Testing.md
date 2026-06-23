# Data Flow Testing - White Box Testing

Dokumen ini mendokumentasikan hasil pengujian alur data (*Data Flow Testing*) untuk memvalidasi deklarasi, modifikasi, dan penggunaan variabel (*Definition-Use / DU Chain*) pada metode `store(Request $request)` di [BookingController.php](file:///c:/xampp/htdocs/WebsiteBookingLapangan/app/Http/Controllers/BookingController.php).

---

## 1. Siklus Hidup Variabel (DU-Pair Analysis)

Penguji menganalisis seluruh variabel kunci dari saat dideklarasikan (Define) hingga dibaca (Use) di dalam kode program:

### A. Variabel `$request` (Object Request)
*   **Define (d):** Baris 12 (Masuk sebagai parameter metode).
*   **Use (u):**
    *   Baris 14 (`$request->validate`) ➔ Predicate Use (p-use) untuk evaluasi parameter validasi.
    *   Baris 23 (`$request->field_id`) ➔ Computation Use (c-use) sebagai pencarian database.
    *   Baris 27 (`$request->booking_date`) ➔ c-use untuk penugasan variabel.
    *   Baris 28 (`$request->start_time`) ➔ c-use.
    *   Baris 29 (`$request->end_time`) ➔ c-use.
*   **DU-Chain:** `(12, 14)`, `(12, 23)`, `(12, 27)`, `(12, 28)`, `(12, 29)`

### B. Variabel `$field` (Object Model Field)
*   **Define (d):** Baris 23 (`$field = Field::findOrFail(...)`).
*   **Use (u):**
    *   Baris 25 (`$field->status`) ➔ p-use (evaluasi kondisi status aktif).
    *   Baris 47 (`$field->id`) ➔ c-use (parameter kueri pencarian SQL).
    *   Baris 61 (`$field->price_per_hour`) ➔ c-use (perkalian total tarif).
    *   Baris 65 (`$field->id`) ➔ c-use (penyimpanan DB).
*   **DU-Chain:** `(23, 25)`, `(23, 47)`, `(23, 61)`, `(23, 65)`

### C. Variabel `$start` & `$end` (Carbon Object)
*   **Define (d):** Baris 34 & 35 (`$start = Carbon::parse(...)`, `$end = Carbon::parse(...)`).
*   **Use (u):**
    *   Baris 37 (`$start->minute`, `$end->minute`) ➔ p-use (validasi menit genap).
    *   Baris 42 (`$start->hour`, `$end->hour`) ➔ p-use (validasi jam operasional).
    *   Baris 60 (`$end->diffInHours($start)`) ➔ c-use (perhitungan selisih jam).
*   **DU-Chain:** `(34, 37)`, `(34, 42)`, `(34, 60)`, `(35, 37)`, `(35, 42)`, `(35, 60)`

---

## 2. Pendeteksian Anomali Alur Data (Data Flow Anomalies Checking)

Penguji Grey/White Box mengevaluasi kode program untuk mencari 3 jenis anomali data:

1.  **UR Anomali (Undefined Read):** Membaca variabel yang belum didefinisikan nilainya.
    *   *Analisis Kode:* Tidak ditemukan UR Anomali. Variabel `$bookingDate`, `$startTime`, dan `$endTime` dideklarasikan secara eksplisit pada baris 27-29 sebelum diparsing oleh Carbon di baris 32-33 atau digunakan di kueri database pada baris 46-52.
2.  **DU Anomali (Defined but not Used):** Mendefinisikan variabel namun nilainya ditimpa atau program selesai tanpa membacanya sama sekali.
    *   *Analisis Kode:* Tidak ditemukan DU Anomali. Seluruh variabel yang di-define memiliki rantai use yang aktif.
3.  **DK Anomali (Defined but Killed):** Mendefinisikan variabel lalu menghapus variabel (`unset`) tanpa sempat menggunakannya.
    *   *Analisis Kode:* Bebas dari DK Anomali.

---

## 3. Matriks Kasus Uji Data Flow (DU-Path Testing)

Kasus uji dirancang untuk memastikan nilai variabel diteruskan dengan benar sepanjang rantai DU:

| ID Uji | Variabel Uji | Aliran Jalur DU yang Diuji | Nilai Input Uji | Kinerja Aliran Data | Status |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **TC-DF-01** | `$field` | Baris 23 ➔ Baris 61 | `field_id = 1` (harga: 150000) | Nilai `$field->price_per_hour` diteruskan dengan benar ke perkalian tarif. | Pass |
| **TC-DF-02** | `$start` | Baris 34 ➔ Baris 60 | `start_time = '08:00'` | Objek Carbon `$start` berhasil dihitung selisih jamnya di `diffInHours`. | Pass |
| **TC-DF-03** | `$overlap` | Baris 47 ➔ Baris 56 | Bentrok = true | Nilai boolean `$overlap` berhasil dibaca di baris 56 untuk memblokir booking. | Pass |
