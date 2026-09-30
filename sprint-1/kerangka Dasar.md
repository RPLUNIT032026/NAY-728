# Inisialisasi Kode Proyek (Project Setup) — Digital Twin Sistem Parkir

**Kerangka Dasar (Boilerplate / Starter Code)** untuk Sprint 1 / Product Release 1: struktur folder awal, kerangka kode, dan inisialisasi basis data yang menandakan proyek siap masuk ke tahap development.

Seluruh file di bawah ini juga ada di `digital-twin-parkir.zip`, di folder dengan nama yang sama.

## 1. Struktur folder awal

```
digital-twin-parkir/
├── README.md
├── docs/                    wireframe, flowchart, arsitektur, ERD,
│                            Function Point, dokumen desain
├── frontend/
│   ├── index.html           login
│   ├── dashboard.html
│   ├── denah.html
│   ├── kendaraan.html
│   ├── riwayat.html
│   ├── css/style.css
│   └── js/app.js            data simulasi dan logika tampilan
├── backend/
│   ├── config.php
│   ├── db.php
│   └── api/
│       ├── login.php
│       ├── slots.php
│       └── sensor.php
└── database/
    └── database.sql         skema dan data awal
```

## 2. Ringkasan kerangka

| Bagian | Kondisi saat ini | Cara mencoba |
|---|---|---|
| Frontend | Lima halaman berjalan dengan data simulasi di browser. Tombol simulasi mengubah status slot dan mencatat riwayat. | Buka `frontend/index.html`. Akun demo `admin@parkir.test` / `admin123`. |
| Database | Lima tabel, kunci relasi, dan data awal. | `mysql -u root -p < database/database.sql` |
| Backend | Kerangka API PHP: login, daftar dan ubah slot, penerimaan data sensor. Belum terhubung ke frontend. | `php -S localhost:8080 -t backend` |

> **Status pengujian:** frontend sudah diuji otomatis tanpa error. SQL lolos pemeriksaan sintaks tetapi belum dijalankan di MySQL. **Kode PHP belum pernah dijalankan**, jadi uji dulu di komputer lokal.

## 3. Cara menjalankan

**Frontend**

```bash
cd frontend
python3 -m http.server 8000
```

Buka http://localhost:8000, login dengan `admin@parkir.test` / `admin123`.

**Database dan backend**

1. Pastikan MySQL/MariaDB dan PHP 8 aktif (XAMPP atau Laragon juga bisa).
2. Impor skema: `mysql -u root -p < database/database.sql`
3. Sesuaikan `backend/config.php` atau variabel lingkungan `DB_HOST`, `DB_NAME`, `DB_USER`, `DB_PASS`, `DEVICE_KEY`.
4. Jalankan: `php -S localhost:8080 -t backend`

## 4. Isi file

### 4.1 `README.md`

Deskripsi project, tujuan, teknologi, struktur folder, cara menjalankan, dan daftar tim.

````markdown
# Digital Twin Sistem Parkir

Digital Twin Sistem Parkir merepresentasikan kondisi area parkir dalam bentuk digital dan disiapkan untuk terhubung dengan miniatur parkir sebagai dunia nyata (physical twin). Dashboard web menjadi digital twin-nya.

## Tujuan

- Memantau status slot parkir (kosong, terisi, maintenance)
- Menampilkan denah dan ringkasan kondisi parkir
- Menyimpan data kendaraan dan transaksi parkir
- Menampilkan riwayat parkir dan log perubahan status slot

## Status: Sprint 1 / Product Release 1

| Bagian | Status |
|---|---|
| Dokumen desain (wireframe, flowchart, arsitektur, ERD) | Selesai, ada di `docs/` |
| Function Point | Selesai, ada di `docs/` |
| Prototype frontend (login, dashboard, denah, kendaraan, riwayat) | Berjalan dengan data simulasi di browser |
| Skema database dan data awal | Selesai, `database/database.sql` |
| Kerangka backend PHP (login, slots, sensor) | Kerangka awal, belum terhubung ke frontend |
| Integrasi sensor/miniatur | Sprint berikutnya |

## Teknologi

- Frontend: HTML, CSS, JavaScript (tanpa framework)
- Backend: PHP (PDO) dengan REST API sederhana
- Database: MySQL / MariaDB
- Miniatur (rencana): sensor slot + ESP32/Arduino

## Struktur folder

```
digital-twin-parkir/
├── README.md
├── docs/                    Dokumen desain dan perhitungan
│   ├── wireframe-1-login.png ... wireframe-5-riwayat.png
│   ├── flowchart-masuk.png, flowchart-keluar.png, flowchart-sinkronisasi.png
│   ├── architecture.png
│   ├── erd.png
│   ├── 03_Function_Point.xlsx / .md
│   └── 04_Desain_dan_Arsitektur.pdf / .md
├── frontend/                Prototype web
│   ├── index.html           Login
│   ├── dashboard.html
│   ├── denah.html
│   ├── kendaraan.html
│   ├── riwayat.html
│   ├── css/style.css
│   └── js/app.js
├── backend/                 Kerangka API PHP
│   ├── config.php
│   ├── db.php
│   └── api/  login.php, slots.php, sensor.php
└── database/
    └── database.sql         Skema + data awal
```

## Menjalankan prototype frontend

Buka `frontend/index.html` di browser, atau jalankan server statis:

```
cd frontend
python3 -m http.server 8000
```

Lalu buka http://localhost:8000. Akun demo: `admin@parkir.test` / `admin123`.

Data disimpan di `localStorage` browser dan berubah lewat tombol simulasi. Tombol "Reset data" di halaman Denah mengembalikan kondisi awal.

## Menyiapkan database dan backend

1. Pastikan MySQL/MariaDB dan PHP 8 aktif (XAMPP atau Laragon juga bisa).
2. Impor skema: `mysql -u root -p < database/database.sql`
3. Sesuaikan `backend/config.php` (atau variabel lingkungan `DB_HOST`, `DB_NAME`, `DB_USER`, `DB_PASS`, `DEVICE_KEY`).
4. Jalankan server pengembangan: `php -S localhost:8080 -t backend`
5. Coba endpoint:
   - `GET  http://localhost:8080/api/slots.php`
   - `POST http://localhost:8080/api/login.php` dengan body `{"email":"admin@parkir.test","password":"admin123"}`
   - `POST http://localhost:8080/api/sensor.php` dengan header `X-Device-Key` dan body `{"nomor_slot":"P05","status":"terisi","device_id":"sim-01"}`

Catatan: kode PHP di `backend/` adalah kerangka awal dan belum diuji terhadap MySQL. Uji dan sesuaikan sebelum dipakai pada sprint berikutnya.

## Rencana sprint berikutnya

- Hubungkan frontend ke API (`GET /api/slots.php` dengan polling)
- CRUD kendaraan dan slot, pencatatan kendaraan masuk dan keluar
- Halaman riwayat membaca database, laporan parkir
- Skrip simulator yang mengirim data ke `/api/sensor.php`
- Integrasi sensor miniatur (ESP32/Arduino)

## Tim

| Nama | Peran |
|---|---|
| (isi nama anggota 1) | Project Manager |
| (isi nama anggota 2) | UI/UX Designer |
| (isi nama anggota 3) | Programmer |
| (isi nama anggota 4) | Database |
| (isi nama anggota 5) | Hardware / Miniatur |

## Mengunggah ke GitHub

