# 25 — Release Checklist
Berurutan; berhenti di langkah pertama yang gagal. Perintah: `docs/11_COMMANDS.md`. Gerbang & rollback: `docs/22_CHANGE_POLICY.md`.

| # | Langkah | Lulus bila |
|---|---|---|
| 1 | Di branch rilis: `php artisan test` + `vendor/bin/pint --test` | 0 gagal, pint bersih |
| 2 | Suite terhadap MySQL (perintah "Tes terhadap MySQL") | 0 gagal |
| 3 | `php artisan migrate --pretend` pada salinan DB produksi; tinjau SQL | Tidak ada DROP/ALTER destruktif tanpa persetujuan tertulis |
| 4 | Merge ke `main`, buat tag `vX.Y.Z` | Tag ada di remote |
| 5 | Server: `php artisan down --secret=<token>` | Halaman 503 tampil, admin bisa masuk via secret |
| 6 | Backup DB (perintah "Backup DB") + salin `storage/app/private` | Berkas `.sql.gz` > 0 byte & bisa dibaca `zcat \| head` |
| 7 | `git fetch --tags && git checkout vX.Y.Z` → `composer install --no-dev --optimize-autoloader` → `npm ci && npm run build` | Tanpa error |
| 8 | `php artisan migrate --force` | Selesai tanpa error; `migrate:status` semua `Ran` |
| 9 | `php artisan optimize` → `php artisan queue:restart` → `sudo supervisorctl status` | Worker `RUNNING` |
| 10 | `php artisan up` | Situs normal |
| 11 | Smoke test: login admin, buka satu pelatihan, unduh satu bahan, buka satu `/verifikasi/{code}`, `php artisan queue:failed` | Semua tampil tanpa error; tidak ada job gagal baru |
| 12 | Cek `storage/logs/laravel-*.log` 10 menit setelah rilis | Tidak ada `ERROR` baru |

## Rollback (bila langkah 8–12 gagal)
1. `php artisan down`
2. Bila migrasi sudah berjalan: `php artisan migrate:rollback --step=<n>` (hanya bila `down()` aman), atau pulihkan backup langkah 6.
3. `git checkout <tag-sebelumnya>` → ulangi langkah 7, 9, 10.
4. Catat insiden + sebab di PR/issue sebelum mencoba rilis ulang.

## Prasyarat server (sekali)
Nginx `client_max_body_size 25m`; PHP `upload_max_filesize=25M`, `post_max_size=25M`; Supervisor program `mycorpu-worker` (`php artisan queue:work --tries=3 --timeout=120`); cron `* * * * * php /var/www/mycorpu/artisan schedule:run`; HTTPS (Let's Encrypt).
