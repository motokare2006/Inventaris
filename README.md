# Inventory Laravel

Aplikasi inventory IT berbasis Laravel untuk mengelola login admin, dashboard, data PT, department, karyawan, device, serta lisensi Windows dan Office.

## Login Awal

- Email: `admin@example.com`
- Password: `admin123`

Ganti password admin setelah berhasil login atau ubah langsung melalui database.

## Kebutuhan Server

- PHP 8.3 atau lebih baru.
- Composer.
- MySQL atau MariaDB.
- Ekstensi PHP umum Laravel: `pdo_mysql`, `mbstring`, `openssl`, `tokenizer`, `xml`, `ctype`, `json`, `fileinfo`.

## Menjalankan di Localhost Laragon

1. Buat database MySQL bernama `u1489632_ithw`.
2. Salin `.env.example` menjadi `.env`.
3. Sesuaikan koneksi database di `.env`. Untuk Laragon/XAMPP default, pakai user `root` tanpa password.

```env
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=u1489632_ithw
DB_USERNAME=root
DB_PASSWORD=

SESSION_DRIVER=database
CACHE_STORE=database
QUEUE_CONNECTION=database
```

4. Jalankan perintah setup.

```bash
composer install
php artisan key:generate
php artisan migrate --seed
php artisan serve
```

Buka `http://localhost:8000`.

Jika MySQL lokal memakai password, isi `DB_PASSWORD=password_mysql_anda`. Jika tanpa password, biarkan kosong seperti contoh di atas.

## Import SQL Lengkap

File SQL lengkap ada di `database/sql/u1489632_ithw_full.sql`. File ini berisi:

- `CREATE DATABASE u1489632_ithw`
- tabel Laravel auth/session/cache/queue
- tabel inventory `pt`, `department`, `device`, `karyawan`, `license_windows`, `license_office`
- foreign key antar tabel terkait
- view legacy/report
- data awal dan user login admin

Import via phpMyAdmin atau jalankan lewat terminal MySQL:

```bash
mysql -u root -p < database/sql/u1489632_ithw_full.sql
```

Untuk root tanpa password, cukup tekan Enter saat diminta password.

## Jika Database Lama Sudah Ada

Project ini sudah punya migration yang menyesuaikan tabel legacy `pt`, `department`, `device`, `karyawan`, `license_windows`, dan `license_office`. Jika database lama sudah berisi data, pakai:

```bash
php artisan migrate
php artisan db:seed
```

Jangan pakai `migrate:fresh` di database produksi/lama karena akan menghapus tabel.

## Upload ke Hosting

1. Upload semua file project ke hosting.
2. Arahkan document root domain/subdomain ke folder `public`.
3. Jika hosting tidak bisa mengubah document root, file `.htaccess` di root project akan meneruskan request ke `public`.
4. Salin `.env.production.example` menjadi `.env`.
5. Isi `APP_URL`, `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, dan `DB_PASSWORD` sesuai hosting. Banyak hosting memakai `DB_HOST=localhost`, tetapi username/password biasanya bukan `root`.
6. Jalankan command berikut via terminal hosting.

```bash
composer install --no-dev --optimize-autoloader
php artisan key:generate
php artisan migrate --seed --force
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

Pastikan permission folder `storage` dan `bootstrap/cache` bisa ditulis oleh web server.

## Fitur

- Auth login/logout memakai Laravel session.
- Session, cache, dan queue siap memakai database.
- Seeder admin default.
- CRUD PT dan Department, termasuk kode department.
- CRUD Device dengan proteksi hapus jika masih dipakai karyawan.
- CRUD Karyawan dengan validasi PT, department, dan assignment device.
- Search dan pagination.
- Migration kompatibel untuk MySQL local dan hosting.

## Test

```bash
php artisan test
```
