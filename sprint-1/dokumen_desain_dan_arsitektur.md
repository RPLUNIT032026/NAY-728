# Dokumen Desain dan Arsitektur
**Digital Twin Sistem Parkir | Sprint 1 / Product Release 1 | 30 September 2026**

Dokumen ini memuat rancangan yang diminta pada tugas Sprint 1: rancangan UX/UI, rancangan sistem (flowchart, arsitektur, ERD), dan penjelasan kerangka dasar project. Semua gambar juga tersedia sebagai file terpisah di folder `docs/`.

| Kebutuhan tugas | Isi dokumen | Bagian |
| :--- | :--- | :--- |
| Rancangan UX/UI (wireframe, mockup, purwarupa) | 5 wireframe halaman + prototype berjalan di folder `frontend/` | 1 dan 3 |
| Rancangan sistem: flowchart | 3 flowchart: kendaraan masuk, kendaraan keluar, sinkronisasi miniatur | 2.1 |
| Rancangan sistem: arsitektur sistem | Diagram arsitektur physical twin dan digital twin | 2.2 |
| Rancangan sistem: struktur database (ERD) | ERD 5 tabel beserta kamus data | 2.3 |
| Kerangka dasar (boilerplate / starter code) | Struktur folder, prototype frontend, kerangka backend PHP, `database.sql` | 3 |

## Konsep Singkat
Miniatur parkir adalah *physical twin* (dunia nyata). Website atau dashboard adalah *digital twin*: menampilkan denah, status slot, kendaraan, dan riwayat. Pada Sprint 1 sensor belum dipasang, sehingga data slot berasal dari simulator yang mengirim data dengan format yang sama seperti nanti dikirim sensor. Dengan begitu, saat sensor asli dipasang, backend dan dashboard tidak perlu diubah.

---

## 1. Rancangan UX/UI
Alur pengguna: admin masuk lewat halaman login, melihat ringkasan di dashboard, membuka denah untuk melihat atau mengubah status slot, lalu membuka data kendaraan dan riwayat bila perlu. Navigasi memakai sidebar tetap di kiri.

### Prinsip Desain
| Aspek | Keputusan |
| :--- | :--- |
| Warna status | Hijau kosong, merah terisi, abu-biru maintenance. Status selalu ditulis dengan teks, tidak hanya warna. |
| Denah | Slot digambar di atas latar aspal gelap dengan tanda MASUK dan KELUAR, meniru area parkir sebenarnya. |
| Aksen | Kuning marka jalan dipakai hanya untuk menu aktif dan slot yang dipilih. |
| Tata letak | Sidebar tetap + area konten. Pada layar kecil sidebar berubah menjadi bar atas. |
| Bahasa | Kalimat singkat dan langsung, misalnya tombol 'Simpan status' dan 'Simulasikan perubahan'. |

### Wireframe 1 - Login
* **Tampilan:** Kotak login tengah dengan logo "Parking Twin", input `Email`, `Password`, dan tombol `Masuk`.
* **Fungsi terkait:** `El01` (Login admin). Pesan kesalahan muncul di bawah tombol jika email atau password salah.

### Wireframe 2 - Dashboard
* **Tampilan:** Empat kartu angka (Total slot, Terisi, Kosong, Okupansi), denah ringkas (12 slot), dan lima aktivitas terakhir.
* **Fungsi terkait:** `E001` (Dashboard ringkasan). Tombol simulasi mengubah satu slot secara acak.

### Wireframe 3 - Denah Parkir
* **Tampilan:** Denah interaktif area parkir dengan tombol status, panel detail slot di sisi kanan, serta tombol simulasi otomatis.
* **Fungsi terkait:** `EQ01` (denah), `E108` (ubah status manual), `E109` (data simulator). Klik slot untuk membuka panel detail di kanan.

