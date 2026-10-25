# 13 — Testing
Pemilik strategi tes. Perintah: `docs/11_COMMANDS.md`.

## Jenis & cakupan
| Jenis | Alat | Cakupan target |
|---|---|---|
| Unit | Pest | Semua `app/Services/*`: 100% cabang `PassingCalculator`, `TestScorer`, `CertificateNumberGenerator` |
| Feature (HTTP + Livewire) | Pest + `Livewire::test()` | Setiap alur MVP, termasuk happy path dan ≥ 1 cabang gagal (`docs/06_BUSINESS_PROCESS.md`) |
| Otorisasi | Pest dataset | Matriks `docs/05_USER_ROLE.md`: tiap kapabilitas × tiap peran → izin/403 |
| Regresi bug | Pest | 1 tes yang gagal sebelum perbaikan (`docs/18_REPAIR_RULES.md`) |

## Wajib dites (alur kritikal)
1. Login NIP, user nonaktif ditolak, paksa ganti password, throttle.
2. Impor Excel: baris valid masuk, NIP salah/duplikat/sudah terdaftar dilaporkan, NIP dengan nol di depan tetap utuh.
3. Syarat `StartTrainingAction` (Σ JP sesi ≠ JP materi → gagal).
4. Presensi: upsert, narasumber hanya untuk sesinya, terkunci setelah `closed`.
5. `PassingCalculator`: batas tepat (79,99% vs 80%; post 69,99 vs 70), tanpa post-test, tanpa presensi.
6. Tes: di luar jendela ditolak, percobaan kedua ditolak, waktu habis dinilai, `is_correct` tidak terkirim ke peserta.
7. `CloseTrainingAction`: nilai dibekukan, data terkunci, job sertifikat ter-dispatch hanya untuk yang lulus.
8. Sertifikat: nomor unik & berurutan, halaman verifikasi valid/dicabut, NIP tersamar.
9. Unduh berkas privat: peserta pelatihan lain → 403.

## Aturan
- Gunakan factory (`database/factories`) + `RefreshDatabase`. Jangan bergantung pada data seed.
- Waktu: `$this->travelTo()` / `Carbon::setTestNow()`. Jangan memakai jam asli.
- Queue: `Queue::fake()` untuk cek dispatch, `Bus::dispatchSync` untuk menguji isi job.
- Berkas: `Storage::fake('local')` + `UploadedFile::fake()`.
- Excel: fixture `.xlsx` di `tests/Fixtures/`, atau `Excel::fake()` untuk cek antrean.
- Tidak boleh menghapus/melewati tes untuk membuat suite hijau.
