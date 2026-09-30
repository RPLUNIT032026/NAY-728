# Perhitungan Function Point — Digital Twin Sistem Parkir

**Metode:** IFPUG (bobot standar) | **Cakupan:** Sprint 1 / Product Release 1  
**Sumber angka:** `03_Function_Point.xlsx` (semua nilai di bawah sama dengan hasil di workbook)

> DET, FTR/RET, pembagian Sprint, dan nilai GSC adalah **estimasi awal** dari rancangan tabel dan fitur. Sesuaikan dengan ketentuan dosen jika ada perbedaan metode.

## 1. Ringkasan Hasil

| Jenis Fungsi | Low | Average | High | Jumlah Fungsi | UFP Seluruh Produk | UFP Sprint 1 |
|---|---:|---:|---:|---:|---:|---:|
| External Input (EI) | 8 | 3 | 0 | 11 | 36 | 9 |
| External Output (EO) | 0 | 2 | 0 | 2 | 10 | 5 |
| External Inquiry (EQ) | 3 | 3 | 0 | 6 | 21 | 7 |
| Internal Logical File (ILF) | 5 | 0 | 0 | 5 | 35 | 35 |
| External Interface File (EIF) | 0 | 0 | 0 | 0 | 0 | 0 |
| **TOTAL UFP** | **16** | **8** | **0** | **24** | **102** | **56** |

| Perhitungan | Hasil |
|---|---:|
| Total skor 14 GSC | 27 |
| VAF = 0,65 + (0,01 × 27) | 0,92 |
| **AFP Seluruh Produk** = 102 × 0,92 | **93,84 FP** |
| **AFP Sprint 1** = 56 × 0,92 | **51,52 FP** |

## 2. Penjelasan Istilah

| Istilah | Kepanjangan | Pengertian | Contoh di project ini |
|---|---|---|---|
| **EI** | External Input | Data atau perintah yang masuk dari luar sistem, lalu memperbarui data yang disimpan atau mengubah perilaku sistem. | Input data slot parkir baru; login; catat kendaraan masuk; sensor mengirim status slot. |
| **EO** | External Output | Informasi yang keluar dari sistem dan mengandung perhitungan, data turunan, atau pengolahan (bukan sekadar menampilkan data mentah). | Dashboard (jumlah terisi/kosong, persentase okupansi); laporan parkir per periode. |
| **EQ** | External Inquiry | Permintaan informasi: ada input pencarian dan output hasil, tanpa perhitungan dan tanpa mengubah data. | Lihat denah slot; cari kendaraan berdasarkan plat; lihat riwayat parkir. |
| **ILF** | Internal Logical File | Kelompok data yang dipelihara oleh sistem sendiri (disimpan di database sistem). | Tabel Users, Kendaraan, Slot Parkir, Transaksi Parkir, Log Status Slot. |
| **EIF** | External Interface File | Kelompok data yang dipelihara sistem lain, hanya dibaca oleh sistem ini. | Belum ada di Sprint 1 (nilai 0). Contoh nanti: API cuaca atau sistem akademik kampus. |
| **DET** | Data Element Type | Satu bidang data unik yang dikenali pengguna dan melewati batas sistem (field, tombol aksi). | Form tambah slot: nomor_slot, jenis, pos_x, pos_y, status, tombol simpan = 6 DET. |
| **FTR** | File Type Referenced | Jumlah file (ILF/EIF) yang dibaca atau diubah oleh sebuah transaksi (EI/EO/EQ). | Catat kendaraan masuk menyentuh Transaksi, Slot, Log, Kendaraan = 4 FTR. |
| **RET** | Record Element Type | Subgrup data dalam sebuah file (ILF/EIF). File sederhana tanpa sub-tabel bernilai 1 RET. | Tiap tabel di ERD awal bernilai 1 RET. |
| **UFP** | Unadjusted Function Point | Jumlah bobot seluruh fungsi sebelum faktor penyesuaian. | Total bobot EI + EO + EQ + ILF + EIF. |
| **GSC / VAF** | General System Characteristics / Value Adjustment Factor | 14 karakteristik teknis sistem (nilai 0-5). VAF = 0,65 + 0,01 × total skor. | Web app, input online tinggi, satu lokasi, dll. |
| **AFP** | Adjusted Function Point | Ukuran akhir = UFP × VAF. | Dipakai sebagai ukuran besar aplikasi (dan dasar estimasi usaha). |