### Wireframe 4 - Data Kendaraan
* **Tampilan:** Tabel daftar kendaraan terdaftar, kolom pencarian plat nomor, serta tombol `+ Tambah kendaraan`.
* **Fungsi terkait:** `EQ03` dan `EQ04`. Sprint 1 hanya menampilkan dan mencari; tombol tambah kendaraan aktif di Sprint 2.

### Wireframe 5 - Riwayat Parkir
* **Tampilan:** Tabel log catatan kendaraan masuk, keluar, dan perubahan status slot dengan filter plat nomor, status, dan tanggal.
* **Fungsi terkait:** `EQ05` dan `EQ06`. Filter plat, status, dan tanggal. Sprint 1 membaca log perubahan status slot.

---

## 2. Rancangan Sistem

### 2.1 Flowchart
Tiga alur utama: kendaraan masuk, kendaraan keluar, dan sinkronisasi data dari miniatur ke digital twin.

1. **Flowchart 1 - Kendaraan Masuk:**
   * Admin memasukkan plat nomor kendaraan $\rightarrow$ Cek apakah kendaraan sudah terdaftar. Jika belum $\rightarrow$ Tambah data kendaraan (plat, jenis, pemilik).
   * Cek apakah ada slot kosong sesuai jenis kendaraan. Jika tidak $\rightarrow$ Tampilkan pesan 'Parkir penuh'. Jika ya $\rightarrow$ Pilih slot kosong $\rightarrow$ Simpan transaksi parkir (`waktu_masuk`, `status = aktif`) $\rightarrow$ Ubah status slot menjadi 'terisi' $\rightarrow$ Catat perubahan ke log status slot $\rightarrow$ Digital Twin diperbarui (denah dan dashboard).

2. **Flowchart 2 - Kendaraan Keluar:**
   * Admin memasukkan plat nomor kendaraan $\rightarrow$ Cek apakah ada transaksi parkir yang masih aktif. Jika tidak $\rightarrow$ Tampilkan pesan 'Kendaraan tidak sedang parkir'. Jika ya $\rightarrow$ Isi waktu keluar dan tutup transaksi (`status = selesai`) $\rightarrow$ Ubah status slot menjadi 'kosong' $\rightarrow$ Catat perubahan ke log status slot $\rightarrow$ Digital Twin diperbarui (denah dan dashboard).

3. **Flowchart 3 - Sinkronisasi Miniatur ke Digital Twin:**
   * Sensor miniatur/simulator membaca kondisi slot $\rightarrow$ Kirim data ke API (`nomor_slot`, `status`, `timestamp`, `device_id`) $\rightarrow$ Validasi data dan slot. Jika tidak valid/tidak dikenal $\rightarrow$ API menolak data (respons 400/404). Jika valid $\rightarrow$ Perbarui status slot di database $\rightarrow$ Catat ke log status slot (`sumber = sensor/simulator`) $\rightarrow$ Dashboard mengambil data terbaru (polling tiap beberapa detik) $\rightarrow$ Denah dan angka dashboard berubah sesuai kondisi miniatur.

### 2.2 Arsitektur Sistem
Sistem dibagi menjadi **Dunia Fisik (Physical Twin)** dan **Digital Twin (Sistem Web)**.

| Komponen | Peran | Sprint |
| :--- | :--- | :--- |
| Miniatur parkir | Representasi fisik area parkir (12 slot). | Berikutnya |
| Sensor + mikrokontroler | Membaca terisi atau kosong tiap slot lalu mengirim ke API lewat Wi-Fi. | Berikutnya |
| Simulator data | Sensor/miniatur masih menggunakan simulasi data pada tahap prototype. Mengubah status slot dengan format yang sama seperti sensor. | 1 |
| Backend / REST API (PHP) | Menerima data sensor, memeriksa validasi, menyimpan ke database, melayani dashboard. | 1 (kerangka) |
| Database MySQL | Menyimpan pengguna, kendaraan, slot, transaksi, dan log status slot. | 1 |
| Dashboard web | Menampilkan denah, status, kendaraan, dan riwayat; mengambil data terbaru dengan polling. | 1 (prototype) |

