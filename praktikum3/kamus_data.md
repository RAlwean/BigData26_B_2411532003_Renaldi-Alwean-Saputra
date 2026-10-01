# Kamus Data (Data Dictionary) - Pipeline Taksi TLC

| Nama Kolom | Tipe Data | Satuan | Sumber Asal | Aturan Validitas & Penanganan Null |
| :--- | :--- | :--- | :--- | :--- |
| `tpep_pickup_datetime` | datetime64[ns] | Waktu Lokal | Parquet TLC | Rentang valid dalam 1 bulan spesifik. Null dihapus (MCAR). |
| `trip_distance` | float64 | Mil | Parquet TLC | Rentang valid 0.01 - 100 mil. Outlier atas di-winsorizing (persentil 99.5). |
| `total_amount` | float64 | USD | Parquet TLC | Harus > 0. Outlier tarif tinggi dipertahankan (fenomena bisnis). |
| `durasi_menit` | float64 | Menit | Fitur Rekayasa| Rentang valid 1 - 180 menit. Baris di luar ini dibuang. |
| `passenger_count` | float64 | Orang | Parquet TLC | Diisi dengan nilai median berdasarkan jam (`groupby(jam).transform(median)`). |
| `borough_naik` | string | - | CSV Zona TLC | Hasil join 1:1. Jika tidak ditemukan, dibiarkan null ('Unknown'). |
| `nama_pembayaran`| string | - | SQLite Operasional | Hasil join 1:1 dari basis data `tarif_referensi`. |
| `suhu_c` | float64 | Celcius | Open-Meteo API | Disesuaikan dengan `dt.floor('h')`. Dibiarkan null jika API tidak mencakup jam tersebut. |
| `hujan` | biner (0/1) | - | Open-Meteo API| > 0.1 mm = 1. Null dikonversi menjadi 0. |
| `is_libur` | biner (0/1) | - | Nager.Date API| 1 = Libur, 0 = Tidak. Null pasca-join dikonversi menjadi 0. |
