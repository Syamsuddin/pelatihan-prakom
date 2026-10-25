# 18 — Repair Rules
Pemilik aturan perbaikan bug.

## Alur
1. **Reproduksi**: tulis tes Pest yang gagal dan menggambarkan bug (`tests/Feature/...` atau `tests/Unit/...`).
2. **Akar masalah**: telusuri sampai sebab, bukan gejala. Cek `docs/16_DEBUGGING_GUIDE.md` (jebakan) dan `docs/07_DATA_MODEL.md` (constraint).
3. **Perbaikan minimal** di lapisan yang benar (`docs/08_ARCHITECTURE.md`).
4. **Verifikasi**: tes reproduksi hijau + `php artisan test` hijau + `vendor/bin/pint --test` bersih.
5. Commit: `fix: <ringkas>`, sebut tes regresi di badan commit.

## Larangan
- Perbaikan tidak melebihi scope bug: tanpa refactor, rename, atau "sekalian rapikan".
- Jangan "memperbaiki" dengan melonggarkan validasi, Policy, atau constraint DB.
- Jangan mengubah data produksi langsung via SQL untuk menutupi bug. Koreksi data = operasi irreversibel (`docs/22_CHANGE_POLICY.md`).
- Bug pada data pelatihan yang sudah `closed` → berhenti & laporkan. Jangan membuka kunci tanpa keputusan pemilik produk.
- Bila perbaikan butuh perubahan skema → ikuti rute "Ubah skema DB" di INDEX.
