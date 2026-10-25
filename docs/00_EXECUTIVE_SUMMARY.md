# 00 — Executive Summary

## Masalah
Pelatihan dikelola dengan Excel terpisah-pisah (daftar peserta, presensi, nilai, sertifikat). Rekap JP, kelulusan, dan sertifikat dikerjakan manual setelah pelatihan selesai, sehingga lambat dan rawan salah hitung.

## Solusi
MyCORPU: aplikasi web corporate university untuk banyak pelatihan/angkatan. Admin menyusun pelatihan (materi ber-JP → sesi terjadwal), mengimpor peserta dari Excel, dan mencatat presensi bersama narasumber. Peserta mengunduh bahan, mengumpulkan tugas, serta mengerjakan pre/post-test. Saat pelatihan ditutup, kelulusan dihitung otomatis dan sertifikat PDF ber-QR verifikasi langsung terbit.

## Nilai & metrik sukses
- **Metrik utama:** sertifikat + laporan akhir terbit ≤ 1 hari kerja setelah pelatihan ditutup.
- Nol rekap JP/kelulusan manual di Excel.

## Pengguna sasaran
Admin/panitia penyelenggara, narasumber, dan peserta pelatihan (ASN). Peran & akses: `docs/05_USER_ROLE.md`.

## Status & cakupan
Greenfield. MVP = 9 fitur (`docs/01_PRD.md`), dibangun bertahap per vertical slice (`docs/03_ROADMAP.md`). Batas scope: `docs/02_SCOPE.md`.
