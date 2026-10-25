# 16 — Debugging Guide
Log: `docs/15_OBSERVABILITY.md`. Perintah: `docs/11_COMMANDS.md`. Pola error: `docs/14_ERROR_HANDLING.md`.

## Gejala → diagnosa
| Gejala | Langkah |
|---|---|
| Impor "macet" di status queued | Worker tidak jalan → `php artisan queue:work` lokal / `supervisorctl status` produksi; cek `queue:failed` |
| Sertifikat tidak terbit setelah tutup | `queue:failed` → lihat exception; render ulang lewat tombol Terbitkan ulang |
| Perubahan kode tak berefek di job | Worker memakai kode lama → `php artisan queue:restart` |
| Unggah berkas gagal tanpa pesan jelas | Lihat jebakan #1 |
| Tampilan Tailwind tidak berubah | `npm run dev` tidak jalan / belum `npm run build`; kelas dinamis tidak terdeteksi (jebakan #7) |
| Jam sesi bergeser ±8 jam | Zona waktu `config/app.php` bukan `Asia/Makassar` |
| Tes hijau lokal, error di MySQL | Lihat jebakan #6 |

## Jebakan proyek (baca sebelum menyentuh area terkait)
1. **Batas unggah berlapis.** Livewire memakai unggah sementara dengan batas default 12 MB (`config/livewire.php` → `temporary_file_upload.rules`). Agar 20 MB lolos, set `max:20480` di sana **dan** `upload_max_filesize`/`post_max_size` PHP ≥ 25M **dan** Nginx `client_max_body_size 25m`.
2. **NIP rusak oleh Excel.** NIP 18 digit terbaca sebagai float (notasi ilmiah, digit hilang). `ParticipantsImport` wajib membaca kolom NIP sebagai string (`WithCustomValueBinder` / `getFormattedValue`), dan template impor memformat kolom NIP sebagai Text.
3. **Properti publik Livewire terlihat & bisa diubah klien.** Jangan menaruh model `Question` beserta opsi `is_correct` di properti publik komponen tes. Tandai ID dengan `#[Locked]` dan authorize ulang di setiap method aksi. Validasi sisi klien tidak berarti apa-apa.
4. **dompdf ≠ browser.** Tanpa flexbox/grid (pakai `<table>`), font harus lokal (`storage/fonts`, daftarkan Inter), gambar/QR disisipkan sebagai data URI base64. CSS Tailwind tidak dimuat. Sertifikat memakai CSS inline yang nilainya diambil dari token `docs/26_UI_CONVENTIONS.md`.
5. **Nama tabel `sessions` sudah dipakai** oleh session driver Laravel. Sesi pelatihan = `training_sessions` / model `TrainingSession`.
6. **SQLite (tes) vs MySQL (produksi).** Perbedaan perbandingan string case-insensitive, `json` query, dan pembulatan decimal. Fitur yang menyentuh hal ini → jalankan suite terhadap MySQL sebelum merge.
7. **Kelas Tailwind dinamis tidak terdeteksi.** `"bg-{$color}"` tidak akan di-generate. Pakai peta kelas lengkap (mis. `match($status)` → string kelas utuh) di komponen `x-ui.badge`.
8. **Nomor sertifikat ganda saat job paralel.** `{seq}` diambil dengan `lockForUpdate` di dalam transaksi + constraint UNIQUE sebagai jaring terakhir. Jangan menghitung `count()+1` tanpa kunci.
