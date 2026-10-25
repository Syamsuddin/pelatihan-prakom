# 20 — Guardrails
Larangan operasional keras. Aturan keamanan rinci: `docs/21_SECURITY_RULES.md`.

**JANGAN:**
1. Menulis rahasia (password, `APP_KEY`, kredensial DB) di kode, seeder, tes, atau commit. Semua lewat `.env`.
2. Menonaktifkan/melonggarkan validasi, Policy, middleware `auth`/`EnsurePasswordChanged`, CSRF, atau throttle, termasuk "sementara".
3. Menulis rumus kelulusan di luar `App\Services\PassingCalculator`.
4. Mengubah data pelatihan berstatus `closed` (kecuali kolom `revoked_*`/`path` sertifikat).
5. Hard-delete sertifikat, presensi, nilai, atau user yang punya riwayat. Pakai soft delete / pencabutan.
6. Menjalankan `migrate:fresh`, `db:wipe`, atau `migrate:rollback` di luar lingkungan lokal.
7. Menambah dependensi Composer/npm tanpa alasan tertulis & persetujuan (`docs/22_CHANGE_POLICY.md`). Daftar terlarang: `docs/09_STACK.md`.
8. Menyimpan berkas unggahan di disk `public` atau menyajikannya tanpa route ber-Policy.
9. Mengirim `question_options.is_correct` ke browser peserta.
10. Menulis nilai warna/ukuran/font di luar token `docs/26_UI_CONVENTIONS.md`.
11. Menghapus, melewati (`->skip()`), atau melemahkan tes agar suite hijau.
12. Mengerjakan fitur di luar `docs/02_SCOPE.md` tanpa konfirmasi.
13. Commit langsung ke `main`.