**Contoh format data sensor/simulator (`POST /api/sensor.php`):**
```json
X-Device-Key: <kunci perangkat>
{
  "nomor_slot": "P05",
  "status": "terisi",
  "device_id": "sim-01",
  "timestamp": "2026-09-30T10:42:15+07:00"
}
```

### 2.3 ERD dan Kamus Data
Relasi antar tabel dalam database:
* `users` ($1:N$) $\rightarrow$ `kendaraan`
* `kendaraan` ($1:N$) $\rightarrow$ `transaksi_parkir`
* `slot_parkir` ($1:N$) $\rightarrow$ `transaksi_parkir` & `log_status_slot`

| Tabel | Kolom | Keterangan |
| :--- | :--- | :--- |
| `users` | `id` (PK), `nama`, `email`, `password`, `role` | Akun admin/operator. Password disimpan sebagai hash bcrypt. |
| `kendaraan` | `id_kendaraan` (PK), `plat_nomor`, `jenis_kendaraan`, `id_user` (FK) | Kendaraan terdaftar dan pemiliknya. |
| `slot_parkir` | `id_slot` (PK), `nomor_slot`, `jenis`, `status`, `pos_x`, `pos_y` | Slot beserta status terkini dan posisi pada denah. |
| `transaksi_parkir` | `id_transaksi` (PK), `id_kendaraan` (FK), `id_slot` (FK), `waktu_masuk`, `waktu_keluar`, `status` | Satu baris tiap kendaraan parkir. `waktu_keluar` kosong selama status 'aktif'. |
| `log_status_slot` | `id_log` (PK), `id_slot` (FK), `status_lama`, `status_baru`, `sumber`, `waktu` | Riwayat kondisi twin. `sumber` = manual, simulator, atau sensor. |

*Catatan: Tabel `log_status_slot` dan kolom `pos_x`/`pos_y` adalah tambahan di atas rancangan awal, dipakai untuk riwayat kondisi dan penggambaran denah.*

---

## 3. Inisialisasi Kode Proyek
Kerangka awal sudah dapat dijalankan sehingga project siap masuk ke tahap development Product Release 1.

### Struktur Folder
```text
digital-twin-parkir/
├── README.md
├── docs/                      # wireframe, flowchart, arsitektur, ERD, Function Point
├── frontend/
│   ├── index.html             # login
│   ├── dashboard.html
│   ├── denah.html
│   ├── kendaraan.html
│   ├── riwayat.html
│   ├── css/style.css
│   └── js/app.js              # data simulasi + logika tampilan
├── backend/
│   ├── config.php
│   ├── db.php
│   └── api/
│       ├── login.php
│       ├── slots.php
│       └── sensor.php
└── database/
    └── database.sql           # skema + data awal
```

### Cara Menjalankan Bagian Proyek
| Bagian | Kondisi saat ini | Cara mencoba |
| :--- | :--- | :--- |
| **Frontend** | Lima halaman berjalan dengan data simulasi di browser. Tombol simulasi mengubah status slot dan mencatat riwayat. | Buka `frontend/index.html`. Akun demo: `admin@parkir.test` / `admin123`. |
| **Database** | Lima tabel, kunci relasi, dan data awal (12 slot, 8 kendaraan, akun admin). | `mysql -u root -p < database/database.sql` |
| **Backend** | Kerangka API PHP: login, daftar dan ubah slot, penerimaan data sensor. Belum terhubung ke frontend dan belum diuji terhadap MySQL. | `php -S localhost:8080 -t backend` |

### Rencana Sprint Berikutnya
Hubungkan frontend ke API dengan polling, buat CRUD kendaraan dan slot, catat kendaraan masuk dan keluar, buat skrip simulator yang mengirim data ke API, lalu integrasikan sensor pada miniatur.