```
git init
git add README.md && git commit -m "Initial project setup"
git add docs/03_Function_Point.* && git commit -m "Add function point calculation"
git add docs/wireframe-*.png && git commit -m "Add UI wireframe"
git add docs/flowchart-*.png docs/architecture.png docs/erd.png docs/04_Desain_dan_Arsitektur.* && git commit -m "Add system design diagrams"
git add database && git commit -m "Add database structure"
git add frontend backend && git commit -m "Add frontend prototype and backend skeleton"
git branch -M main
git remote add origin https://github.com/<username>/digital-twin-parkir.git
git push -u origin main
```
````

### 4.2 `database/database.sql`

Inisialisasi basis data: 5 tabel dengan relasi dan data awal (12 slot, 8 kendaraan, akun admin).

```sql
-- Digital Twin Sistem Parkir - skema database awal (MySQL / MariaDB)
-- Jalankan: mysql -u root -p < database/database.sql

CREATE DATABASE IF NOT EXISTS digital_twin_parkir CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE digital_twin_parkir;

DROP TABLE IF EXISTS log_status_slot;
DROP TABLE IF EXISTS transaksi_parkir;
DROP TABLE IF EXISTS kendaraan;
DROP TABLE IF EXISTS slot_parkir;
DROP TABLE IF EXISTS users;

CREATE TABLE users (
  id INT UNSIGNED NOT NULL AUTO_INCREMENT,
  nama VARCHAR(100) NOT NULL,
  email VARCHAR(100) NOT NULL,
  password VARCHAR(255) NOT NULL COMMENT 'hash bcrypt, jangan simpan password asli',
  role ENUM('admin','operator') NOT NULL DEFAULT 'admin',
  PRIMARY KEY (id),
  UNIQUE KEY uq_users_email (email)
) ENGINE=InnoDB;

CREATE TABLE kendaraan (
  id_kendaraan INT UNSIGNED NOT NULL AUTO_INCREMENT,
  plat_nomor VARCHAR(15) NOT NULL,
  jenis_kendaraan ENUM('mobil','motor') NOT NULL,
  id_user INT UNSIGNED NULL COMMENT 'pemilik',
  PRIMARY KEY (id_kendaraan),
  UNIQUE KEY uq_kendaraan_plat (plat_nomor),
  CONSTRAINT fk_kendaraan_user FOREIGN KEY (id_user) REFERENCES users (id) ON DELETE SET NULL ON UPDATE CASCADE
) ENGINE=InnoDB;

CREATE TABLE slot_parkir (
  id_slot INT UNSIGNED NOT NULL AUTO_INCREMENT,
  nomor_slot VARCHAR(10) NOT NULL,
  jenis ENUM('mobil','motor') NOT NULL,
  status ENUM('kosong','terisi','maintenance') NOT NULL DEFAULT 'kosong',
  pos_x TINYINT UNSIGNED NOT NULL COMMENT 'kolom pada denah',
  pos_y TINYINT UNSIGNED NOT NULL COMMENT 'baris pada denah',
  PRIMARY KEY (id_slot),
  UNIQUE KEY uq_slot_nomor (nomor_slot)
) ENGINE=InnoDB;

CREATE TABLE transaksi_parkir (
  id_transaksi INT UNSIGNED NOT NULL AUTO_INCREMENT,
  id_kendaraan INT UNSIGNED NOT NULL,
  id_slot INT UNSIGNED NOT NULL,
  waktu_masuk DATETIME NOT NULL,
  waktu_keluar DATETIME NULL,
  status ENUM('aktif','selesai') NOT NULL DEFAULT 'aktif',
  PRIMARY KEY (id_transaksi),
  KEY idx_transaksi_status (status),
  CONSTRAINT fk_transaksi_kendaraan FOREIGN KEY (id_kendaraan) REFERENCES kendaraan (id_kendaraan) ON UPDATE CASCADE,
  CONSTRAINT fk_transaksi_slot FOREIGN KEY (id_slot) REFERENCES slot_parkir (id_slot) ON UPDATE CASCADE
) ENGINE=InnoDB;

CREATE TABLE log_status_slot (
  id_log INT UNSIGNED NOT NULL AUTO_INCREMENT,
  id_slot INT UNSIGNED NOT NULL,
  status_lama VARCHAR(15) NOT NULL,
  status_baru VARCHAR(15) NOT NULL,
  sumber ENUM('manual','simulator','sensor') NOT NULL DEFAULT 'manual',
  waktu DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id_log),
  KEY idx_log_waktu (waktu),
  CONSTRAINT fk_log_slot FOREIGN KEY (id_slot) REFERENCES slot_parkir (id_slot) ON UPDATE CASCADE
) ENGINE=InnoDB;

-- ---------- Data awal (seed) ----------
-- Akun demo: admin@parkir.test / admin123
INSERT INTO users (nama, email, password, role) VALUES
  ('Administrator', 'admin@parkir.test', '$2y$10$Efzyo2Ty1ThAzLkgLbcWfO4CIHE5plZxe0c3AFlvT.mWqvuYltOxW', 'admin');

INSERT INTO slot_parkir (nomor_slot, jenis, status, pos_x, pos_y) VALUES
  ('P01','mobil','terisi',1,1), ('P02','mobil','kosong',2,1), ('P03','mobil','kosong',3,1), ('P04','mobil','terisi',4,1),
  ('P05','mobil','kosong',1,2), ('P06','mobil','kosong',2,2), ('P07','mobil','maintenance',3,2), ('P08','mobil','kosong',4,2),
  ('P09','motor','kosong',1,3), ('P10','motor','terisi',2,3), ('P11','motor','terisi',3,3), ('P12','motor','kosong',4,3);

INSERT INTO kendaraan (plat_nomor, jenis_kendaraan, id_user) VALUES
  ('BL 1234 XX','mobil',NULL), ('BL 3456 XX','mobil',NULL), ('BL 9012 XX','mobil',NULL), ('BL 2468 XX','mobil',NULL),
  ('BL 5678 XX','motor',NULL), ('BL 7788 XX','motor',NULL), ('BL 1357 XX','motor',NULL), ('BL 8642 XX','motor',NULL);

-- Transaksi aktif untuk slot yang terisi
INSERT INTO transaksi_parkir (id_kendaraan, id_slot, waktu_masuk, status) VALUES
  ((SELECT id_kendaraan FROM kendaraan WHERE plat_nomor='BL 1234 XX'), (SELECT id_slot FROM slot_parkir WHERE nomor_slot='P01'), CONCAT(CURDATE(),' 07:58:00'), 'aktif'),
  ((SELECT id_kendaraan FROM kendaraan WHERE plat_nomor='BL 3456 XX'), (SELECT id_slot FROM slot_parkir WHERE nomor_slot='P04'), CONCAT(CURDATE(),' 08:20:00'), 'aktif'),
  ((SELECT id_kendaraan FROM kendaraan WHERE plat_nomor='BL 5678 XX'), (SELECT id_slot FROM slot_parkir WHERE nomor_slot='P10'), CONCAT(CURDATE(),' 08:41:00'), 'aktif'),
  ((SELECT id_kendaraan FROM kendaraan WHERE plat_nomor='BL 7788 XX'), (SELECT id_slot FROM slot_parkir WHERE nomor_slot='P11'), CONCAT(CURDATE(),' 09:47:00'), 'aktif');
-- Transaksi selesai
INSERT INTO transaksi_parkir (id_kendaraan, id_slot, waktu_masuk, waktu_keluar, status) VALUES
  ((SELECT id_kendaraan FROM kendaraan WHERE plat_nomor='BL 2468 XX'), (SELECT id_slot FROM slot_parkir WHERE nomor_slot='P02'), CONCAT(CURDATE(),' 09:05:00'), CONCAT(CURDATE(),' 09:32:00'), 'selesai');

INSERT INTO log_status_slot (id_slot, status_lama, status_baru, sumber, waktu) VALUES
  ((SELECT id_slot FROM slot_parkir WHERE nomor_slot='P01'), 'kosong', 'terisi', 'simulator', CONCAT(CURDATE(),' 07:58:00')),
  ((SELECT id_slot FROM slot_parkir WHERE nomor_slot='P04'), 'kosong', 'terisi', 'simulator', CONCAT(CURDATE(),' 08:20:00')),
  ((SELECT id_slot FROM slot_parkir WHERE nomor_slot='P10'), 'kosong', 'terisi', 'simulator', CONCAT(CURDATE(),' 08:41:00')),
  ((SELECT id_slot FROM slot_parkir WHERE nomor_slot='P02'), 'kosong', 'terisi', 'simulator', CONCAT(CURDATE(),' 09:05:00')),
  ((SELECT id_slot FROM slot_parkir WHERE nomor_slot='P02'), 'terisi', 'kosong', 'simulator', CONCAT(CURDATE(),' 09:32:00')),
  ((SELECT id_slot FROM slot_parkir WHERE nomor_slot='P11'), 'kosong', 'terisi', 'simulator', CONCAT(CURDATE(),' 09:47:00')),
  ((SELECT id_slot FROM slot_parkir WHERE nomor_slot='P07'), 'kosong', 'maintenance', 'manual', CONCAT(CURDATE(),' 10:15:00'));
```

