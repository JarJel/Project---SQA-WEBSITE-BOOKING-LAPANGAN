# Pattern Testing (Pengujian Pola) - Grey Box Testing

Dokumen ini mendokumentasikan skenario pengujian pola (*Pattern Testing*) pada tingkat Grey Box untuk aplikasi [Website-BookingLapangan](https://github.com/JarJel/Website-BookingLapangan). Pengujian ini berfokus pada dua aspek:
1.  **Validasi Pola Respon API (JSON Schema Testing):** Penguji memverifikasi apakah struktur data JSON yang dikembalikan oleh API memiliki skema terstandarisasi.
2.  **Validasi Pola Log Server (Log Pattern Analysis):** Penguji menganalisis log Laravel untuk memastikan kesalahan internal dan aktivitas keamanan tercatat dalam format yang teratur.

---

## 1. Validasi Pola Skema Respon JSON (JSON Schema Testing)

Setiap respon API yang dikembalikan oleh backend harus mengikuti pola skema terdefinisi berdasarkan status operasi:

### A. Pola Respon Sukses (HTTP 200 / 201)
*   **Endpoint:** `POST /api/bookings` atau `GET /api/bookings/my`
*   **Pola Skema JSON yang Divalidasi:**
    ```json
    {
      "$schema": "http://json-schema.org/draft-07/schema#",
      "type": "object",
      "properties": {
        "success": { "type": "boolean", "const": true },
        "message": { "type": "string" },
        "data": {
          "type": "object",
          "properties": {
            "id": { "type": "integer" },
            "user_id": { "type": "integer" },
            "field_id": { "type": "integer" },
            "booking_date": { "type": "string", "format": "date" },
            "start_time": { "type": "string" },
            "end_time": { "type": "string" },
            "total_price": { "type": "number" },
            "status": { "type": "string", "enum": ["pending", "approved", "rejected", "cancelled"] }
          },
          "required": ["id", "user_id", "field_id", "booking_date", "start_time", "end_time", "total_price", "status"]
        }
      },
      "required": ["success", "message", "data"]
    }
    ```

### B. Pola Respon Gagal Validasi (HTTP 422 Unprocessable Entity)
*   **Pola Skema JSON yang Divalidasi:**
    ```json
    {
      "success": false,
      "message": "The given data was invalid.",
      "errors": {
        "booking_date": [
          "The booking date field must be a date after or equal to today."
        ]
      }
    }
    ```

---

## 2. Validasi Pola Log Aktivitas Server (Log Pattern Analysis)

Penguji menganalisis file log di `storage/logs/laravel.log` menggunakan pola regular expression (regex) untuk mendeteksi anomali:

### Skenario Uji Log:
1.  **Pemicu:** Menembak endpoint API pembayaran dengan berkas kosong atau parameter rusak secara berkala.
2.  **Verifikasi Log:**
    *   *Direktori File Log:* `C:\xampp\htdocs\WebsiteBookingLapangan\storage\logs\laravel.log`
    *   *Pola Regex yang Dicari:* `/^\[\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}\] staging\.ERROR: ModelNotFoundException.*$/m`
3.  **Evaluasi Ketangguhan Log:**
    *   Format log harus diawali dengan stempel waktu UTC/lokal: `[YYYY-MM-DD HH:MM:SS]`.
    *   Tingkat keparahan kesalahan (`ERROR`/`WARNING`/`INFO`) tercatat dengan jelas.
    *   Trace error database tidak ditampilkan ke pengguna (disembunyikan di frontend) tetapi tercatat lengkap pada log file server.
