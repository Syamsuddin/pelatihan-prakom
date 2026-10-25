# 15 — Observability
Pemilik logging & metrik.

## Kanal log
| Kanal | Berkas | Isi |
|---|---|---|
| `daily` (via `stack`) | `storage/logs/laravel-YYYY-MM-DD.log`, retensi 14 hari | Error aplikasi, warning otorisasi |
| `audit` (daily, kanal kustom di `config/logging.php`) | `storage/logs/audit-YYYY-MM-DD.log`, retensi 365 hari | Operasi penting (tabel di bawah) |

## Peristiwa audit (wajib dicatat, level `info`)
| Peristiwa | Konteks wajib |
|---|---|
| `auth.login_failed` | nip, ip |
| `training.started` / `training.closed` | training_id, user_id, jumlah lulus/gagal (closed) |
| `import.finished` | import_id, training_id, success_rows, error_rows |
| `certificate.issued` / `certificate.revoked` | certificate_id, number, user_id, reason (revoked) |
| `user.password_reset` / `user.deactivated` | target_user_id, user_id |
| `participant.removed` | enrollment_id, user_id |

Format: `Log::channel('audit')->info('training.closed', [...])`, konteks array (JSON). **Jangan** mencatat password, isi berkas, atau nomor HP.

## Level
`error` = butuh tindakan · `warning` = mencurigakan (403 berulang) · `info` = audit · `debug` hanya lokal.

## Metrik & monitoring
- Antrean macet: `php artisan queue:failed` dicek di smoke test rilis (`docs/25_RELEASE_CHECKLIST.md`).
- [TERBUKA] Uptime monitor & notifikasi error eksternal pasca-MVP.
