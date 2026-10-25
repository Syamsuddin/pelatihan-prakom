# 08 — Architecture
Monolit Laravel modular, server-rendered dengan Livewire. Teknologi & versi: `docs/09_STACK.md`.

## Lapisan
```
Browser ──HTTP──▶ routes/web.php ──▶ Middleware (auth, EnsurePasswordChanged, role)
                                   ├─▶ Livewire component (app/Livewire/**)  ── UI state + validasi form
                                   └─▶ Controller tipis (unduh berkas, PDF, verifikasi publik, ekspor)
                                             │  $this->authorize() → app/Policies/**
                                             ▼
                                   Action (app/Actions/**)   ── satu kasus pakai, transaksi DB, aturan bisnis
                                             │
                                   Service (app/Services/**) ── logika murni bersama (hitung kelulusan, nomor sertifikat, PDF)
                                             │
                                   Eloquent Model (app/Models/**) ── relasi, cast, scope; tanpa aturan bisnis
                                             │
                                   MySQL 8  ·  Storage disk `local` (privat)  ·  Queue `database`
```

## Tanggung jawab
| Lapisan | Boleh | Tidak boleh |
|---|---|---|
| Livewire component | Validasi input (`#[Validate]`/`rules()`), authorize, panggil Action, render view | Query kompleks, aturan bisnis, transaksi |
| Controller | Stream berkas/PDF/Excel, halaman publik verifikasi | Aturan bisnis |
| Action | Aturan bisnis + transaksi; melempar `BusinessRuleException` (`docs/14_ERROR_HANDLING.md`) | Akses `request()`/session, render |
| Service | Fungsi deterministik yang bisa diuji unit | Efek samping di luar yang dinyatakan |
| Job | Membungkus Action untuk kerja berat (impor, sertifikat massal) | Logika bisnis sendiri |

## Komponen kunci
| Komponen | Path | Fungsi |
|---|---|---|
| `PassingCalculator` | `app/Services/PassingCalculator.php` | JP hadir, total JP, %, hasil lulus. **Satu-satunya** tempat rumus kelulusan |
| `TestScorer` | `app/Services/TestScorer.php` | Nilai pre/post dari jawaban tersimpan |
| `CertificateNumberGenerator` | `app/Services/CertificateNumberGenerator.php` | Isi pola nomor; `{seq}` = urutan per pelatihan, dikunci `lockForUpdate` |
| `CertificatePdfRenderer` | `app/Services/CertificatePdfRenderer.php` | dompdf + QR SVG → PDF |
| `StartTrainingAction` / `CloseTrainingAction` | `app/Actions/Training/` | Transisi status (`docs/06_BUSINESS_PROCESS.md` A, F) |
| `ImportParticipantsJob` | `app/Jobs/` | Memakai `app/Imports/ParticipantsImport.php` |
| `IssueCertificatesJob` | `app/Jobs/` | Satu job per enrollment lulus (retry independen) |
| `tests:finalize-expired` | `app/Console/Commands/` | Scheduler tiap menit: menilai attempt yang waktunya habis |

## Keputusan arsitektural
| Keputusan | Alasan |
|---|---|
| Livewire 3 class-based, **tanpa Volt & tanpa Flux** | Satu gaya komponen; UI dikendalikan penuh oleh token `docs/26_UI_CONVENTIONS.md` |
| Auth ditulis sendiri (Livewire `Auth\Login`), tanpa starter kit | Login pakai NIP, bukan email; starter kit membawa Flux/Volt |
| Otorisasi via Policy + `role` kolom tunggal (tanpa spatie/permission) | Hanya 3 peran tetap; aturan berbasis kepemilikan (materi/enrollment), bukan izin granular |
| Nilai hasil dibekukan di `enrollments` saat tutup | Laporan & sertifikat tidak berubah bila data mentah disentuh kemudian |
| Queue driver `database` | Tanpa Redis di MVP; cukup untuk beban ratusan peserta |
| Berkas di disk `local` privat, diunduh lewat route ber-Policy | Bahan/tugas bukan publik |
| Enum sebagai varchar + PHP enum | Tambah nilai tanpa migrasi ALTER |

## Integrasi eksternal
Tidak ada di MVP (lihat `docs/02_SCOPE.md`).
