# Loop Testing (Pengujian Perulangan) - White Box Testing

Dokumen ini mendokumentasikan skenario pengujian perulangan (*Loop Testing*) tingkat White Box untuk memastikan tidak adanya kesalahan batas perulangan (*off-by-one errors*), perulangan tak terbatas (*infinite loops*), atau kesalahan pembacaan memori pada perulangan di dalam aplikasi [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan).

---

## 1. Kode yang Diuji: Generator Jadwal Operasional Lapangan

Di bawah ini adalah fungsi helper yang digunakan oleh sistem untuk mengumpulkan slot waktu sewa (jam) dalam format array dari jam mulai hingga jam selesai operasional:

```php
public function generateTimeSlots($startHour, $endHour)
{
    $slots = [];
    $current = $startHour;

    while ($current < $endHour) { // Struktur Loop: while
        $next = $current + 1;
        $slots[] = sprintf('%02d:00 - %02d:00', $current, $next);
        $current++;
    }

    return $slots;
}
```

---

## 2. Kelas Kasus Uji Perulangan Sederhana (Simple Loop Testing)

Berdasarkan jam operasional sistem (07:00 hingga 22:00), nilai maksimum iterasi yang mungkin terjadi adalah **15 iterasi** ($22 - 7 = 15$). Maka $N = 15$.

Rancangan kasus uji batas perulangan didefinisikan sebagai berikut:

1.  **Iterasi 0 (Melewati Perulangan Segera):**
    *   *Input:* `$startHour = 7`, `$endHour = 7` (atau `$startHour > $endHour`).
    *   *Hasil Diharapkan:* Loop dilewati sepenuhnya, mengembalikan array kosong `[]`.
2.  **Iterasi 1 (Satu Putaran):**
    *   *Input:* `$startHour = 7`, `$endHour = 8`.
    *   *Hasil Diharapkan:* Loop berputar sekali, mengembalikan `['07:00 - 08:00']`.
3.  **Iterasi 2 (Dua Putaran):**
    *   *Input:* `$startHour = 7`, `$endHour = 9`.
    *   *Hasil Diharapkan:* Loop berputar dua kali, mengembalikan `['07:00 - 08:00', '08:00 - 09:00']`.
4.  **Iterasi M Putaran (Di Bawah Maksimum - Nilai Tengah):**
    *   *Input:* `$startHour = 7`, `$endHour = 17` ($M = 10$ putaran).
    *   *Hasil Diharapkan:* Mengembalikan 10 slot waktu dari pukul 07:00 hingga 17:00.
5.  **Iterasi $N-1$ Putaran (Maksimum Kurang Satu):**
    *   *Input:* `$startHour = 7`, `$endHour = 21` (14 putaran).
    *   *Hasil Diharapkan:* Mengembalikan 14 slot waktu lengkap dari pukul 07:00 hingga 21:00.
6.  **Iterasi $N$ Putaran (Maksimum Penuh):**
    *   *Input:* `$startHour = 7`, `$endHour = 22` (15 putaran).
    *   *Hasil Diharapkan:* Mengembalikan 15 slot waktu lengkap hingga pukul 22:00.
7.  **Iterasi $N+1$ Putaran (Di Luar Batas Maksimum):**
    *   *Input:* `$startHour = 7`, `$endHour = 23` (16 putaran).
    *   *Hasil Diharapkan:* Sistem memotong input di tingkat validator sebelum masuk ke fungsi generator ini (mencegah loop berjalan di luar batas operasional 22:00).

---

## 3. Matriks Kasus Uji Loop Testing Detail

| ID Uji | Kondisi Pengujian | Parameter Input | Jumlah Putaran Loop | Hasil Output yang Diharapkan | Status |
| :--- | :--- | :--- | :---: | :--- | :---: |
| **TC-LP-001** | Iterasi = 0 | `start = 7`, `end = 7` | 0 | `[]` (Array kosong) | Pass |
| **TC-LP-002** | Iterasi = 1 | `start = 7`, `end = 8` | 1 | `["07:00 - 08:00"]` | Pass |
| **TC-LP-003** | Iterasi = 2 | `start = 7`, `end = 9` | 2 | `["07:00 - 08:00", "08:00 - 09:00"]` | Pass |
| **TC-LP-004** | Iterasi = $N-1$ | `start = 7`, `end = 21` | 14 | 14 Slot dari `07:00 - 08:00` s.d `20:00 - 21:00` | Pass |
| **TC-LP-005** | Iterasi = $N$ | `start = 7`, `end = 22` | 15 | 15 Slot dari `07:00 - 08:00` s.d `21:00 - 22:00` | Pass |
| **TC-LP-006** | Iterasi = $N+1$ (Invalid) | `start = 7`, `end = 23` | - | Ditolak sebelum fungsi dipanggil (Error validasi jam operasional). | Pass |
