# 01 — PRD
Pemilik fitur & user story. Kriteria terima per fitur: `docs/23_ACCEPTANCE_CRITERIA.md`.

## Daftar fitur
| # | Fitur | Deskripsi | Prioritas |
|---|---|---|---|
| F1 | Kelola pelatihan | Buat/ubah pelatihan/angkatan: kode, judul, tanggal, lokasi, menit per JP (default 45), ambang kehadiran (default 80%) & post-test (default 70), pola nomor sertifikat, penandatangan | MVP |
| F2 | Impor peserta | Unggah Excel → akun peserta dibuat/dipakai ulang (kunci NIP) + didaftarkan ke pelatihan; laporan per baris gagal | MVP |
| F3 | Materi, sesi & narasumber | Materi punya total JP, dipecah ke ≥1 sesi terjadwal; tiap materi ditugaskan ke satu narasumber; peserta melihat jadwal | MVP |
| F4 | Presensi per sesi | Admin/narasumber menginput status per peserta per sesi; rekap JP hadir per peserta | MVP |
| F5 | Bahan ajar | Narasumber/admin mengunggah berkas per materi; peserta mengunduh | MVP |
| F6 | Tugas | Narasumber membuat tugas (instruksi + tenggat); peserta mengunggah jawaban; narasumber memberi nilai 0–100 + umpan balik | MVP |
| F7 | Pre/post-test | Bank soal PG per pelatihan; pre & post memakai soal yang sama; 1× percobaan, berbatas waktu, hanya dalam jendela yang dibuka admin; dinilai otomatis | MVP |
| F8 | Penutupan & sertifikat | Pratinjau kelulusan → tutup pelatihan (mengunci data) → sertifikat PDF + QR ke halaman verifikasi publik; admin dapat mencabut sertifikat | MVP |
| F9 | Laporan | Ekspor Excel & PDF: rekap kehadiran (JP), pre/post + peningkatan, nilai tugas, kelulusan | MVP |
| — | TTE BSrE · notifikasi email/WA · kuesioner evaluasi · dasbor lintas tahun | — | Nanti |

Aturan kelulusan (definisi istilah: `docs/04_DOMAIN_MODEL.md`): **Lulus = persentase kehadiran ≥ ambang kehadiran DAN nilai post-test ≥ ambang post-test.** Nilai tugas dilaporkan, tetapi tidak menentukan kelulusan.

## User story (MVP)
- **F1** Sebagai admin, saya ingin membuat pelatihan dengan ambang lulusnya sendiri agar tiap angkatan bisa punya aturan berbeda.
- **F2** Sebagai admin, saya ingin mengimpor daftar peserta dari Excel agar tidak perlu membuat akun satu per satu.
- **F3** Sebagai admin, saya ingin memecah JP materi ke beberapa sesi dan menugaskan narasumber agar jadwal sesuai kurikulum. Sebagai peserta, saya ingin melihat jadwal sesi agar tahu kapan dan di mana harus hadir.
- **F4** Sebagai narasumber, saya ingin mencatat kehadiran di sesi saya agar JP peserta terhitung akurat.
- **F5** Sebagai peserta, saya ingin mengunduh bahan ajar agar bisa belajar ulang.
- **F6** Sebagai peserta, saya ingin mengunggah tugas sebelum tenggat. Sebagai narasumber, saya ingin menilainya di satu layar.
- **F7** Sebagai peserta, saya ingin mengerjakan pre/post-test agar peningkatan pemahaman saya terukur.
- **F8** Sebagai admin, saya ingin melihat pratinjau kelulusan lalu menutup pelatihan agar sertifikat terbit otomatis. Sebagai pihak luar, saya ingin memindai QR sertifikat agar bisa memastikan keasliannya.
- **F9** Sebagai admin, saya ingin mengekspor laporan akhir agar bisa dilaporkan ke pimpinan.

## Non-fungsional
- Layak dipakai di HP lebar 360 px (peserta) dan desktop (admin), sesuai `docs/26_UI_CONVENTIONS.md`.
- Impor 500 baris dan pembuatan 100 sertifikat berjalan di queue tanpa timeout request.
- Bahasa antarmuka Indonesia, zona waktu WITA (`Asia/Makassar`).
