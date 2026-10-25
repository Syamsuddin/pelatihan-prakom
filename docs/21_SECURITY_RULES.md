# 21 — Security Rules
Pemilik aturan keamanan. Peran: `docs/05_USER_ROLE.md`. Data sensitif: `docs/07_DATA_MODEL.md`.

## Autentikasi
- Login: `nip` + password; `Auth::attempt(['nip'=>…, 'password'=>…, 'is_active'=>true])`. Pesan gagal generik: "NIP atau password salah."
- Throttle: 5 percobaan/menit per kombinasi `nip`+IP (`RateLimiter`), lalu kunci 60 detik.
- `session()->regenerate()` setelah login; logout = invalidate + regenerate token.
- Middleware `EnsurePasswordChanged`: user `must_change_password=true` hanya boleh mengakses halaman ganti password & logout.
- Password baru: min. 8 karakter, mengandung huruf dan angka, **≠ NIP**, ≠ password lama. Reset oleh admin → password = NIP + `must_change_password=true`.
- Cookie sesi: `secure` (produksi HTTPS), `http_only`, `same_site=lax`. Session lifetime 120 menit.

## Otorisasi
- Middleware `EnsureRole` per grup route (`/admin`, `/narasumber`, `/peserta`) **dan** Policy per model. Keduanya wajib; middleware saja tidak cukup.
- Policy berbasis kepemilikan: narasumber → `subject.speaker_id === user.id`; peserta → punya `enrollment` di pelatihan tersebut, dan untuk data pribadi `enrollment.user_id === user.id`.
- Setiap method aksi Livewire memanggil `$this->authorize(...)`. Jangan hanya di `mount()`.
- Pelatihan `closed` → Policy `update`/`delete` menolak semua entitas turunannya (`docs/07_DATA_MODEL.md` aturan integritas).

## Validasi input
- Semua input lewat rules Livewire / FormRequest; tipe, panjang, `exists:` untuk FK, `in:` untuk enum.
- ID dari klien di Livewire: `#[Locked]` + query terbatas scope pengguna (mis. `$training->enrollments()->findOrFail($id)`), bukan `Model::find($id)` mentah.
- Mass assignment: `$fillable` eksplisit per model; `role`, `is_active`, `must_change_password` tidak boleh diisi dari form non-admin.

## Berkas
- Disk `local` (privat). Nama berkas disimpan = ULID + ekstensi, bukan nama asli.
- Validasi: `file|max:20480|mimes:pdf,docx,pptx,xlsx,zip` (+ cek `mimetypes`). Impor: `mimes:xlsx`.
- Unduh hanya lewat `FileDownloadController` → `authorize` → `Storage::download()` dengan header `Content-Disposition: attachment`.

## Data sensitif & privasi
- Halaman verifikasi publik hanya menampilkan nama, NIP tersamar (6 digit awal + `******` + 4 digit akhir), judul pelatihan, total JP, tanggal, status. Tanpa email/HP/unit.
- `verification_code` = ULID (tidak bisa ditebak dari nomor sertifikat). Route verifikasi di-throttle 30/menit per IP.
- Jawaban benar (`is_correct`) tidak pernah dikirim ke klien peserta. Hasil tes peserta hanya berupa nilai, tanpa kunci jawaban.
- Ekspor laporan (memuat PII) hanya untuk admin & narasumber (materinya).

## Lain-lain
- Output Blade selalu `{{ }}`. `{!! !!}` hanya untuk SVG ikon/QR yang dihasilkan server.
- Query mentah wajib binding parameter. Dilarang menggabung string SQL.
- Produksi: HTTPS wajib (`URL::forceHttps()` bila di balik proxy), `APP_DEBUG=false`, `.env` permission 600.
- Kepatuhan: [ASUMSI] tidak ada regulasi khusus selain praktik baik pelindungan data pribadi (UU PDP) di atas.