### 4.3 `backend/config.php`

Konfigurasi koneksi database dan kunci perangkat sensor.

```php
<?php
// Konfigurasi dasar. Ubah sesuai lingkungan lokal (XAMPP/Laragon) atau isi lewat variabel lingkungan.
return [
    'db_host' => getenv('DB_HOST') ?: '127.0.0.1',
    'db_name' => getenv('DB_NAME') ?: 'digital_twin_parkir',
    'db_user' => getenv('DB_USER') ?: 'root',
    'db_pass' => getenv('DB_PASS') ?: '',
    // Kunci sederhana untuk perangkat sensor/simulator. Ganti sebelum dipakai sungguhan.
    'device_key' => getenv('DEVICE_KEY') ?: 'ganti-kunci-ini',
];
```

### 4.4 `backend/db.php`

Koneksi PDO, fungsi bantu JSON, dan fungsi `ubah_status_slot` (update slot + catat log dalam satu transaksi).

```php
<?php
// Koneksi PDO dan fungsi bantu untuk semua endpoint.
function db(): PDO {
    static $pdo = null;
    if ($pdo === null) {
        $c = require __DIR__ . '/config.php';
        $dsn = "mysql:host={$c['db_host']};dbname={$c['db_name']};charset=utf8mb4";
        $pdo = new PDO($dsn, $c['db_user'], $c['db_pass'], [
            PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
            PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
        ]);
    }
    return $pdo;
}

function json_out($data, int $code = 200): void {
    http_response_code($code);
    header('Content-Type: application/json; charset=utf-8');
    echo json_encode($data, JSON_UNESCAPED_UNICODE);
    exit;
}

function json_in(): array {
    $raw = file_get_contents('php://input');
    $data = json_decode($raw ?: '[]', true);
    return is_array($data) ? $data : [];
}

const STATUS_SLOT = ['kosong', 'terisi', 'maintenance'];

/** Mengubah status slot dan mencatat log dalam satu transaksi database. */
function ubah_status_slot(string $nomor, string $statusBaru, string $sumber): array {
    if (!in_array($statusBaru, STATUS_SLOT, true)) {
        return ['ok' => false, 'code' => 400, 'msg' => 'Status tidak valid.'];
    }
    $pdo = db();
    $pdo->beginTransaction();
    try {
        $st = $pdo->prepare('SELECT id_slot, status FROM slot_parkir WHERE nomor_slot = ? FOR UPDATE');
        $st->execute([$nomor]);
        $slot = $st->fetch();
        if (!$slot) { $pdo->rollBack(); return ['ok' => false, 'code' => 404, 'msg' => 'Slot tidak ditemukan.']; }
        if ($slot['status'] === $statusBaru) { $pdo->rollBack(); return ['ok' => true, 'code' => 200, 'msg' => 'Status tidak berubah.']; }

        $pdo->prepare('UPDATE slot_parkir SET status = ? WHERE id_slot = ?')->execute([$statusBaru, $slot['id_slot']]);
        $pdo->prepare('INSERT INTO log_status_slot (id_slot, status_lama, status_baru, sumber, waktu) VALUES (?, ?, ?, ?, NOW())')
            ->execute([$slot['id_slot'], $slot['status'], $statusBaru, $sumber]);
        $pdo->commit();
        return ['ok' => true, 'code' => 200, 'msg' => 'Status slot diperbarui.'];
    } catch (Throwable $e) {
        if ($pdo->inTransaction()) { $pdo->rollBack(); }
        return ['ok' => false, 'code' => 500, 'msg' => 'Kesalahan server.'];
    }
}
```

### 4.5 `backend/api/login.php`

Endpoint login admin.

```php
<?php
// POST /api/login.php  body: {"email": "...", "password": "..."}
require __DIR__ . '/../db.php';
if ($_SERVER['REQUEST_METHOD'] !== 'POST') { json_out(['error' => 'Gunakan metode POST.'], 405); }

$in = json_in();
$email = trim($in['email'] ?? '');
$pass = (string)($in['password'] ?? '');

$st = db()->prepare('SELECT id, nama, email, password, role FROM users WHERE email = ?');
$st->execute([$email]);
$user = $st->fetch();

if (!$user || !password_verify($pass, $user['password'])) {
    json_out(['error' => 'Email atau password salah.'], 401);
}
session_start();
$_SESSION['user'] = ['id' => (int)$user['id'], 'nama' => $user['nama'], 'role' => $user['role']];
json_out(['ok' => true, 'user' => $_SESSION['user']]);
```

### 4.6 `backend/api/slots.php`

Endpoint daftar slot (GET) dan ubah status manual (POST).

```php
<?php
// GET  /api/slots.php                        -> daftar slot + ringkasan (dipakai dashboard, polling)
// POST /api/slots.php  {"nomor_slot":"P05","status":"terisi"}  -> ubah status manual (perlu login)
require __DIR__ . '/../db.php';

if ($_SERVER['REQUEST_METHOD'] === 'GET') {
    $slots = db()->query(
        "SELECT s.nomor_slot, s.jenis, s.status, s.pos_x, s.pos_y, k.plat_nomor
           FROM slot_parkir s
           LEFT JOIN transaksi_parkir t ON t.id_slot = s.id_slot AND t.status = 'aktif'
           LEFT JOIN kendaraan k ON k.id_kendaraan = t.id_kendaraan
          ORDER BY s.pos_y, s.pos_x"
    )->fetchAll();
    $ringkas = ['total' => count($slots), 'terisi' => 0, 'kosong' => 0, 'maintenance' => 0];
    foreach ($slots as $s) { $ringkas[$s['status']]++; }
    $ringkas['okupansi'] = $ringkas['total'] ? (int)round($ringkas['terisi'] / $ringkas['total'] * 100) : 0;
    json_out(['ringkasan' => $ringkas, 'slots' => $slots, 'waktu' => date('c')]);
}

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    session_start();
    if (empty($_SESSION['user'])) { json_out(['error' => 'Silakan login terlebih dahulu.'], 401); }
    $in = json_in();
    $r = ubah_status_slot((string)($in['nomor_slot'] ?? ''), (string)($in['status'] ?? ''), 'manual');
    json_out($r['ok'] ? ['ok' => true, 'msg' => $r['msg']] : ['error' => $r['msg']], $r['code']);
}
json_out(['error' => 'Metode tidak didukung.'], 405);
```