## 3. Bobot dan Matriks Kompleksitas (IFPUG)

**Bobot per jenis fungsi**

| Tipe | Low | Average | High |
|---|---:|---:|---:|
| EI | 3 | 4 | 6 |
| EO | 4 | 5 | 7 |
| EQ | 3 | 4 | 6 |
| ILF | 7 | 10 | 15 |
| EIF | 5 | 7 | 10 |

**EI (External Input)**

| FTR \ DET | 1-4 | 5-15 | 16+ |
|---|---|---|---|
| 0-1 | Low | Low | Average |
| 2 | Low | Average | High |
| 3+ | Average | High | High |

**EO dan EQ (External Output / Inquiry)** — EQ memakai matriks yang sama dengan EO

| FTR \ DET | 1-5 | 6-19 | 20+ |
|---|---|---|---|
| 0-1 | Low | Low | Average |
| 2-3 | Low | Average | High |
| 4+ | Average | High | High |

**ILF dan EIF (File)** — foreign key dihitung sebagai DET

| RET \ DET | 1-19 | 20-50 | 51+ |
|---|---|---|---|
| 1 | Low | Low | Average |
| 2-5 | Low | Average | High |
| 6+ | Average | High | High |

## 4. Daftar Fungsi dan Perhitungan

### 4.1 External Input (EI) — 11 fungsi, 36 UFP

| ID | Nama Fungsi | DET | FTR | Kompleksitas | Bobot | Sprint |
|---|---|---:|---:|---|---:|---:|
| EI01 | Login admin | 3 | 1 | Low | 3 | 1 |
| EI02 | Tambah data kendaraan | 4 | 2 | Low | 3 | 2 |
| EI03 | Ubah data kendaraan | 5 | 2 | Average | 4 | 2 |
| EI04 | Hapus data kendaraan | 2 | 2 | Low | 3 | 2 |
| EI05 | Tambah slot parkir | 6 | 1 | Low | 3 | 2 |
| EI06 | Ubah slot parkir | 6 | 1 | Low | 3 | 2 |
| EI07 | Hapus slot parkir | 2 | 2 | Low | 3 | 2 |
| EI08 | Ubah status slot (manual oleh admin) | 3 | 2 | Low | 3 | 1 |
| EI09 | Terima data status slot dari sensor / simulator (API) | 4 | 2 | Low | 3 | 1 |
| EI10 | Catat kendaraan masuk | 4 | 4 | Average | 4 | 2 |
| EI11 | Catat kendaraan keluar | 3 | 3 | Average | 4 | 2 |

#### EI01 — Login admin

- **Penjelasan:** Admin memasukkan email dan password untuk masuk ke sistem. Sistem mencocokkannya dengan data pengguna, lalu membuka akses ke dashboard.
- **Elemen data (DET):** email, password, tombol login
- **File terlibat:** Users
- **Alasan klasifikasi:** EI: data (email, password) masuk dari luar sistem dan mengubah perilaku sistem (membuka sesi akses).
- **Dasar kompleksitas:** DET = 3; FTR = 1 → Low (3 FP)
- **Sprint:** 1

#### EI02 — Tambah data kendaraan

- **Penjelasan:** Admin menginput data kendaraan baru (plat nomor, jenis, pemilik) untuk disimpan ke database.
- **Elemen data (DET):** plat_nomor, jenis_kendaraan, pemilik (id_user), tombol simpan
- **File terlibat:** Kendaraan, Users
- **Alasan klasifikasi:** EI: data masuk dari luar sistem lalu disimpan sebagai record baru di ILF Kendaraan; ILF Users dirujuk untuk memilih pemilik.
- **Dasar kompleksitas:** DET = 4; FTR = 2 → Low (3 FP)
- **Sprint:** 2

#### EI03 — Ubah data kendaraan

