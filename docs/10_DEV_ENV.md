# 10 — Dev Environment
Perintah aktual: `docs/11_COMMANDS.md`. Versi: `docs/09_STACK.md`.

## Prasyarat
PHP 8.3 (ekstensi: pdo_mysql, pdo_sqlite, mbstring, gd, zip, intl, xml, fileinfo), Composer 2, Node 22 + npm, MySQL 8 (lokal boleh SQLite file).

## Setup lokal (pertama kali)
1. `git clone` → `composer install` → `npm install`
2. `cp .env.example .env` → `php artisan key:generate`
3. Isi variabel DB, lalu jalankan migrasi + seeder (perintah di `docs/11_COMMANDS.md`)
4. `php artisan storage:link` tidak diperlukan (semua berkas privat)
5. Jalankan server dev (`composer run dev`) → buka `http://localhost:8000`. Login admin seed: NIP `000000000000000001`, password awal sama dengan NIP (wajib diganti)

## Variabel `.env` (nama saja)
| Variabel | Catatan |
|---|---|
| `APP_NAME` | `MyCORPU` |
| `APP_ENV`, `APP_DEBUG`, `APP_URL`, `APP_KEY` | `APP_DEBUG=false` di produksi |
| `APP_LOCALE`, `APP_FALLBACK_LOCALE` | `id` |
| `DB_CONNECTION`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD` | `mysql` |
| `SESSION_DRIVER`, `QUEUE_CONNECTION`, `CACHE_STORE` | `database` ketiganya |
| `FILESYSTEM_DISK` | `local` |
| `LOG_CHANNEL`, `LOG_STACK`, `LOG_LEVEL` | `stack` / `daily` / `info` (produksi) |
| `SEED_ADMIN_NIP`, `SEED_ADMIN_NAME` | Dibaca `Database\Seeders\AdminSeeder` |

Zona waktu: set `'timezone' => 'Asia/Makassar'` di `config/app.php`. Laravel 12 tidak membaca variabel env untuk ini secara default.

## Tes
`phpunit.xml` memakai SQLite in-memory (`DB_CONNECTION=sqlite`, `DB_DATABASE=:memory:`). Sebelum rilis, jalankan ulang suite terhadap MySQL (lihat `docs/16_DEBUGGING_GUIDE.md` jebakan #6).
