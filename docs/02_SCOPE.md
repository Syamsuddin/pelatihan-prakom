# 02 — Scope
Gerbang anti scope-creep. Permintaan di luar daftar In-scope → berhenti & tanya.

## In-scope (MVP)
- Fitur F1–F9 di `docs/01_PRD.md`, untuk tiga peran: admin, narasumber, peserta.
- Banyak pelatihan/angkatan berjalan paralel. Satu peserta bisa ikut banyak pelatihan.
- Akun dibuat admin (impor Excel / form). Login memakai NIP + password.
- Berkas (bahan, tugas, sertifikat) di disk lokal privat server.

## Out-of-scope (eksplisit)
- **Integrasi SIASN / LMS LAN / sistem kepegawaian lain**, termasuk sinkronisasi data JP ke luar.
- **Tanda tangan elektronik (TTE BSrE)**: sertifikat ditandatangani berupa nama/jabatan tercetak. TTE masuk fase berikutnya.
- **Video conference, streaming, atau rekaman kelas**: tautan rapat boleh ditulis di deskripsi sesi sebagai teks.
- **Presensi mandiri peserta (QR, geolokasi, swafoto)**: presensi hanya diinput admin/narasumber.
- **Pendaftaran mandiri peserta** dan alur persetujuan pendaftaran.
- **Aplikasi mobile native**, multi-bahasa, notifikasi email/WA, pembayaran.
- **Bank soal selain pilihan ganda** (esai, isian, acak dari kumpulan besar).
- **Membuka kembali pelatihan yang sudah ditutup**. Lihat `[TERBUKA]` di bawah.

## Asumsi & batasan
- [ASUMSI] 1 user = 1 peran. Narasumber yang juga menjadi peserta memerlukan akun terpisah.
- [ASUMSI] Status presensi: hadir/izin/sakit/alpa. Hanya *hadir* yang dihitung sebagai JP kehadiran.
- [ASUMSI] Berkas maks. 20 MB; ekstensi pdf, docx, pptx, xlsx, zip.
- [TERBUKA] Prosedur koreksi setelah pelatihan ditutup. Sementara: cabut sertifikat (F8). Membuka kembali memerlukan keputusan pemilik produk.
- [TERBUKA] Desain visual template sertifikat (latar/logo instansi) menunggu aset dari pengguna. MVP memakai layout polos dari token `docs/26_UI_CONVENTIONS.md`.
