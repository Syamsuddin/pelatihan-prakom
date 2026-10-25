# 12 — Project Structure
Pemilik struktur folder & penamaan.

## Pohon (tingkat penting)
```
app/
├── Actions/{Training,Enrollment,Subject,Attendance,Material,Assignment,Test,Certificate}/   # XxxAction.php, method handle()
├── Console/Commands/FinalizeExpiredTests.php
├── Enums/            # Role, TrainingStatus, AttendanceStatus, EnrollmentResult, TestType, ImportStatus
├── Exceptions/BusinessRuleException.php
├── Exports/          # TrainingReportExport.php (+ sheet per jenis)
├── Http/Controllers/ # FileDownloadController, CertificateController, VerificationController, ReportController
├── Http/Middleware/  # EnsurePasswordChanged, EnsureRole
├── Imports/ParticipantsImport.php
├── Jobs/             # ImportParticipantsJob, IssueCertificatesJob
├── Livewire/
│   ├── Auth/         # Login, ChangePassword
│   ├── Admin/        # Dashboard, Trainings/{Index,Form,Show}, Participants/Import, Subjects/*, Sessions/*, Questions/*, Closing/Preview, Certificates/Index, Users/*
│   ├── Speaker/      # Dashboard, Subjects/Show, Grading/*
│   ├── Participant/  # Dashboard, Trainings/Show, Assignments/Submit, Tests/Take
│   └── Shared/       # AttendanceGrid (dipakai admin & narasumber), Schedule
├── Models/           # Training, Subject, TrainingSession, Enrollment, Attendance, MaterialFile, Assignment, Submission, Question, QuestionOption, TestAttempt, AttemptAnswer, Certificate, Import, User
├── Policies/
└── Services/         # PassingCalculator, TestScorer, CertificateNumberGenerator, CertificatePdfRenderer
resources/
├── css/app.css       # @import "tailwindcss" + blok @theme token (docs/26)
├── views/components/ui/     # komponen x-ui.* (inventaris: docs/26)
├── views/components/icon/   # Heroicons SVG
├── views/components/layouts/{app,guest}.blade.php
├── views/livewire/{auth,admin,speaker,participant,shared}/...
└── views/pdf/{certificate,report}.blade.php
routes/web.php        # dikelompokkan prefix: /admin, /narasumber, /peserta, /verifikasi
tests/Feature/{Auth,Training,Import,Attendance,Material,Assignment,Test,Closing,Certificate,Report,Authorization}/
tests/Unit/Services/
database/{migrations,factories,seeders}/
storage/app/private/{materials,submissions,certificates,imports}/
```

## Konvensi penamaan
| Hal | Konvensi | Contoh |
|---|---|---|
| Model | PascalCase tunggal, Inggris | `TrainingSession` |
| Tabel/kolom | snake_case jamak/tunggal, Inggris | `training_sessions.start_time` |
| Action | Kata kerja + objek + `Action` | `CloseTrainingAction` |
| Livewire | Folder per peran, nama halaman | `Admin\Trainings\Show` |
| Route name | `{peran}.{sumber}.{aksi}` | `admin.trainings.show` |
| URL | Indonesia, kebab-case | `/admin/pelatihan/{training}/presensi` |
| Tes | `{Fitur}Test.php`, deskripsi `it('...')` berbahasa Indonesia | `CloseTrainingTest` |
| Teks UI | Bahasa Indonesia, di Blade (tanpa file terjemahan di MVP) | — |

## Lokasi jenis kode
- Aturan bisnis → `app/Actions` atau `app/Services`, **bukan** Livewire/Controller/Model.
- Rumus kelulusan → hanya `PassingCalculator`.
- Cek kepemilikan/peran → hanya `app/Policies`.
- Nilai warna/ukuran → hanya `@theme` di `resources/css/app.css` (`docs/26_UI_CONVENTIONS.md`).
