# 07 — Data Model
Sumber kebenaran skema (MySQL 8, InnoDB, utf8mb4). Dokumen lain merujuk, tidak menyalin. Semua tabel punya `created_at`/`updated_at` kecuali dinyatakan lain. Enum disimpan sebagai `varchar` + PHP backed enum di `app/Enums/` (bukan enum MySQL), agar penambahan nilai tidak butuh migrasi ALTER.

## users
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| id | bigint unsigned | PK | |
| nip | varchar(30) | UNIQUE, NOT NULL | Identitas login. ASN = NIP 18 digit; narasumber non-ASN = kode dari admin [ASUMSI]. PII |
| name | varchar(150) | NOT NULL | Nama lengkap + gelar |
| email | varchar(150) | NULL, UNIQUE | PII |
| phone | varchar(20) | NULL | PII |
| position | varchar(150) | NULL | Jabatan |
| rank | varchar(60) | NULL | Pangkat/golongan |
| unit | varchar(200) | NULL | Unit kerja |
| role | varchar(20) | NOT NULL | `App\Enums\Role`: admin/narasumber/peserta |
| password | varchar(255) | NOT NULL | bcrypt |
| must_change_password | boolean | NOT NULL, default true | |
| is_active | boolean | NOT NULL, default true | |
| remember_token | varchar(100) | NULL | |
| deleted_at | timestamp | NULL | Soft delete |

Kolom Excel impor (baris 1 = header, persis): `nip, nama, jabatan, pangkat_golongan, unit_kerja, email, no_hp` → `nip, name, position, rank, unit, email, phone`.

## trainings
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| id | bigint unsigned | PK | |
| code | varchar(30) | UNIQUE, NOT NULL | Kode singkat, mis. `PRAKOM-2026-01`; dipakai di nomor sertifikat |
| title | varchar(200) | NOT NULL | |
| batch | smallint unsigned | NULL | Angkatan |
| description | text | NULL | |
| location | varchar(200) | NULL | |
| start_date / end_date | date | NOT NULL, end ≥ start | |
| minutes_per_jp | tinyint unsigned | NOT NULL, default 45 | |
| min_attendance_pct | tinyint unsigned | NOT NULL, default 80, 0–100 | |
| min_posttest_score | tinyint unsigned | NOT NULL, default 70, 0–100 | |
| test_duration_minutes | smallint unsigned | NOT NULL, default 30 | Berlaku untuk pre & post |
| pretest_opens_at / pretest_closes_at | datetime | NULL | Jendela pre-test |
| posttest_opens_at / posttest_closes_at | datetime | NULL | Jendela post-test |
| certificate_number_pattern | varchar(100) | NOT NULL, default `{seq}/{code}/{year}` | Token: `{seq}` (3 digit), `{code}`, `{year}`, `{month_roman}` |
| signer_name / signer_title | varchar(150) | NULL | Wajib terisi sebelum ditutup |
| signer_nip | varchar(30) | NULL | |
| status | varchar(20) | NOT NULL, default `draft` | `App\Enums\TrainingStatus`: draft/ongoing/closed |
| started_at / closed_at | timestamp | NULL | |
| closed_by | bigint unsigned | NULL, FK→users.id | |
| deleted_at | timestamp | NULL | Soft delete hanya boleh saat `draft` |

## enrollments
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| id | bigint unsigned | PK | |
| training_id | bigint unsigned | FK→trainings.id, NOT NULL | ON DELETE RESTRICT |
| user_id | bigint unsigned | FK→users.id, NOT NULL | ON DELETE RESTRICT |
| attended_jp | smallint unsigned | NULL | Dibekukan saat tutup |
| total_jp | smallint unsigned | NULL | Dibekukan saat tutup |
| attendance_pct | decimal(5,2) | NULL | Dibekukan saat tutup |
| pretest_score / posttest_score | decimal(5,2) | NULL | Dibekukan saat tutup |
| result | varchar(20) | NOT NULL, default `pending` | `App\Enums\EnrollmentResult`: pending/passed/failed |

UNIQUE(training_id, user_id).

## subjects (materi)
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| id | bigint unsigned | PK | |
| training_id | bigint unsigned | FK→trainings.id, NOT NULL | ON DELETE CASCADE (hanya terjadi saat draft) |
| title | varchar(200) | NOT NULL | |
| description | text | NULL | |
| jp | tinyint unsigned | NOT NULL, ≥ 1 | Total JP materi |
| speaker_id | bigint unsigned | NULL, FK→users.id | User ber-role narasumber |
| sort_order | smallint unsigned | NOT NULL, default 0 | |

## training_sessions (sesi)
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| id | bigint unsigned | PK | Nama tabel bukan `sessions` (dipakai session driver Laravel) |
| subject_id | bigint unsigned | FK→subjects.id, NOT NULL | ON DELETE CASCADE (draft) |
| date | date | NOT NULL | Dalam rentang tanggal pelatihan |
| start_time / end_time | time | NOT NULL, end > start | WITA |
| jp | tinyint unsigned | NOT NULL, ≥ 1 | |
| location | varchar(200) | NULL | Ruang / tautan daring (teks) |

Indeks: (date, start_time).

## attendances
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| id | bigint unsigned | PK | |
| training_session_id | bigint unsigned | FK→training_sessions.id, NOT NULL | ON DELETE RESTRICT |
| enrollment_id | bigint unsigned | FK→enrollments.id, NOT NULL | ON DELETE CASCADE |
| status | varchar(10) | NOT NULL | `App\Enums\AttendanceStatus`: hadir/izin/sakit/alpa |
| note | varchar(255) | NULL | |
| recorded_by | bigint unsigned | FK→users.id, NOT NULL | |

UNIQUE(training_session_id, enrollment_id).