- **Penjelasan:** Admin memperbarui data kendaraan yang sudah ada, misalnya koreksi plat nomor atau ganti pemilik.
- **Elemen data (DET):** id_kendaraan, plat_nomor, jenis_kendaraan, pemilik, tombol simpan
- **File terlibat:** Kendaraan, Users
- **Alasan klasifikasi:** EI: data masuk dari luar sistem lalu mengubah record yang sudah ada di ILF Kendaraan.
- **Dasar kompleksitas:** DET = 5; FTR = 2 → Average (4 FP)
- **Sprint:** 2

#### EI04 — Hapus data kendaraan

- **Penjelasan:** Admin menghapus data kendaraan. Sistem lebih dulu memeriksa apakah kendaraan masih punya transaksi parkir.
- **Elemen data (DET):** id_kendaraan, tombol hapus
- **File terlibat:** Kendaraan, Transaksi
- **Alasan klasifikasi:** EI: data masuk dari luar sistem lalu menghapus record di ILF Kendaraan; ILF Transaksi dibaca untuk validasi relasi.
- **Dasar kompleksitas:** DET = 2; FTR = 2 → Low (3 FP)
- **Sprint:** 2
- **Catatan:** Transaksi dibaca untuk cek relasi sebelum hapus

#### EI05 — Tambah slot parkir

- **Penjelasan:** Input data slot parkir baru, misalnya slot P13 dengan jenis mobil, posisi pada denah, dan status awal kosong.
- **Elemen data (DET):** nomor_slot, jenis, pos_x, pos_y, status awal, tombol simpan
- **File terlibat:** Slot Parkir
- **Alasan klasifikasi:** EI: data masuk dari luar sistem lalu disimpan sebagai record baru di ILF Slot Parkir.
- **Dasar kompleksitas:** DET = 6; FTR = 1 → Low (3 FP)
- **Sprint:** 2
- **Catatan:** Sprint 1: slot diisi lewat database.sql (seed)

#### EI06 — Ubah slot parkir

- **Penjelasan:** Admin mengubah data slot yang sudah ada, seperti nomor slot, jenis (mobil/motor), atau posisinya pada denah.
- **Elemen data (DET):** id_slot, nomor_slot, jenis, pos_x, pos_y, tombol simpan
- **File terlibat:** Slot Parkir
- **Alasan klasifikasi:** EI: data masuk dari luar sistem lalu mengubah record di ILF Slot Parkir.
- **Dasar kompleksitas:** DET = 6; FTR = 1 → Low (3 FP)
- **Sprint:** 2

#### EI07 — Hapus slot parkir

- **Penjelasan:** Admin menghapus slot yang tidak dipakai lagi. Sistem memeriksa dulu apakah slot punya riwayat transaksi.
- **Elemen data (DET):** id_slot, tombol hapus
- **File terlibat:** Slot Parkir, Transaksi
- **Alasan klasifikasi:** EI: data masuk dari luar sistem lalu menghapus record di ILF Slot Parkir; ILF Transaksi dibaca untuk validasi.
- **Dasar kompleksitas:** DET = 2; FTR = 2 → Low (3 FP)
- **Sprint:** 2

#### EI08 — Ubah status slot (manual oleh admin)

- **Penjelasan:** Admin mengubah status slot secara manual (kosong, terisi, maintenance) untuk mensimulasikan kondisi miniatur. Perubahan dicatat di log.
- **Elemen data (DET):** id_slot, status_baru, tombol simpan
- **File terlibat:** Slot Parkir, Log Status Slot
- **Alasan klasifikasi:** EI: data masuk dari luar sistem lalu mengubah status di ILF Slot Parkir dan menambah catatan di ILF Log.
- **Dasar kompleksitas:** DET = 3; FTR = 2 → Low (3 FP)
- **Sprint:** 1

#### EI09 — Terima data status slot dari sensor / simulator (API)

