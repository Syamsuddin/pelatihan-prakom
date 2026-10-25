# 24 — Definition of Done
Berlaku untuk setiap task. Semua harus ✓.

- [ ] Kriteria terima fitur terkait di `docs/23_ACCEPTANCE_CRITERIA.md` terpenuhi, termasuk blok verifikasinya.
- [ ] `php artisan test` hijau seluruhnya, tanpa tes baru yang di-skip (`docs/13_TESTING.md`).
- [ ] `vendor/bin/pint --test` bersih.
- [ ] Tes baru ditambahkan untuk perilaku baru / bug yang diperbaiki.
- [ ] Struktur & penamaan sesuai `docs/12_PROJECT_STRUCTURE.md`; aturan bisnis tidak berada di Livewire/Controller.
- [ ] Tidak melanggar `docs/20_GUARDRAILS.md` dan `docs/21_SECURITY_RULES.md`. Setiap aksi baru memanggil `authorize`.
- [ ] UI: hanya token & komponen `docs/26_UI_CONVENTIONS.md`; empat state ada; dicek di 360 px & 1280 px.
- [ ] Migrasi baru punya `down()` yang benar; perubahan skema tercermin di `docs/07_DATA_MODEL.md`.
- [ ] `npm run build` sukses bila menyentuh aset.
- [ ] Dokumen pemilik diperbarui bila fakta berubah, dan `bash scripts/validate.sh` tanpa `[FAIL]`.
