# 11 — Commands
Satu-satunya rumah perintah. ⚠️ = mengubah data/irreversibel → baca `docs/22_CHANGE_POLICY.md`.

| Tujuan | Perintah |
|---|---|
| Dev server (serve + queue + logs + vite) | `composer run dev` |
| Server saja | `php artisan serve` |
| Vite dev | `npm run dev` |
| Build aset produksi | `npm run build` |
| Tes semua | `php artisan test` |
| Tes terfilter | `php artisan test --filter=ParticipantImportTest` |
| Tes paralel | `php artisan test --parallel` |
| Tes terhadap MySQL | `DB_CONNECTION=mysql DB_DATABASE=mycorpu_test php artisan test` |
| Cek format | `vendor/bin/pint --test` |
| Perbaiki format | `vendor/bin/pint` |
| Migrasi ⚠️ | `php artisan migrate` |
| Migrasi produksi ⚠️ | `php artisan migrate --force` |
| Lihat status migrasi | `php artisan migrate:status` |
| Reset DB lokal + seed ⚠️ (LOKAL SAJA) | `php artisan migrate:fresh --seed` |
| Seed admin | `php artisan db:seed --class=AdminSeeder` |
| Seed data demo (lokal) | `php artisan db:seed --class=DemoSeeder` |
| Buat migrasi | `php artisan make:migration create_xxx_table` |
| Buat komponen Livewire | `php artisan make:livewire Admin/Trainings/Index` |
| Buat Policy | `php artisan make:policy TrainingPolicy --model=Training` |
| Buat tes Pest | `php artisan make:test Feature/XxxTest --pest` |
| Worker queue | `php artisan queue:work --tries=3 --timeout=120` |
| Restart worker (setelah deploy) | `php artisan queue:restart` |
| Job gagal | `php artisan queue:failed` · ulangi: `php artisan queue:retry all` |
| Scheduler (cron, tiap menit) | `php artisan schedule:run` |
| Finalisasi tes kedaluwarsa (manual) | `php artisan tests:finalize-expired` |
| Log real-time | `php artisan pail` |
| Cache konfigurasi produksi | `php artisan optimize` · bersihkan: `php artisan optimize:clear` |
| Mode pemeliharaan ⚠️ | `php artisan down --secret=<token>` · `php artisan up` |
| Backup DB ⚠️ (produksi) | `mysqldump --single-transaction -u $DB_USERNAME -p $DB_DATABASE \| gzip > ~/backup/mycorpu-$(date +%F-%H%M).sql.gz` |
| Deploy (produksi) | Ikuti `docs/25_RELEASE_CHECKLIST.md` berurutan |