- **Penjelasan:** Simulator (nanti sensor pada miniatur) mengirim data lewat API, misalnya 'slot P05 terisi'. Sistem memperbarui status slot dan mencatat log, sehingga dashboard ikut berubah.
- **Elemen data (DET):** id_slot, status, timestamp, device_id
- **File terlibat:** Slot Parkir, Log Status Slot
- **Alasan klasifikasi:** EI: data masuk dari perangkat luar (bukan pengguna) dan memperbarui ILF Slot Parkir serta Log. Bukan EIF karena sistem yang menyimpan datanya.
- **Dasar kompleksitas:** DET = 4; FTR = 2 → Low (3 FP)
- **Sprint:** 1
- **Catatan:** Logika mirip EI08 tapi sumber input berbeda. Jika dosen menganggap satu proses, gabungkan (UFP -3)

#### EI10 — Catat kendaraan masuk

- **Penjelasan:** Admin mencatat kendaraan masuk: memilih kendaraan dan slot kosong. Sistem membuat transaksi, mengubah slot menjadi terisi, dan mencatat log.
- **Elemen data (DET):** id_kendaraan, id_slot, waktu_masuk, tombol simpan
- **File terlibat:** Transaksi, Slot Parkir, Log Status Slot, Kendaraan
- **Alasan klasifikasi:** EI: data masuk dari luar sistem lalu menambah record di ILF Transaksi dan mengubah ILF Slot Parkir & Log; ILF Kendaraan dirujuk.
- **Dasar kompleksitas:** DET = 4; FTR = 4 → Average (4 FP)
- **Sprint:** 2
- **Catatan:** Membuat transaksi + mengubah status slot menjadi terisi

#### EI11 — Catat kendaraan keluar

- **Penjelasan:** Admin mencatat kendaraan keluar. Sistem mengisi waktu keluar, menutup transaksi, mengembalikan slot menjadi kosong, dan mencatat log.
- **Elemen data (DET):** id_transaksi, waktu_keluar, tombol simpan
- **File terlibat:** Transaksi, Slot Parkir, Log Status Slot
- **Alasan klasifikasi:** EI: data masuk dari luar sistem lalu mengubah ILF Transaksi, ILF Slot Parkir, dan ILF Log.
- **Dasar kompleksitas:** DET = 3; FTR = 3 → Average (4 FP)
- **Sprint:** 2
- **Catatan:** Menutup transaksi + slot kembali kosong

### 4.2 External Output (EO) — 2 fungsi, 10 UFP

| ID | Nama Fungsi | DET | FTR | Kompleksitas | Bobot | Sprint |
|---|---|---:|---:|---|---:|---:|
| EO01 | Dashboard ringkasan parkir | 9 | 2 | Average | 5 | 1 |
| EO02 | Laporan parkir per periode | 8 | 3 | Average | 5 | 2 |

#### EO01 — Dashboard ringkasan parkir

- **Penjelasan:** Menampilkan ringkasan kondisi parkir: total slot, jumlah terisi dan kosong, persentase okupansi, serta kendaraan masuk/keluar hari ini. Angka-angka ini dihitung sistem, bukan disimpan langsung.
- **Elemen data (DET):** total_slot, slot_terisi, slot_kosong, slot_maintenance, persentase_okupansi, kendaraan_parkir, masuk_hari_ini, keluar_hari_ini, waktu_update
- **File terlibat:** Slot Parkir, Transaksi
- **Alasan klasifikasi:** EO: informasi keluar dari sistem dan mengandung perhitungan (data turunan: hitungan & persentase).
- **Dasar kompleksitas:** DET = 9; FTR = 2 → Average (5 FP)
- **Sprint:** 1

#### EO02 — Laporan parkir per periode

- **Penjelasan:** Menghasilkan rekap parkir untuk periode yang dipilih: total transaksi, jumlah masuk/keluar, rata-rata durasi, jam tersibuk, dan rata-rata okupansi.
- **Elemen data (DET):** tgl_awal, tgl_akhir, total_transaksi, total_masuk, total_keluar, rata2_durasi, jam_tersibuk, rata2_okupansi
- **File terlibat:** Transaksi, Slot Parkir, Kendaraan
- **Alasan klasifikasi:** EO: keluaran berupa laporan hasil kalkulasi agregat (total, rata-rata).
- **Dasar kompleksitas:** DET = 8; FTR = 3 → Average (5 FP)
- **Sprint:** 2