### 4.7 `backend/api/sensor.php`

Endpoint penerima data sensor atau simulator.

```php
<?php
// POST /api/sensor.php   header: X-Device-Key: <device_key>
// body: {"nomor_slot":"P05","status":"terisi","device_id":"sim-01","timestamp":"2026-09-30T10:42:15+07:00"}
// Dipakai simulator (Sprint 1) dan nanti sensor/ESP32 pada miniatur.
require __DIR__ . '/../db.php';
if ($_SERVER['REQUEST_METHOD'] !== 'POST') { json_out(['error' => 'Gunakan metode POST.'], 405); }

$cfg = require __DIR__ . '/../config.php';
$key = $_SERVER['HTTP_X_DEVICE_KEY'] ?? '';
if (!hash_equals($cfg['device_key'], $key)) { json_out(['error' => 'Kunci perangkat tidak valid.'], 401); }

$in = json_in();
$device = (string)($in['device_id'] ?? '');
if ($device === '' || empty($in['nomor_slot']) || empty($in['status'])) {
    json_out(['error' => 'Data tidak lengkap: nomor_slot, status, device_id wajib diisi.'], 400);
}
$sumber = strpos($device, 'sim') === 0 ? 'simulator' : 'sensor';
$r = ubah_status_slot((string)$in['nomor_slot'], (string)$in['status'], $sumber);
json_out($r['ok'] ? ['ok' => true, 'msg' => $r['msg']] : ['error' => $r['msg']], $r['code']);
```

### 4.8 `frontend/index.html`

Halaman login.

```html
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Masuk | Parking Twin</title>
<link rel="stylesheet" href="css/style.css">
</head>
<body data-page="login">
<main class="login-page">
  <section class="login-card">
    <h1>Parking Twin</h1>
    <p>Masuk untuk memantau area parkir.</p>
    <form id="loginForm" novalidate>
      <label for="email">Email</label>
      <input class="input" id="email" type="email" autocomplete="username" value="admin@parkir.test" required>
      <label for="password">Password</label>
      <input class="input" id="password" type="password" autocomplete="current-password" required>
      <button class="btn btn-primary" type="submit">Masuk</button>
      <div class="msg err" id="loginError" role="alert"></div>
    </form>
    <p class="hint">Akun demo: admin@parkir.test / admin123</p>
  </section>
</main>
<script src="js/app.js"></script>
</body>
</html>
```

### 4.9 `frontend/dashboard.html`

Halaman dashboard.

```html
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Dashboard | Parking Twin</title>
<link rel="stylesheet" href="css/style.css">
</head>
<body data-page="dashboard">
<div class="app">
  <aside class="sidebar" id="sidebar"></aside>
  <main class="main">
    <header class="topbar">
      <div><h1>Dashboard</h1><p id="subtitle">Kondisi area parkir saat ini</p></div>
      <span class="user-chip" id="userChip"></span>
    </header>
    <section class="stats" aria-label="Ringkasan slot">
      <div class="stat"><div class="label">Total slot</div><div class="value" id="sTotal">0</div></div>
      <div class="stat busy"><div class="label">Terisi</div><div class="value" id="sTerisi">0</div></div>
      <div class="stat free"><div class="label">Kosong</div><div class="value" id="sKosong">0</div></div>
      <div class="stat"><div class="label">Okupansi</div><div class="value" id="sOkupansi">0%</div></div>
    </section>
    <div class="cols">
      <section>
        <h2 class="section-title">Denah ringkas</h2>
        <div class="lot"><div class="lot-grid" id="miniMap"></div></div>
      </section>
      <section class="panel">
        <h2>Aktivitas terakhir</h2>
        <ul class="activity" id="activity"></ul>
      </section>
    </div>
    <div class="btn-row">
      <button class="btn btn-primary" id="simBtn" type="button">Simulasikan perubahan</button>
      <a class="btn" href="denah.html" style="text-decoration:none">Lihat denah</a>
      <span class="hint">Tombol simulasi mengubah satu slot secara acak (pengganti sensor pada Sprint 1).</span>
    </div>
    <div class="msg" id="simMsg" role="status"></div>
  </main>
</div>
<script src="js/app.js"></script>
</body>
</html>
```

### 4.10 `frontend/denah.html`

Halaman denah parkir dan ubah status slot.

```html
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Denah Parkir | Parking Twin</title>
<link rel="stylesheet" href="css/style.css">
</head>
<body data-page="denah">
<div class="app">
  <aside class="sidebar" id="sidebar"></aside>
  <main class="main">
    <header class="topbar">
      <div><h1>Denah Parkir</h1><p>Klik slot untuk melihat detail dan mengubah status.</p></div>
      <span class="user-chip" id="userChip"></span>
    </header>
    <div class="legend">
      <span><i class="kosong"></i>Kosong</span><span><i class="terisi"></i>Terisi</span><span><i class="maintenance"></i>Maintenance</span>
    </div>
    <div class="cols">
      <div class="lot big" id="lot"></div>
      <div>
        <section class="panel detail" id="detail"></section>
        <div class="msg" id="detailMsg" role="status"></div>
        <section class="panel" style="margin-top:16px">
          <h2>Simulasi otomatis</h2>
          <p class="hint" style="margin-top:0">Mengubah satu slot secara acak tiap 4 detik, meniru data dari sensor miniatur.</p>
          <div class="btn-row" style="margin-top:12px">
            <button class="btn" id="autoBtn" type="button">Mulai simulasi</button>
            <button class="btn" id="resetBtn" type="button">Reset data</button>
          </div>
          <p class="hint" id="autoState" role="status"></p>
        </section>
      </div>
    </div>
  </main>
</div>
<script src="js/app.js"></script>
</body>
</html>
```

### 4.11 `frontend/kendaraan.html`

Halaman data kendaraan.

```html
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Kendaraan | Parking Twin</title>
<link rel="stylesheet" href="css/style.css">
</head>
<body data-page="kendaraan">
<div class="app">
  <aside class="sidebar" id="sidebar"></aside>
  <main class="main">
    <header class="topbar">
      <div><h1>Data Kendaraan</h1><p>Kendaraan terdaftar dan status parkirnya.</p></div>
      <span class="user-chip" id="userChip"></span>
    </header>
    <div class="filters">
      <div><label for="q">Cari plat nomor atau pemilik</label><input class="input" id="q" type="search" placeholder="Contoh: BL 1234"></div>
      <button class="btn btn-primary" type="button" disabled title="Tersedia di Sprint 2">+ Tambah kendaraan</button>
    </div>
    <div class="table-wrap">
      <table>
        <thead><tr><th>No</th><th>Plat Nomor</th><th>Jenis</th><th>Pemilik</th><th>Status</th></tr></thead>
        <tbody id="vehicleRows"></tbody>
      </table>
    </div>
  </main>
</div>
<script src="js/app.js"></script>
</body>
</html>
```

### 4.12 `frontend/riwayat.html`

