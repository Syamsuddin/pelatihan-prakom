# 04 — Domain Model
Pemilik istilah domain & entitas konseptual. Skema fisik: `docs/07_DATA_MODEL.md`.

## Glosarium
| Istilah (UI) | Identifier kode | Makna |
|---|---|---|
| Pelatihan | `Training` | Satu penyelenggaraan/angkatan dengan peserta, materi, tanggal, dan aturan lulusnya sendiri |
| Angkatan | `Training.batch` | Nomor urut penyelenggaraan untuk judul pelatihan yang sama |
| JP (Jam Pelajaran) | `jp` | Satuan beban belajar; durasi 1 JP = `minutes_per_jp` pelatihan (default 45 menit) |
| Materi | `Subject` | Mata pelatihan dengan total JP tetap dan satu narasumber |
| Sesi | `TrainingSession` | Satu pertemuan terjadwal milik satu materi, berbobot sejumlah JP. Σ JP sesi = JP materi |
| Peserta terdaftar | `Enrollment` | Keikutsertaan satu user di satu pelatihan; pembawa hasil (nilai, kelulusan) |
| Presensi | `Attendance` | Status satu peserta di satu sesi: hadir/izin/sakit/alpa |
| JP hadir | (dihitung) | Σ JP sesi dengan status *hadir* |
| Total JP | (dihitung) | Σ JP seluruh materi pelatihan |
| Persentase kehadiran | (dihitung) | JP hadir ÷ Total JP × 100, dibulatkan 2 desimal |
| Bahan ajar | `MaterialFile` | Berkas milik satu materi |
| Tugas / Pengumpulan | `Assignment` / `Submission` | Penugasan per materi / jawaban satu peserta (nilai 0–100) |
| Pre-test / Post-test | `TestAttempt.type` = `pre`/`post` | Ujian PG dari bank soal pelatihan yang sama; nilai = benar ÷ jumlah soal × 100 |
| Peningkatan | (dihitung) | Nilai post − nilai pre |
| Jendela tes | `pretest_opens_at`… | Rentang waktu tes boleh dimulai, ditetapkan admin |
| Kelulusan | `Enrollment.result` | `passed` bila % kehadiran ≥ ambang DAN post-test ≥ ambang; selain itu `failed`; `pending` sebelum ditutup |
| Tutup pelatihan | `CloseTrainingAction` | Mengunci data, membekukan hasil, menerbitkan sertifikat. Irreversibel |
| Sertifikat | `Certificate` | Bukti lulus: nomor unik, total JP, kode verifikasi; bisa *dicabut* (tidak dihapus) |
| Kode verifikasi | `verification_code` | Token acak di QR → halaman verifikasi publik |

## Status pelatihan
`draft` → `ongoing` (berjalan) → `closed` (ditutup). Tidak ada transisi mundur di MVP. Syarat tiap transisi: `docs/06_BUSINESS_PROCESS.md`.

## Entitas & relasi konseptual
- Pelatihan 1—N Materi 1—N Sesi.
- Pelatihan 1—N Peserta terdaftar N—1 User (peran peserta).
- Materi N—1 User (peran narasumber).
- Peserta terdaftar 1—N Presensi (satu per sesi), 1—N Pengumpulan, 0..2 Percobaan tes (pre, post), 0..1 Sertifikat.
- Pelatihan 1—N Soal 1—N Opsi (tepat satu benar).