### 4.3 External Inquiry (EQ) — 6 fungsi, 21 UFP

| ID | Nama Fungsi | DET | FTR | Kompleksitas | Bobot | Sprint |
|---|---|---:|---:|---|---:|---:|
| EQ01 | Lihat denah parkir & status slot | 7 | 3 | Average | 4 | 1 |
| EQ02 | Lihat daftar slot parkir | 5 | 1 | Low | 3 | 1 |
| EQ03 | Lihat daftar kendaraan | 5 | 2 | Low | 3 | 2 |
| EQ04 | Cari kendaraan berdasarkan plat | 7 | 3 | Average | 4 | 2 |
| EQ05 | Lihat riwayat parkir | 7 | 3 | Average | 4 | 2 |
| EQ06 | Lihat log perubahan status slot | 5 | 2 | Low | 3 | 2 |

#### EQ01 — Lihat denah parkir & status slot

- **Penjelasan:** Menampilkan denah visual seluruh slot beserta warna statusnya, dan plat kendaraan yang sedang parkir di slot terisi. Ini inti tampilan Digital Twin.
- **Elemen data (DET):** nomor_slot, jenis, status, pos_x, pos_y, plat_nomor, waktu_masuk
- **File terlibat:** Slot Parkir, Transaksi, Kendaraan
- **Alasan klasifikasi:** EQ: hanya mengambil dan menampilkan data tersimpan, tanpa perhitungan dan tanpa mengubah data.
- **Dasar kompleksitas:** DET = 7; FTR = 3 → Average (4 FP)
- **Sprint:** 1

#### EQ02 — Lihat daftar slot parkir

- **Penjelasan:** Menampilkan tabel semua slot beserta jenis, status, dan posisinya; dipakai admin untuk mengelola slot.
- **Elemen data (DET):** nomor_slot, jenis, status, pos_x, pos_y
- **File terlibat:** Slot Parkir
- **Alasan klasifikasi:** EQ: pengambilan data slot tanpa perhitungan.
- **Dasar kompleksitas:** DET = 5; FTR = 1 → Low (3 FP)
- **Sprint:** 1
- **Catatan:** Halaman tabel untuk pengelolaan slot

#### EQ03 — Lihat daftar kendaraan

- **Penjelasan:** Menampilkan daftar kendaraan terdaftar beserta status parkirnya (sedang parkir atau sudah keluar).
- **Elemen data (DET):** id_kendaraan, plat_nomor, jenis_kendaraan, pemilik, status parkir
- **File terlibat:** Kendaraan, Transaksi
- **Alasan klasifikasi:** EQ: pengambilan data tersimpan tanpa perhitungan.
- **Dasar kompleksitas:** DET = 5; FTR = 2 → Low (3 FP)
- **Sprint:** 2

#### EQ04 — Cari kendaraan berdasarkan plat

- **Penjelasan:** Admin mengetik plat nomor untuk mengetahui data kendaraan dan apakah sedang parkir, beserta di slot mana.
- **Elemen data (DET):** plat_nomor (input & output dihitung sekali), jenis, pemilik, status, nomor_slot, waktu_masuk, tombol cari
- **File terlibat:** Kendaraan, Transaksi, Slot Parkir
- **Alasan klasifikasi:** EQ: ada input pencarian dan output hasil, tanpa perhitungan dan tanpa perubahan data.
- **Dasar kompleksitas:** DET = 7; FTR = 3 → Average (4 FP)
- **Sprint:** 2

#### EQ05 — Lihat riwayat parkir

- **Penjelasan:** Menampilkan riwayat kendaraan masuk dan keluar; dapat difilter berdasarkan rentang tanggal dan plat nomor.
- **Elemen data (DET):** tgl_awal, tgl_akhir, plat_nomor, waktu_masuk, waktu_keluar, nomor_slot, status
- **File terlibat:** Transaksi, Kendaraan, Slot Parkir
- **Alasan klasifikasi:** EQ: pengambilan data riwayat dengan filter, tanpa perhitungan.
- **Dasar kompleksitas:** DET = 7; FTR = 3 → Average (4 FP)
- **Sprint:** 2