Halaman riwayat parkir.

```html
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Riwayat Parkir | Parking Twin</title>
<link rel="stylesheet" href="css/style.css">
</head>
<body data-page="riwayat">
<div class="app">
  <aside class="sidebar" id="sidebar"></aside>
  <main class="main">
    <header class="topbar">
      <div><h1>Riwayat Parkir</h1><p>Catatan kendaraan masuk, keluar, dan perubahan status slot.</p></div>
      <span class="user-chip" id="userChip"></span>
    </header>
    <div class="filters">
      <div><label for="fq">Plat nomor</label><input class="input" id="fq" type="search" placeholder="Semua"></div>
      <div><label for="fs">Status</label>
        <select class="input" id="fs"><option value="">Semua</option><option>Masuk</option><option>Keluar</option><option>Maintenance</option><option>Selesai maintenance</option></select></div>
      <div><label for="fd">Tanggal</label><input class="input" id="fd" type="date"></div>
    </div>
    <div class="table-wrap">
      <table>
        <thead><tr><th>Waktu</th><th>Kendaraan</th><th>Slot</th><th>Status</th><th>Sumber</th></tr></thead>
        <tbody id="historyRows"></tbody>
      </table>
    </div>
  </main>
</div>
<script src="js/app.js"></script>
</body>
</html>
```

### 4.13 `frontend/css/style.css`

Gaya tampilan seluruh halaman.

```css
/* Parking Twin - gaya dasar. Palet: aspal gelap, marka jalan kuning, status hijau/merah/abu. */
:root {
  --asphalt: #2b3038;
  --ink: #1f2937;
  --paper: #f5f6f4;
  --line: #d5d9de;
  --muted: #6b7280;
  --marka: #f2b705;
  --free: #2f9e6b;   --free-bg: #d7f0e3;
  --busy: #c8433a;   --busy-bg: #f7d9d6;
  --maint: #6b7a8f;  --maint-bg: #dfe4ea;
}
* { box-sizing: border-box; }
html, body { margin: 0; height: 100%; }
body { font-family: "Segoe UI", system-ui, -apple-system, "Helvetica Neue", Arial, sans-serif; color: var(--ink); background: var(--paper); font-size: 15px; line-height: 1.45; }
h1, h2, h3 { margin: 0; line-height: 1.2; }
button, input, select { font: inherit; }
:focus-visible { outline: 3px solid var(--marka); outline-offset: 2px; }

/* Kerangka halaman */
.app { display: flex; min-height: 100%; }
.sidebar { width: 236px; flex: none; background: #e7e9ec; border-right: 1px solid var(--line); padding: 24px 14px; display: flex; flex-direction: column; }
.brand { padding: 0 10px 22px; }
.brand strong { display: block; font-size: 19px; }
.brand span { font-size: 12px; color: var(--muted); }
.nav a { display: block; padding: 11px 14px; margin-bottom: 6px; border-radius: 8px; color: var(--ink); text-decoration: none; border-left: 6px solid transparent; }
.nav a:hover { background: #f1f2f4; }
.nav a.active { background: #fff; font-weight: 700; border-left-color: var(--marka); }
.sidebar .logout { margin-top: auto; }
.main { flex: 1; padding: 28px 34px 40px; min-width: 0; }
.topbar { display: flex; justify-content: space-between; align-items: flex-start; gap: 16px; margin-bottom: 22px; flex-wrap: wrap; }
.topbar h1 { font-size: 26px; }
.topbar p { margin: 4px 0 0; color: var(--muted); font-size: 14px; }
.user-chip { border: 1px solid var(--line); background: #fff; border-radius: 8px; padding: 7px 14px; font-size: 13px; color: var(--muted); }

/* Tombol */
.btn { border: 1px solid var(--ink); background: #fff; color: var(--ink); padding: 10px 18px; border-radius: 8px; cursor: pointer; font-weight: 600; }
.btn:hover { background: #f1f2f4; }
.btn-primary { background: var(--ink); color: #fff; }
.btn-primary:hover { background: #111827; }
.btn:disabled { opacity: .5; cursor: not-allowed; }
.btn-row { display: flex; gap: 12px; flex-wrap: wrap; align-items: center; margin-top: 20px; }
.hint { color: var(--muted); font-size: 13px; }

/* Kartu statistik */
.stats { display: grid; grid-template-columns: repeat(auto-fit, minmax(190px, 1fr)); gap: 16px; margin-bottom: 26px; }
.stat { background: #fff; border: 1px solid var(--line); border-radius: 10px; padding: 16px 18px; }
.stat .label { color: var(--muted); font-size: 13px; }
.stat .value { font-size: 34px; font-weight: 700; font-variant-numeric: tabular-nums; }
.stat.busy .value { color: var(--busy); } .stat.free .value { color: var(--free); }

/* Panel dan tata letak dua kolom */
.cols { display: grid; grid-template-columns: minmax(0, 1.25fr) minmax(0, 1fr); gap: 24px; }
.panel { background: #fff; border: 1px solid var(--line); border-radius: 10px; padding: 18px 20px; }
.panel h2 { font-size: 16px; margin-bottom: 14px; }
.section-title { font-size: 16px; margin: 0 0 10px; }

/* Denah: aspal dengan marka putih */
.lot { background: var(--asphalt); border-radius: 10px; padding: 20px; }
.lot-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; }
.lot-label { color: var(--marka); font-size: 12px; font-weight: 700; letter-spacing: .04em; text-align: center; margin: 2px 0 12px; }
.lot-label.bottom { margin: 14px 0 0; }
.slot { border-radius: 6px; padding: 10px 6px; text-align: center; border: 2px solid; cursor: default; min-height: 84px; background: #fff; }
.lot.big .slot { min-height: 108px; padding-top: 16px; cursor: pointer; }
.slot .no { font-weight: 700; font-size: 15px; }
.slot .st { font-size: 12px; font-weight: 700; margin-top: 2px; }
.slot .plat { font-size: 12px; margin-top: 4px; min-height: 16px; }
.slot.kosong { background: var(--free-bg); border-color: var(--free); } .slot.kosong .st { color: var(--free); }
.slot.terisi { background: var(--busy-bg); border-color: var(--busy); } .slot.terisi .st { color: var(--busy); }
.slot.maintenance { background: var(--maint-bg); border-color: var(--maint); } .slot.maintenance .st { color: var(--maint); }
.slot.selected { outline: 3px dashed var(--marka); outline-offset: 3px; }
.slot:focus-visible { outline: 3px solid var(--marka); outline-offset: 3px; }
.legend { display: flex; gap: 22px; margin-bottom: 14px; flex-wrap: wrap; font-size: 14px; }
.legend i { display: inline-block; width: 16px; height: 16px; border-radius: 4px; border: 2px solid; vertical-align: -3px; margin-right: 8px; }
.legend .kosong { background: var(--free-bg); border-color: var(--free); }
.legend .terisi { background: var(--busy-bg); border-color: var(--busy); }
.legend .maintenance { background: var(--maint-bg); border-color: var(--maint); }

/* Daftar aktivitas */
.activity { list-style: none; margin: 0; padding: 0; }
.activity li { display: grid; grid-template-columns: 64px 1fr auto; gap: 10px; padding: 11px 0; border-bottom: 1px solid #eceef1; font-size: 14px; }
.activity li:last-child { border-bottom: 0; }
.activity time { font-weight: 700; font-variant-numeric: tabular-nums; }
.empty { color: var(--muted); padding: 12px 0; }
.tag { font-weight: 700; } .tag.masuk { color: var(--busy); } .tag.keluar { color: var(--free); } .tag.maintenance { color: var(--maint); }

/* Panel detail slot */
.detail .field { margin-bottom: 16px; }
.detail .field b { display: block; margin-bottom: 6px; font-size: 13px; }
.choice { display: flex; gap: 8px; flex-wrap: wrap; }
.choice label { border: 1.5px solid var(--line); border-radius: 8px; padding: 8px 12px; cursor: pointer; font-size: 13px; }
.choice input { margin-right: 6px; }
.choice label:has(input:checked) { border-color: var(--ink); background: #f1f2f4; font-weight: 700; }
.msg { min-height: 20px; font-size: 13px; margin-top: 10px; color: var(--free); }
.msg.err { color: var(--busy); }

/* Tabel */
.filters { display: flex; gap: 12px; flex-wrap: wrap; align-items: flex-end; margin-bottom: 18px; }
.filters label { display: block; font-size: 13px; font-weight: 700; margin-bottom: 4px; }
.input { border: 1px solid var(--line); background: #fff; border-radius: 8px; padding: 10px 12px; min-width: 200px; }
.table-wrap { overflow-x: auto; border: 1px solid var(--line); border-radius: 10px; background: #fff; }
table { width: 100%; border-collapse: collapse; font-size: 14px; }
th { background: var(--ink); color: #fff; text-align: left; padding: 12px 14px; font-weight: 600; }
td { padding: 12px 14px; border-top: 1px solid #eceef1; }
tbody tr:nth-child(even) { background: #fafafa; }

/* Halaman login */
.login-page { display: flex; align-items: center; justify-content: center; min-height: 100%; padding: 24px; }
.login-card { width: 100%; max-width: 420px; background: #fff; border: 1px solid var(--line); border-radius: 12px; padding: 34px 32px; }
.login-card h1 { font-size: 28px; }
.login-card p { color: var(--muted); margin: 6px 0 24px; }
.login-card label { display: block; font-weight: 700; font-size: 13px; margin: 14px 0 6px; }
.login-card .input { width: 100%; }
.login-card .btn { width: 100%; margin-top: 22px; padding: 12px; }
.login-card .hint { margin-top: 16px; text-align: center; }

@media (max-width: 900px) {
  .app { flex-direction: column; }
  .sidebar { width: 100%; flex-direction: row; flex-wrap: wrap; align-items: center; padding: 12px; gap: 6px; }
  .brand { padding: 0 10px 0 4px; } .brand span { display: none; }
  .nav { display: flex; flex-wrap: wrap; gap: 4px; } .nav a { margin: 0; padding: 8px 12px; }
  .sidebar .logout { margin: 0 0 0 auto; }
  .main { padding: 20px 16px 32px; }
  .cols { grid-template-columns: 1fr; }
}
@media (prefers-reduced-motion: reduce) { * { transition: none !important; animation: none !important; } }
```

