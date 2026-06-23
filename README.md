# SQA Documentation Project

Selamat datang di repositori dokumentasi penjaminan mutu perangkat lunak (Software Quality Assurance). Repositori ini berisi dokumen-dokumen standar untuk siklus hidup pengembangan perangkat lunak (SDLC) yang terstruktur dengan rapi.

## Struktur Dokumen

Berikut adalah struktur folder dokumentasi yang telah dibuat:

```text
c:\SQA-PROJECT\
├── README.md                                  # Halaman utama navigasi dokumentasi
├── 1_Software_Requirement_Specification\      # Spesifikasi Kebutuhan Perangkat Lunak (SRS)
│   └── SRS.md
├── 2_Software_Design_Documentation\           # Dokumentasi Desain Perangkat Lunak (SDD)
│   └── SDD.md
├── 3_Software_Testing_Documentation\          # Dokumentasi Pengujian Perangkat Lunak
│   ├── Black_Box_Testing\                     # Pengujian fungsional luar
│   │   ├── README.md
│   │   └── Test_Cases.md
│   ├── White_Box_Testing\                     # Pengujian unit & alur logika internal
│   │   ├── README.md
│   │   └── White_Box_Test_Suite.md
│   ├── Grey_Box_Testing\                      # Pengujian integrasi API & database
│   │   ├── README.md
│   │   └── Grey_Box_Test_Cases.md
│   └── Test_Execution_Report.md               # Laporan hasil eksekusi pengujian
├── 4_Test_Plan\                               # Rencana Pengujian (IEEE 829)
│   └── Test_Plan.md
└── 5_Software_User_Documentation\             # Dokumentasi Pengguna (User Guide/Manual)
    └── User_Guide.md
```

## Deskripsi Dokumen

### 1. [Software Requirement Specification (SRS)](file:///c:/SQA-PROJECT/1_Software_Requirement_Specification/SRS.md)
Dokumen ini mendefinisikan kebutuhan fungsional dan non-fungsional dari sistem yang akan dibangun. Berfungsi sebagai acuan utama bagi pengembang dan pemangku kepentingan.

### 2. [Software Design Documentation (SDD)](file:///c:/SQA-PROJECT/2_Software_Design_Documentation/SDD.md)
Dokumen ini menjelaskan arsitektur perangkat lunak, desain database, desain antarmuka, serta detail teknis modul-modul sistem berdasarkan kebutuhan pada SRS.

### 3. [Software Testing Documentation](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Test_Execution_Report.md)
Berisi dokumen pelengkap pengujian yang terbagi menjadi:
*   [Black Box Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Black_Box_Testing/README.md) - Pengujian fungsionalitas luar tanpa melihat kode.
*   [White Box Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/White_Box_Testing/README.md) - Pengujian unit program dan cakupan jalur logika internal.
*   [Grey Box Testing](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Grey_Box_Testing/README.md) - Pengujian integrasi fungsional, web service/API, dan integritas database.
*   [Test Execution Report](file:///c:/SQA-PROJECT/3_Software_Testing_Documentation/Test_Execution_Report.md) - Laporan akhir hasil eksekusi pengujian.

### 4. [Test Plan](file:///c:/SQA-PROJECT/4_Test_Plan/Test_Plan.md)
Dokumen rencana pengujian komprehensif (mengacu pada standar IEEE 829) yang mendefinisikan ruang lingkup, pendekatan, sumber daya, jadwal pengujian, dan manajemen risiko pengujian.

### 5. [Software User Documentation](file:///c:/SQA-PROJECT/5_Software_User_Documentation/User_Guide.md)
Dokumen panduan pengguna (User Guide / Manual Book) yang memandu pengguna akhir atau admin tentang cara mengoperasikan sistem.

---
*Dibuat otomatis untuk membantu proses penjaminan mutu perangkat lunak Anda.*