#### EQ06 — Lihat log perubahan status slot

- **Penjelasan:** Menampilkan jejak perubahan status slot: status lama ke status baru, sumber (manual/sensor), dan waktunya.
- **Elemen data (DET):** id_slot (filter), status_lama, status_baru, sumber, waktu
- **File terlibat:** Log Status Slot, Slot Parkir
- **Alasan klasifikasi:** EQ: pengambilan data log tanpa perhitungan.
- **Dasar kompleksitas:** DET = 5; FTR = 2 → Low (3 FP)
- **Sprint:** 2
- **Catatan:** Bagian dari usulan tabel log

### 4.4 Internal Logical File (ILF) — 5 fungsi, 35 UFP

| ID | Nama Fungsi | DET | RET | Kompleksitas | Bobot | Sprint |
|---|---|---:|---:|---|---:|---:|
| ILF01 | Users | 5 | 1 | Low | 7 | 1 |
| ILF02 | Kendaraan | 4 | 1 | Low | 7 | 1 |
| ILF03 | Slot Parkir | 6 | 1 | Low | 7 | 1 |
| ILF04 | Transaksi Parkir | 6 | 1 | Low | 7 | 1 |
| ILF05 | Log Status Slot | 6 | 1 | Low | 7 | 1 |

#### ILF01 — Users

- **Penjelasan:** Data pengguna sistem (admin): nama, email, password (hash), dan role. Dipakai untuk proses login.
- **Elemen data (DET):** id, nama, email, password, role
- **Struktur:** 1 RET
- **Alasan klasifikasi:** ILF: kelompok data yang dipelihara sistem sendiri (ditambah/diubah lewat proses di dalam batas sistem).
- **Dasar kompleksitas:** DET = 5; RET = 1 → Low (7 FP)
- **Sprint:** 1

#### ILF02 — Kendaraan

- **Penjelasan:** Kumpulan data kendaraan terdaftar: plat nomor, jenis kendaraan, dan pemiliknya.
- **Elemen data (DET):** id_kendaraan, plat_nomor, jenis_kendaraan, id_user
- **Struktur:** 1 RET
- **Alasan klasifikasi:** ILF: data kendaraan dipelihara sistem lewat EI02-EI04.
- **Dasar kompleksitas:** DET = 4; RET = 1 → Low (7 FP)
- **Sprint:** 1

#### ILF03 — Slot Parkir

- **Penjelasan:** Data master slot parkir beserta status terkininya dan posisi pada denah. Ini representasi digital dari slot pada miniatur.
- **Elemen data (DET):** id_slot, nomor_slot, jenis, status, pos_x, pos_y
- **Struktur:** 1 RET
- **Alasan klasifikasi:** ILF: data slot dipelihara sistem lewat EI05-EI09.
- **Dasar kompleksitas:** DET = 6; RET = 1 → Low (7 FP)
- **Sprint:** 1
- **Catatan:** pos_x/pos_y = usulan tambahan untuk denah

#### ILF04 — Transaksi Parkir

- **Penjelasan:** Data transaksi parkir: kendaraan mana, di slot mana, waktu masuk dan keluar, serta status transaksi.
- **Elemen data (DET):** id_transaksi, id_kendaraan, id_slot, waktu_masuk, waktu_keluar, status
- **Struktur:** 1 RET
- **Alasan klasifikasi:** ILF: data transaksi dipelihara sistem lewat EI10-EI11.
- **Dasar kompleksitas:** DET = 6; RET = 1 → Low (7 FP)
- **Sprint:** 1

#### ILF05 — Log Status Slot

- **Penjelasan:** Rekam jejak setiap perubahan status slot, sebagai riwayat kondisi Digital Twin.
- **Elemen data (DET):** id_log, id_slot, status_lama, status_baru, sumber, waktu
- **Struktur:** 1 RET
- **Alasan klasifikasi:** ILF: data log dipelihara sistem otomatis saat status slot berubah.
- **Dasar kompleksitas:** DET = 6; RET = 1 → Low (7 FP)
- **Sprint:** 1
- **Catatan:** Usulan tambahan (riwayat kondisi = inti digital twin)