### 4.14 `frontend/js/app.js`

Data simulasi, logika status slot, dan tampilan tiap halaman.

```javascript
/* Parking Twin - prototype frontend (Sprint 1)
 * Data masih disimulasikan di browser (localStorage). Nanti diganti pemanggilan API
 * ke backend (lihat backend/api dan README) tanpa mengubah tampilan.
 */
(function () {
  'use strict';

  var STATE_KEY = 'ptwin_state_v1';
  var AUTH_KEY = 'ptwin_auth';
  var DEMO = { email: 'admin@parkir.test', password: 'admin123' };
  var memState = null; // cadangan bila localStorage tidak tersedia

  /* ---------- Data awal ---------- */
  function seedState() {
    var jenis = function (i) { return i < 8 ? 'mobil' : 'motor'; };
    var slots = [];
    for (var i = 0; i < 12; i++) {
      slots.push({
        nomor: 'P' + (i < 9 ? '0' : '') + (i + 1),
        jenis: jenis(i), status: 'kosong', plat: '',
        x: (i % 4) + 1, y: Math.floor(i / 4) + 1
      });
    }
    var put = function (nomor, status, plat) {
      slots.forEach(function (s) { if (s.nomor === nomor) { s.status = status; s.plat = plat || ''; } });
    };
    put('P01', 'terisi', 'BL 1234 XX'); put('P04', 'terisi', 'BL 3456 XX');
    put('P10', 'terisi', 'BL 5678 XX'); put('P11', 'terisi', 'BL 7788 XX');
    put('P07', 'maintenance', '');

    var vehicles = [
      { plat: 'BL 1234 XX', jenis: 'Mobil', pemilik: 'Budi Santoso' },
      { plat: 'BL 3456 XX', jenis: 'Mobil', pemilik: 'Rahmat Hidayat' },
      { plat: 'BL 9012 XX', jenis: 'Mobil', pemilik: 'Andi Pratama' },
      { plat: 'BL 2468 XX', jenis: 'Mobil', pemilik: 'Maya Sari' },
      { plat: 'BL 5678 XX', jenis: 'Motor', pemilik: 'Siti Aisyah' },
      { plat: 'BL 7788 XX', jenis: 'Motor', pemilik: 'Dewi Lestari' },
      { plat: 'BL 1357 XX', jenis: 'Motor', pemilik: 'Fajar Nugraha' },
      { plat: 'BL 8642 XX', jenis: 'Motor', pemilik: 'Nurul Huda' }
    ];

    var at = function (h, m) { var d = new Date(); d.setHours(h, m, 0, 0); return d.toISOString(); };
    var log = [
      { waktu: at(10, 15), plat: '', slot: 'P07', status: 'Maintenance', sumber: 'manual' },
      { waktu: at(9, 47), plat: 'BL 7788 XX', slot: 'P11', status: 'Masuk', sumber: 'simulator' },
      { waktu: at(9, 32), plat: 'BL 2468 XX', slot: 'P02', status: 'Keluar', sumber: 'simulator' },
      { waktu: at(9, 5), plat: 'BL 2468 XX', slot: 'P02', status: 'Masuk', sumber: 'simulator' },
      { waktu: at(8, 41), plat: 'BL 5678 XX', slot: 'P10', status: 'Masuk', sumber: 'simulator' },
      { waktu: at(8, 20), plat: 'BL 3456 XX', slot: 'P04', status: 'Masuk', sumber: 'simulator' },
      { waktu: at(7, 58), plat: 'BL 1234 XX', slot: 'P01', status: 'Masuk', sumber: 'simulator' }
    ];
    return { slots: slots, vehicles: vehicles, log: log };
  }

  /* ---------- Penyimpanan ---------- */
  function loadState() {
    try {
      var raw = window.localStorage.getItem(STATE_KEY);
      if (raw) return JSON.parse(raw);
      var s = seedState(); saveState(s); return s;
    } catch (e) {
      if (!memState) memState = seedState();
      return memState;
    }
  }
  function saveState(s) {
    memState = s;
    try { window.localStorage.setItem(STATE_KEY, JSON.stringify(s)); } catch (e) { /* abaikan */ }
  }
  function resetState() { var s = seedState(); saveState(s); return s; }

  function isAuthed() {
    try { return window.sessionStorage.getItem(AUTH_KEY) === '1'; } catch (e) { return false; }
  }
  function setAuthed(v) {
    try { if (v) window.sessionStorage.setItem(AUTH_KEY, '1'); else window.sessionStorage.removeItem(AUTH_KEY); } catch (e) { /* abaikan */ }
  }

  /* ---------- Logika status slot ---------- */
  function findSlot(s, nomor) { return s.slots.filter(function (x) { return x.nomor === nomor; })[0]; }
  function parkedPlates(s) { return s.slots.filter(function (x) { return x.plat; }).map(function (x) { return x.plat; }); }
  function pickVehicle(s, jenisSlot) {
    var parked = parkedPlates(s);
    var pool = s.vehicles.filter(function (v) { return v.jenis.toLowerCase() === jenisSlot && parked.indexOf(v.plat) === -1; });
    return pool.length ? pool[Math.floor(Math.random() * pool.length)] : null;
  }

  /* Mengubah status satu slot. sumber: 'manual' | 'simulator' | 'sensor'. */
  function setStatus(nomor, status, sumber) {
    var s = loadState();
    var slot = findSlot(s, nomor);
    if (!slot) return { ok: false, msg: 'Slot ' + nomor + ' tidak ditemukan.' };
    if (slot.status === status) return { ok: false, msg: 'Slot ' + nomor + ' sudah berstatus ' + status + '.' };

    var lama = slot.status, plat = '', label;
    if (status === 'terisi') {
      var v = pickVehicle(s, slot.jenis);
      if (!v) return { ok: false, msg: 'Tidak ada kendaraan ' + slot.jenis + ' yang tersedia untuk slot ' + nomor + '.' };
      slot.plat = v.plat; plat = v.plat; label = 'Masuk';
    } else if (status === 'kosong') {
      plat = slot.plat; slot.plat = '';
      label = lama === 'maintenance' ? 'Selesai maintenance' : 'Keluar';
    } else {
      plat = slot.plat; slot.plat = ''; label = 'Maintenance';
    }
    slot.status = status;
    s.log.unshift({ waktu: new Date().toISOString(), plat: plat, slot: nomor, status: label, sumber: sumber || 'manual' });
    if (s.log.length > 200) s.log.length = 200;
    saveState(s);
    return { ok: true, msg: 'Slot ' + nomor + ': ' + lama + ' menjadi ' + status + (plat ? ' (' + plat + ')' : '') + '.' };
  }

  /* Satu langkah simulasi: mengubah satu slot secara acak. */
  function simulateOnce() {
    var s = loadState();
    for (var tries = 0; tries < 12; tries++) {
      var slot = s.slots[Math.floor(Math.random() * s.slots.length)];
      var next = slot.status === 'kosong' ? (Math.random() < 0.92 ? 'terisi' : 'maintenance') : 'kosong';
      var r = setStatus(slot.nomor, next, 'simulator');
      if (r.ok) return r;
      s = loadState();
    }
    return { ok: false, msg: 'Tidak ada perubahan yang bisa disimulasikan.' };
  }

  function stats(s) {
    var total = s.slots.length, terisi = 0, kosong = 0, maint = 0;
    s.slots.forEach(function (x) { if (x.status === 'terisi') terisi++; else if (x.status === 'kosong') kosong++; else maint++; });
    return { total: total, terisi: terisi, kosong: kosong, maintenance: maint, okupansi: total ? Math.round(terisi / total * 100) : 0 };
  }

  /* ---------- Util tampilan ---------- */
  function esc(t) { return String(t == null ? '' : t).replace(/[&<>"']/g, function (c) { return { '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' }[c]; }); }
  function pad(n) { return (n < 10 ? '0' : '') + n; }
  function hhmm(iso) { var d = new Date(iso); return pad(d.getHours()) + ':' + pad(d.getMinutes()); }
  function hhmmss(d) { return pad(d.getHours()) + ':' + pad(d.getMinutes()) + ':' + pad(d.getSeconds()); }
  function dateKey(d) { return d.getFullYear() + '-' + pad(d.getMonth() + 1) + '-' + pad(d.getDate()); }
  function $(id) { return document.getElementById(id); }
  function statusLabel(k) { return { kosong: 'Kosong', terisi: 'Terisi', maintenance: 'Maintenance' }[k] || k; }
  function tagClass(label) { return label === 'Masuk' ? 'masuk' : label === 'Keluar' ? 'keluar' : 'maintenance'; }

  function slotHTML(slot, big, selected) {
    var cls = 'slot ' + slot.status + (selected ? ' selected' : '');
    var attrs = big ? ' role="button" tabindex="0" data-slot="' + esc(slot.nomor) + '" aria-label="Slot ' + esc(slot.nomor) + ', ' + statusLabel(slot.status) + '"' : '';
    return '<div class="' + cls + '"' + attrs + '><div class="no">' + esc(slot.nomor) + '</div><div class="st">' +
      statusLabel(slot.status) + '</div><div class="plat">' + esc(slot.plat) + '</div></div>';
  }

  function buildShell(active) {
    var items = [['dashboard', 'dashboard.html', 'Dashboard'], ['denah', 'denah.html', 'Denah Parkir'],
                 ['kendaraan', 'kendaraan.html', 'Kendaraan'], ['riwayat', 'riwayat.html', 'Riwayat']];
    var nav = items.map(function (i) {
      return '<a href="' + i[1] + '"' + (i[0] === active ? ' class="active" aria-current="page"' : '') + '>' + i[2] + '</a>';
    }).join('');
    var side = $('sidebar');
    if (side) {
      side.innerHTML = '<div class="brand"><strong>Parking Twin</strong><span>Digital Twin Sistem Parkir</span></div>' +
        '<nav class="nav">' + nav + '</nav><div class="logout"><button class="btn" id="logoutBtn" type="button">Keluar</button></div>';
      $('logoutBtn').addEventListener('click', function () { setAuthed(false); location.href = 'index.html'; });
    }
    var chip = $('userChip'); if (chip) chip.textContent = DEMO.email;
  }

  /* ---------- Halaman ---------- */
  var pages = {};

  pages.login = function () {
    if (isAuthed()) { location.replace('dashboard.html'); return; }
    var form = $('loginForm'), err = $('loginError');
    form.addEventListener('submit', function (e) {
      e.preventDefault();
      var email = $('email').value.trim(), pass = $('password').value;
      if (email === DEMO.email && pass === DEMO.password) { setAuthed(true); location.href = 'dashboard.html'; }
      else { err.textContent = 'Email atau password salah. Periksa lagi lalu coba masuk kembali.'; err.className = 'msg err'; }
    });
  };

  pages.dashboard = function () {
    var msg = $('simMsg');
    function render() {
      var s = loadState(), st = stats(s);
      $('sTotal').textContent = st.total; $('sTerisi').textContent = st.terisi;
      $('sKosong').textContent = st.kosong; $('sOkupansi').textContent = st.okupansi + '%';
      $('subtitle').textContent = 'Kondisi area parkir saat ini, diperbarui ' + hhmmss(new Date()) +
        (st.maintenance ? '; ' + st.maintenance + ' slot maintenance' : '');
      $('miniMap').innerHTML = s.slots.map(function (x) { return slotHTML(x, false, false); }).join('');
      $('activity').innerHTML = s.log.length ? s.log.slice(0, 5).map(function (l) {
        return '<li><time>' + hhmm(l.waktu) + '</time><span>' + (l.plat ? esc(l.plat) + ', ' : '') + esc(l.slot) +
          '</span><span class="tag ' + tagClass(l.status) + '">' + esc(l.status) + '</span></li>';
      }).join('') : '<li class="empty">Belum ada aktivitas. Klik "Simulasikan perubahan" untuk mencoba.</li>';
    }
    $('simBtn').addEventListener('click', function () { var r = simulateOnce(); msg.textContent = r.msg; msg.className = 'msg' + (r.ok ? '' : ' err'); render(); });
    render(); setInterval(render, 2000);
  };

  pages.denah = function () {
    var selected = null, pending = {}, timer = null;
    var lot = $('lot'), msg = $('detailMsg');

    function renderMap() {
      var s = loadState();
      lot.innerHTML = '<div class="lot-label">MASUK &gt;&gt;&gt;</div><div class="lot-grid">' +
        s.slots.map(function (x) { return slotHTML(x, true, x.nomor === selected); }).join('') +
        '</div><div class="lot-label bottom">&lt;&lt;&lt; KELUAR</div>';
    }
    function renderDetail() {
      var s = loadState(), box = $('detail');
      if (!selected) { box.innerHTML = '<h2>Detail slot</h2><p class="empty">Pilih salah satu slot pada denah untuk melihat detail dan mengubah statusnya.</p>'; return; }
      var slot = findSlot(s, selected), want = pending[selected] || slot.status;
      var opts = ['kosong', 'terisi', 'maintenance'].map(function (k) {
        return '<label><input type="radio" name="newStatus" value="' + k + '"' + (k === want ? ' checked' : '') + '>' + statusLabel(k) + '</label>';
      }).join('');
      box.innerHTML = '<h2>Slot ' + esc(slot.nomor) + '</h2><p class="hint">Jenis: ' + esc(slot.jenis) + '; posisi baris ' + slot.y + ', kolom ' + slot.x + '</p>' +
        '<div class="field"><b>Status saat ini</b><span id="curStatus">' + statusLabel(slot.status) + (slot.plat ? ' (' + esc(slot.plat) + ')' : '') + '</span></div>' +
        '<div class="field"><b>Ubah status menjadi</b><div class="choice">' + opts + '</div></div>' +
        '<button class="btn btn-primary" id="saveBtn" type="button">Simpan status</button>' +
        '<p class="hint" style="margin-top:12px">Perubahan tercatat di log dengan sumber "manual".</p>';
      box.querySelectorAll('input[name=newStatus]').forEach(function (r) { r.addEventListener('change', function () { pending[selected] = r.value; }); });
      $('saveBtn').addEventListener('click', function () {
        var v = box.querySelector('input[name=newStatus]:checked').value;
        var res = setStatus(selected, v, 'manual');
        msg.textContent = res.msg; msg.className = 'msg' + (res.ok ? '' : ' err');
        delete pending[selected]; renderMap(); renderDetail();
      });
    }
    function refreshCurrent() {
      if (!selected) return; var el = $('curStatus'); if (!el) return;
      var slot = findSlot(loadState(), selected);
      el.textContent = statusLabel(slot.status) + (slot.plat ? ' (' + slot.plat + ')' : '');
    }
    function pick(nomor) { selected = nomor; msg.textContent = ''; renderMap(); renderDetail(); }
    lot.addEventListener('click', function (e) { var t = e.target.closest('[data-slot]'); if (t) pick(t.getAttribute('data-slot')); });
    lot.addEventListener('keydown', function (e) {
      if (e.key !== 'Enter' && e.key !== ' ') return;
      var t = e.target.closest('[data-slot]'); if (t) { e.preventDefault(); pick(t.getAttribute('data-slot')); var again = lot.querySelector('[data-slot="' + selected + '"]'); if (again) again.focus(); }
    });
    var simBtn = $('autoBtn');
    simBtn.addEventListener('click', function () {
      if (timer) { clearInterval(timer); timer = null; simBtn.textContent = 'Mulai simulasi'; $('autoState').textContent = 'Simulasi berhenti.'; }
      else { timer = setInterval(function () { var r = simulateOnce(); $('autoState').textContent = r.msg; renderMap(); refreshCurrent(); }, 4000); simBtn.textContent = 'Hentikan simulasi'; $('autoState').textContent = 'Simulasi berjalan: satu slot berubah tiap 4 detik.'; }
    });
    $('resetBtn').addEventListener('click', function () { resetState(); selected = null; pending = {}; msg.textContent = ''; renderMap(); renderDetail(); $('autoState').textContent = 'Data dikembalikan ke kondisi awal.'; });
    renderMap(); renderDetail();
    setInterval(function () { renderMap(); refreshCurrent(); }, 2000);
  };

  pages.kendaraan = function () {
    var q = $('q');
    function render() {
      var s = loadState(), term = q.value.trim().toLowerCase();
      var rows = s.vehicles.filter(function (v) { return !term || v.plat.toLowerCase().indexOf(term) !== -1 || v.pemilik.toLowerCase().indexOf(term) !== -1; });
      $('vehicleRows').innerHTML = rows.length ? rows.map(function (v, i) {
        var slot = s.slots.filter(function (x) { return x.plat === v.plat; })[0];
        return '<tr><td>' + (i + 1) + '</td><td>' + esc(v.plat) + '</td><td>' + esc(v.jenis) + '</td><td>' + esc(v.pemilik) + '</td><td>' +
          (slot ? '<span class="tag masuk">Parkir (' + esc(slot.nomor) + ')</span>' : '<span class="tag keluar">Keluar</span>') + '</td></tr>';
      }).join('') : '<tr><td colspan="5" class="empty">Tidak ada kendaraan yang cocok dengan pencarian.</td></tr>';
    }
    q.addEventListener('input', render); render(); setInterval(render, 2000);
  };

  pages.riwayat = function () {
    var fq = $('fq'), fs = $('fs'), fd = $('fd');
    function render() {
      var s = loadState(), term = fq.value.trim().toLowerCase();
      var rows = s.log.filter(function (l) {
        if (term && (l.plat || '').toLowerCase().indexOf(term) === -1) return false;
        if (fs.value && l.status !== fs.value) return false;
        if (fd.value && dateKey(new Date(l.waktu)) !== fd.value) return false;
        return true;
      });
      $('historyRows').innerHTML = rows.length ? rows.map(function (l) {
        return '<tr><td>' + hhmm(l.waktu) + '</td><td>' + (l.plat ? esc(l.plat) : '-') + '</td><td>' + esc(l.slot) +
          '</td><td><span class="tag ' + tagClass(l.status) + '">' + esc(l.status) + '</span></td><td>' + esc(l.sumber || 'manual') + '</td></tr>';
      }).join('') : '<tr><td colspan="5" class="empty">Tidak ada catatan untuk filter ini.</td></tr>';
    }
    [fq, fs, fd].forEach(function (el) { el.addEventListener('input', render); el.addEventListener('change', render); });
    render(); setInterval(render, 2000);
  };

  /* ---------- Mulai ---------- */
  document.addEventListener('DOMContentLoaded', function () {
    var page = document.body.getAttribute('data-page');
    if (page !== 'login' && !isAuthed()) { location.replace('index.html'); return; }
    if (page !== 'login') buildShell(page);
    if (pages[page]) pages[page]();
  });

  window.PT = { loadState: loadState, resetState: resetState, setStatus: setStatus, simulateOnce: simulateOnce, stats: stats, seedState: seedState };
})();
```

## 5. Langkah unggah ke GitHub

```bash
git init
git add README.md && git commit -m "Initial project setup"
git add docs && git commit -m "Add design documents and function point"
git add database && git commit -m "Add database structure"
git add frontend backend && git commit -m "Add frontend prototype and backend skeleton"
git branch -M main
git remote add origin https://github.com/<username>/digital-twin-parkir.git
git push -u origin main
```
