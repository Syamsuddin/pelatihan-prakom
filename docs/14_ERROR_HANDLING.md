# 14 — Error Handling
Pemilik pola penanganan error.

| Kelas | Contoh | Tampil ke pengguna | Dicatat |
|---|---|---|---|
| Validasi input | `ValidationException` | Pesan di bawah field (Bahasa Indonesia, `lang/id/validation.php`) | Tidak |
| Aturan bisnis | `App\Exceptions\BusinessRuleException` ("Total JP sesi (4) belum sama dengan JP materi (6)") | Toast **bahaya** berisi pesan exception | `info` |
| Otorisasi | `AuthorizationException` | Halaman 403 "Anda tidak memiliki akses ke halaman ini." | `warning` + user_id, route |
| Tidak ditemukan | `ModelNotFoundException` | Halaman 404 | Tidak |
| Impor per baris | NIP tidak valid | Tabel ringkasan baris gagal (`imports.errors`) | Tidak (sudah di DB) |
| Job gagal | Render PDF gagal | Status "Gagal diterbitkan" + tombol Terbitkan ulang di daftar sertifikat | `error` + exception |
| Tak terduga | `QueryException`, dll. | "Terjadi kesalahan. Silakan coba lagi atau hubungi admin." | `error` + exception + konteks |
| Kedaluwarsa sesi | `TokenMismatchException` / 419 | "Sesi Anda berakhir, silakan muat ulang halaman." | Tidak |

## Aturan
- Action melempar `BusinessRuleException` dengan pesan siap-tampil. Livewire menangkapnya di satu tempat (trait `App\Livewire\Concerns\HandlesBusinessErrors`) → dispatch event `toast`.
- Pesan pengguna: kalimat Indonesia lengkap, tanpa istilah teknis, nama tabel, atau ID internal.
- `APP_DEBUG=false` di produksi. Stack trace **tidak pernah** tampil ke pengguna.
- Halaman error kustom: `resources/views/errors/{403,404,419,500,503}.blade.php` memakai layout guest + token `docs/26_UI_CONVENTIONS.md`.
- Jangan menelan exception (`catch` kosong). Tangkap hanya untuk diterjemahkan atau dicatat ulang.