### 4.5 External Interface File (EIF) — 0 fungsi, 0 UFP

Belum ada sistem eksternal yang datanya hanya dibaca pada Sprint 1. Data dari sensor atau simulator yang masuk lewat API dihitung sebagai EI (EI09), bukan EIF.

**Total UFP = 102**

## 5. Faktor Penyesuaian (14 GSC)

Skala 0-5: 0 = tidak berpengaruh, 1 = sedikit, 2 = cukup, 3 = rata-rata, 4 = signifikan, 5 = sangat kuat.

| No | Karakteristik | Nilai | Alasan Penilaian |
|---:|---|---:|---|
| 1 | Data Communications | 3 | Aplikasi web (HTTP) + API untuk data simulator/sensor |
| 2 | Distributed Data Processing | 1 | Pemrosesan utama di server; browser hanya menampilkan |
| 3 | Performance | 3 | Status slot perlu tampil cukup real-time (polling berkala) |
| 4 | Heavily Used Configuration | 1 | Prototype kampus, beban rendah, tanpa batasan hardware khusus |
| 5 | Transaction Rate | 2 | Transaksi masuk/keluar kendaraan berfrekuensi rendah-sedang |
| 6 | On-line Data Entry | 5 | Hampir seluruh input dilakukan lewat form web |
| 7 | End-User Efficiency | 3 | Dashboard dan denah visual memudahkan admin, tanpa fitur lanjutan |
| 8 | On-line Update | 3 | Data slot, kendaraan, dan transaksi diperbarui online |
| 9 | Complex Processing | 1 | Logika sederhana: validasi slot kosong, hitung okupansi/durasi |
| 10 | Reusability | 1 | Kode belum dirancang sebagai komponen yang dipakai ulang |
| 11 | Installation Ease | 1 | Instalasi lokal sederhana (XAMPP/hosting kampus) |
| 12 | Operational Ease | 1 | Tidak ada prosedur backup/recovery otomatis |
| 13 | Multiple Sites | 0 | Hanya satu lokasi parkir |
| 14 | Facilitate Change | 2 | Struktur folder & database dipisah, ada dokumentasi dasar |
| | **Total** | **27** | VAF = 0,65 + 0,01 × 27 = **0,92** |

## 6. Catatan dan Asumsi

- DET, FTR/RET, dan pembagian Sprint adalah estimasi awal berdasarkan rancangan tabel & fitur. Ubah sel biru di sheet Daftar Fungsi, hasil akan terhitung ulang otomatis.
- EIF = 0: pada Sprint 1 belum ada sistem eksternal yang datanya hanya dibaca. Data sensor/simulator masuk lewat API dihitung sebagai EI (EI09), bukan EIF.
- Tabel Log Status Slot dan kolom pos_x/pos_y adalah usulan tambahan di luar ERD awal. Jika tidak dipakai, hapus ILF05 dan EQ06, lalu kurangi FTR pada EI08-EI11 dan EQ06 yang menyebut Log.
- Operasi tambah/ubah/hapus dihitung sebagai EI terpisah karena proses dan DET-nya berbeda (aturan IFPUG).
- Nilai GSC (sheet Faktor GSC) adalah penilaian awal. Sesuaikan dengan ketentuan dosen jika metode yang diminta berbeda (mis. bobot sederhana tanpa VAF).
- UFP/AFP Sprint 1 memakai VAF yang sama dengan seluruh produk. Kolom Sprint 1 hanya memuat fungsi yang ditandai Sprint = 1.

## 7. Yang Perlu Dicek Sebelum Dikumpulkan

- Cocokkan metode hitung (IFPUG penuh atau bobot sederhana, dengan atau tanpa VAF) dengan instruksi asli dosen.
- Jika ERD final berbeda, perbarui DET dan RET pada ILF serta fungsi yang terkait.
- Jika Log Status Slot tidak dipakai, hapus ILF05 dan EQ06, lalu kurangi FTR pada fungsi yang menyebut Log.
- Jika EI08 dan EI09 dianggap satu proses, gabungkan keduanya (UFP turun 3).