## material_files
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| id | bigint unsigned | PK | |
| subject_id | bigint unsigned | FK→subjects.id, NOT NULL | |
| title | varchar(200) | NOT NULL | |
| path | varchar(255) | NOT NULL | Disk `local` (privat), `materials/{subject_id}/{ulid}.{ext}` |
| original_name | varchar(255) | NOT NULL | |
| mime / size_bytes | varchar(100) / int unsigned | NOT NULL | |
| uploaded_by | bigint unsigned | FK→users.id, NOT NULL | |

## assignments
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| id | bigint unsigned | PK | |
| subject_id | bigint unsigned | FK→subjects.id, NOT NULL | |
| title | varchar(200) | NOT NULL | |
| instructions | text | NOT NULL | |
| due_at | datetime | NOT NULL | |
| created_by | bigint unsigned | FK→users.id, NOT NULL | |

## submissions
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| id | bigint unsigned | PK | |
| assignment_id | bigint unsigned | FK→assignments.id, NOT NULL | |
| enrollment_id | bigint unsigned | FK→enrollments.id, NOT NULL | |
| path / original_name | varchar(255) | NOT NULL | Disk `local`, `submissions/{assignment_id}/{ulid}.{ext}` |
| size_bytes | int unsigned | NOT NULL | |
| submitted_at | datetime | NOT NULL | |
| score | tinyint unsigned | NULL, 0–100 | NULL = belum dinilai |
| feedback | text | NULL | |
| graded_by | bigint unsigned | NULL, FK→users.id | |
| graded_at | datetime | NULL | |

UNIQUE(assignment_id, enrollment_id) — unggah ulang mengganti berkas di baris yang sama.

## questions / question_options
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| questions.id | bigint unsigned | PK | |
| questions.training_id | bigint unsigned | FK→trainings.id, NOT NULL | |
| questions.body | text | NOT NULL | |
| questions.sort_order | smallint unsigned | NOT NULL, default 0 | |
| question_options.id | bigint unsigned | PK | |
| question_options.question_id | bigint unsigned | FK→questions.id, NOT NULL, ON DELETE CASCADE | |
| question_options.label | char(1) | NOT NULL | A–E |
| question_options.body | text | NOT NULL | |
| question_options.is_correct | boolean | NOT NULL, default false | Tepat satu `true` per soal (dicek Action). **Rahasia** — lihat `docs/21_SECURITY_RULES.md` |

Soal terkunci (tak bisa diubah/dihapus) begitu ada `test_attempts` di pelatihan tersebut.

## test_attempts / attempt_answers
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| test_attempts.id | bigint unsigned | PK | |
| test_attempts.enrollment_id | bigint unsigned | FK→enrollments.id, NOT NULL | |
| test_attempts.type | varchar(4) | NOT NULL | `App\Enums\TestType`: pre/post |
| test_attempts.started_at | datetime | NOT NULL | |
| test_attempts.submitted_at | datetime | NULL | NULL = sedang dikerjakan |
| test_attempts.score | decimal(5,2) | NULL | benar ÷ jumlah soal × 100 |
| attempt_answers.id | bigint unsigned | PK | |
| attempt_answers.test_attempt_id | bigint unsigned | FK→test_attempts.id, NOT NULL, ON DELETE CASCADE | |
| attempt_answers.question_id | bigint unsigned | FK→questions.id, NOT NULL | |
| attempt_answers.question_option_id | bigint unsigned | NULL, FK→question_options.id | NULL = tidak dijawab |

UNIQUE(test_attempts.enrollment_id, type); UNIQUE(attempt_answers.test_attempt_id, question_id).

## certificates
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| id | bigint unsigned | PK | |
| enrollment_id | bigint unsigned | FK→enrollments.id, UNIQUE, NOT NULL | |
| number | varchar(100) | UNIQUE, NOT NULL | Hasil pola `certificate_number_pattern` |
| verification_code | char(26) | UNIQUE, NOT NULL | ULID acak |
| holder_name / holder_nip | varchar(150) / varchar(30) | NOT NULL | Snapshot saat terbit |
| total_jp | smallint unsigned | NOT NULL | Snapshot |
| issued_at | datetime | NOT NULL | |
| path | varchar(255) | NULL | PDF, disk `local`, `certificates/{training_id}/{number-slug}.pdf`; NULL = belum dirender |
| revoked_at | datetime | NULL | Tidak pernah hard-delete |
| revoked_reason | varchar(255) | NULL | Wajib bila dicabut |
| revoked_by | bigint unsigned | NULL, FK→users.id | |

## imports
| Kolom | Tipe | Constraint | Catatan |
|---|---|---|---|
| id | bigint unsigned | PK | |
| training_id | bigint unsigned | FK→trainings.id, NOT NULL | |
| uploaded_by | bigint unsigned | FK→users.id, NOT NULL | |
| status | varchar(20) | NOT NULL | queued/processing/done/failed |
| total_rows / success_rows / error_rows | int unsigned | NOT NULL, default 0 | |
| errors | json | NULL | `[{row, nip, reason}]` |

## Tabel bawaan Laravel
`sessions`, `cache`, `jobs`, `job_batches`, `failed_jobs`, `password_reset_tokens` — migrasi default Laravel 12, tidak diubah.

## Aturan integritas lintas tabel
- Setelah `trainings.status = closed`: tidak ada INSERT/UPDATE/DELETE pada enrollments, attendances, submissions, questions, test_attempts milik pelatihan itu. Pengecualian: kolom `revoked_*` dan `path` di certificates. Dijaga di Action/Policy (`docs/21_SECURITY_RULES.md`).
- Data sensitif (kolom PII di `users`, berkas di disk privat): aturan akses di `docs/21_SECURITY_RULES.md`.
