# 23 — Acceptance Criteria
Fitur AKTIF saja (fitur diterima → `docs/_archive/23-{fitur}.md`). Patokan selesai = kriteria di sini + `docs/24_DEFINITION_OF_DONE.md`. Fitur: `docs/01_PRD.md`. Kriteria antarmuka umum (berlaku semua fitur ber-UI): empat state tampil, pesan validasi di bawah field, layak di 360 px, aksi destruktif lewat dialog (`docs/26_UI_CONVENTIONS.md`).

## P0 — Fondasi (Auth)
- Given user aktif dengan NIP & password benar, When login, Then diarahkan ke dasbor perannya.
- Given `must_change_password=true`, When membuka URL apa pun, Then dialihkan ke halaman ganti password. Password = NIP ditolak.
- Given 5 percobaan gagal dalam 1 menit, When mencoba lagi, Then ditolak dengan pesan tunggu.
- Given `is_active=false`, When login dengan kredensial benar, Then ditolak dengan pesan generik.
```bash
php artisan test --filter=Auth
```
Lulus: semua tes di `tests/Feature/Auth` hijau, ≥ 4 kasus di atas.

## F1 — Kelola pelatihan
- Given admin, When menyimpan pelatihan tanpa ambang, Then default 45 menit/JP, 80%, 70 tersimpan dan status `draft`.
- Given `end_date < start_date` atau `code` duplikat, When simpan, Then pesan validasi di field terkait.
- Given narasumber/peserta, When membuka `/admin/pelatihan`, Then 403.
```bash
php artisan test --filter=TrainingManagementTest
```

## F2 — Impor peserta
- Given xlsx 3 baris valid + 1 NIP 17 digit + 1 NIP duplikat dalam berkas, When diimpor, Then 3 enrollment dibuat, `imports` = total 5 / sukses 3 / gagal 2 dengan alasan per baris.
- Given NIP sudah ada sebagai user peserta, When diimpor ke pelatihan lain, Then user dipakai ulang (tanpa user baru) dan enrollment dibuat.
- Given NIP diawali `0`, When diimpor, Then tersimpan utuh 18 karakter.
- Given berkas `.csv` atau header salah, When diunggah, Then ditolak sebelum masuk queue.
```bash
php artisan test --filter=ParticipantImportTest
```

## F3 — Materi, sesi & narasumber
- Given materi 6 JP dengan sesi 4 JP, When admin menekan Mulai pelatihan, Then gagal dengan pesan menyebut materi & selisih JP. Status tetap `draft`.
- Given semua syarat `docs/06_BUSINESS_PROCESS.md` A terpenuhi, When Mulai, Then status `ongoing`, `started_at` terisi.
- Given peserta terdaftar, When membuka jadwal, Then sesi tampil terurut tanggal/jam dengan materi, narasumber, JP, lokasi.
```bash
php artisan test --filter=SubjectSessionTest
php artisan test --filter=StartTrainingTest
```

## F4 — Presensi
- Given narasumber materi A, When menyimpan presensi sesi materi A, Then baris tersimpan (upsert, tanpa duplikat).
- Given narasumber materi A, When membuka presensi sesi materi B, Then 403.
- Given pelatihan `closed`, When menyimpan presensi, Then ditolak.
- Given total 10 JP dan peserta hadir di sesi 8 JP, When rekap dibuka, Then JP hadir 8, kehadiran 80,00%.
```bash
php artisan test --filter=AttendanceTest
```

## F5 — Bahan ajar
- Given narasumber materinya, When mengunggah PDF 5 MB, Then tersimpan di `materials/` disk privat dan muncul di daftar peserta.
- Given berkas `.exe` atau > 20 MB, When unggah, Then pesan validasi.
- Given peserta pelatihan lain, When mengakses URL unduh, Then 403.
```bash
php artisan test --filter=MaterialFileTest
```

## F6 — Tugas
- Given sebelum tenggat, When peserta mengunggah dua kali, Then satu baris `submissions` dengan berkas terbaru.
- Given lewat tenggat atau sudah dinilai, When unggah, Then ditolak.
- Given narasumber, When memberi nilai 101, Then pesan validasi. Nilai 85 tersimpan dengan `graded_by`/`graded_at`.
```bash
php artisan test --filter=AssignmentTest
```

## F7 — Pre/post-test
- Given di luar jendela, When peserta mulai, Then ditolak.
- Given 10 soal, 7 benar, When dikumpulkan, Then nilai 70,00.
- Given sudah punya attempt `post`, When mulai lagi, Then ditolak.
- Given `started_at + durasi` lewat tanpa kumpul, When `tests:finalize-expired` jalan, Then attempt dinilai dari jawaban tersimpan.
- Given komponen tes dirender untuk peserta, When payload Livewire diperiksa, Then tidak memuat string `is_correct`.
- Given sudah ada attempt, When admin mengubah soal, Then ditolak.
```bash
php artisan test --filter=PrePostTestTest
php artisan test --filter=TestScorerTest
```

## F8 — Penutupan & sertifikat
- Given peserta A (90%, post 75), B (79,99%, post 90), C (95%, tanpa post), When ditutup, Then A `passed`, B & C `failed`, nilai dibekukan di `enrollments`, status `closed`.
- Given ditutup, Then `IssueCertificatesJob` ter-dispatch tepat 1× (untuk A). Nomor sesuai pola, `{seq}` = 001.
- Given kode verifikasi valid, When `/verifikasi/{code}` dibuka tanpa login, Then status "Valid", NIP tersamar. Setelah dicabut, status "Dicabut".
- Given pelatihan `closed`, When peserta mengunduh sertifikatnya, Then PDF diunduh. Peserta lain → 403.
- Given dialog tutup, When kode pelatihan yang diketik salah, Then tombol Tutup tetap nonaktif.
```bash
php artisan test --filter=CloseTrainingTest
php artisan test --filter=CertificateTest
php artisan test --filter=PassingCalculatorTest
```

## F9 — Laporan
- Given pelatihan `closed`, When ekspor Excel, Then berkas berisi 4 sheet (Kehadiran, Nilai Tes, Tugas, Kelulusan) dengan baris = jumlah peserta.
- Given pelatihan belum `closed`, When ekspor, Then judul memuat "SEMENTARA".
- Given peserta, When membuka URL ekspor, Then 403.
```bash
php artisan test --filter=ReportExportTest
```

## Otorisasi lintas fitur
```bash
php artisan test --filter=AuthorizationMatrixTest
```
Lulus: dataset mencakup setiap baris matriks `docs/05_USER_ROLE.md` × 3 peran, semuanya hijau.
