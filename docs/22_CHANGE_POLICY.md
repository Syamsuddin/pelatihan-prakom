# 22 — Change Policy
Friksi sebanding irreversibilitas.

## Git
- Branch per fitur/perbaikan dari `main`: `feat/<fase>-<ringkas>`, `fix/<ringkas>`, `docs/<ringkas>`.
- Commit konvensional: `feat:`, `fix:`, `docs:`, `test:`, `refactor:`, `chore:`.
- Dilarang commit langsung ke `main`. Merge via PR setelah DoD terpenuhi (`docs/24_DEFINITION_OF_DONE.md`). Yang berhak merge: pemilik produk (pengguna).
- Satu PR = satu vertical slice/fitur. Jangan menggabungkan fitur tak terkait.

## Operasi irreversibel → gerbang manusia (wajib konfirmasi eksplisit sebelum dikerjakan/dijalankan)
| Operasi | Gerbang |
|---|---|
| Migrasi destruktif (drop/rename kolom/tabel, ubah tipe yang memotong data) | Tampilkan SQL/migrasi + dampak → konfirmasi → backup dulu |
| Menjalankan migrasi di produksi | Hanya lewat `docs/25_RELEASE_CHECKLIST.md` |
| Hapus pelatihan / keluarkan peserta / hapus user | UI: dialog konfirmasi; kode: hanya soft delete sesuai `docs/07_DATA_MODEL.md` |
| **Tutup pelatihan** | UI: pratinjau + ketik kode pelatihan; kode: tak ada jalur buka kembali |
| Cabut sertifikat | UI: dialog + alasan wajib |
| Koreksi data produksi via SQL/tinker | Dilarang tanpa persetujuan tertulis + backup |
| Menambah/mengganti dependensi, mengubah aturan keamanan (`docs/21_SECURITY_RULES.md`) | Usulan + alasan → konfirmasi |
| Mengubah rumus kelulusan atau aturan di `docs/06_BUSINESS_PROCESS.md` | Konfirmasi pemilik produk + perbarui dokumen pemilik dulu |
| Rilis produksi | `docs/25_RELEASE_CHECKLIST.md` |

## Perubahan dokumen
Fakta berubah → ubah dokumen pemiliknya (peta di `docs/_MANIFEST.json` `canonical_owners`) + manifest → regenerasi perujuk (`referenced_by`) → `bash scripts/validate.sh`.

## Rollback
- Kode: deploy ulang tag rilis sebelumnya (`git checkout <tag>` → langkah deploy).
- Skema: setiap migrasi wajib punya `down()` yang benar. Rollback produksi hanya `migrate:rollback --step=1` setelah backup, atau pulihkan dari backup bila `down()` destruktif.
