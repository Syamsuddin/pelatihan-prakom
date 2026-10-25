# CLAUDE.md

File ini selalu aktif. Untuk hal di luar ini → buka INDEX.md.

## Proyek
MyCORPU — manajemen pelatihan multi-angkatan: jadwal berbasis JP, presensi per sesi, materi & tugas, pre/post-test, sertifikat ber-QR, laporan. Detail: docs/00_EXECUTIVE_SUMMARY.md. Scope: docs/02_SCOPE.md.

## Prinsip kerja agen (non-negotiable)
1. Friksi sebanding irreversibilitas.
2. Lapisan deterministik (validasi, constraint, tes) di bawah penalaran.
3. Human-in-the-loop untuk high-stakes (migrasi DB, hapus data, keamanan, rilis).
4. Jangan refactor keputusan yang disengaja diam-diam.
5. Hormati scope (docs/02). Out-of-scope → berhenti & tanya.

## Stack  (sumber: docs/09)
PHP 8.3 · Laravel 12 · Livewire 3 (class-based) · Tailwind 4 · MySQL 8 · Pest — Terlarang: jQuery, Bootstrap, SPA/Inertia, Filament/Nova, Volt/Flux, spatie/permission.

## Struktur & konvensi  (sumber: docs/12)
- Livewire/Controller tipis → aturan bisnis di `app/Actions` & `app/Services`; otorisasi hanya di `app/Policies`.
- Rumus kelulusan HANYA di `App\Services\PassingCalculator`.
- Model/kode Inggris, teks UI & URL Bahasa Indonesia. UI hanya token & komponen `x-ui.*` docs/26.

## Perintah penting  (sumber: docs/11)
dev: `composer run dev` · test: `php artisan test` · format: `vendor/bin/pint` · migrasi ⚠️: `php artisan migrate`

## Guardrail inti  (penuh: docs/20, 21, 22)
- Jangan hard-code rahasia; jangan longgarkan validasi/Policy/throttle.
- Data pelatihan `closed` terkunci; jangan hard-delete sertifikat/presensi/nilai.
- Berkas hanya di disk privat via route ber-Policy; `is_correct` tak pernah ke klien.
- Setiap aksi Livewire memanggil `authorize`; ID dari klien `#[Locked]` + query ber-scope.
- Tutup pelatihan, migrasi destruktif, cabut sertifikat, rilis = gerbang manusia.
- Tanpa dependensi baru & tanpa commit ke `main` tanpa persetujuan.

## Alur per-task  (penuh: docs/17, 19)
Baca INDEX.md → muat dokumen relevan → konfirmasi scope → vertical slice → tes (docs/13) → DoD (docs/24).

## Definisi selesai (ringkas; penuh: docs/24)
Kriteria docs/23 terpenuhi + `php artisan test` hijau + `pint --test` bersih + empat state UI + dokumen pemilik diperbarui.

## Untuk apa pun di luar ini → INDEX.md
