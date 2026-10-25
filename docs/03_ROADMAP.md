# 03 — Roadmap
Fase = vertical slice (migrasi → Action/Service → Livewire → tes) yang bisa didemokan. Kerjakan berurutan; fase berikut boleh mulai bila kriteria `docs/23_ACCEPTANCE_CRITERIA.md` fase sebelumnya hijau.

| Fase | Tujuan demoable | Fitur |
|---|---|---|
| P0 Fondasi | Admin seed login dengan NIP, dipaksa ganti password, lalu melihat layout dasbor sesuai peran | Auth NIP, ganti password wajib, layout + komponen `x-ui.*`, token Tailwind |
| P1 Pelatihan & peserta | Admin membuat pelatihan, mengimpor Excel 50 peserta, melihat daftar + laporan baris gagal | F1, F2 |
| P2 Kurikulum & jadwal | Admin menyusun materi (JP) → sesi, menugaskan narasumber, memulai pelatihan; peserta melihat jadwal | F3 + transisi draft→berjalan |
| P3 Presensi | Narasumber mengisi grid presensi sesinya; admin melihat rekap JP hadir & persentase per peserta | F4 |
| P4 Bahan & tugas | Narasumber mengunggah bahan & membuat tugas; peserta mengunduh/mengumpulkan; narasumber menilai | F5, F6 |
| P5 Pre/post-test | Admin menyusun soal & membuka jendela; peserta mengerjakan; nilai + peningkatan tampil | F7 |
| P6 Penutupan & sertifikat | Pratinjau kelulusan → tutup → sertifikat PDF terbit; QR membuka halaman verifikasi | F8 |
| P7 Laporan & rilis | Ekspor Excel/PDF laporan akhir; deploy ke VPS sesuai `docs/25_RELEASE_CHECKLIST.md` | F9 |

Pasca-MVP (urut usulan): kuesioner evaluasi → notifikasi email → TTE BSrE → dasbor lintas tahun.
