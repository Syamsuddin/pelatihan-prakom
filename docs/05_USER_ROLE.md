# 05 — User Role
Pemilik peran & matriks akses. Penegakan: Policy Laravel (`docs/21_SECURITY_RULES.md`).

## Peran
| Peran | `users.role` | Ringkas |
|---|---|---|
| Admin | `admin` | Panitia penyelenggara; akses semua pelatihan |
| Narasumber | `narasumber` | Pengajar; akses terbatas pada **materi yang ditugaskan kepadanya** |
| Peserta | `peserta` | Akses terbatas pada **pelatihan tempat ia terdaftar** dan data miliknya sendiri |

## Matriks akses
| Kapabilitas | Admin | Narasumber | Peserta |
|---|---|---|---|
| Buat/ubah pelatihan, ambang, penandatangan | ✓ | ✗ | ✗ |
| Kelola user (buat, nonaktifkan, reset password) | ✓ | ✗ | ✗ |
| Impor/tambah/keluarkan peserta (status draft/berjalan) | ✓ | ✗ | ✗ |
| Kelola materi & sesi, tugaskan narasumber | ✓ | ✗ | ✗ |
| Lihat jadwal pelatihan | ✓ semua | ✓ materinya | ✓ pelatihannya |
| Input/ubah presensi (pelatihan berjalan) | ✓ semua sesi | ✓ sesi materinya | ✗ |
| Lihat rekap kehadiran | ✓ semua | ✓ materinya | ✓ miliknya |
| Unggah/hapus bahan ajar | ✓ | ✓ materinya | ✗ |
| Unduh bahan ajar | ✓ | ✓ materinya | ✓ pelatihannya |
| Buat tugas & menilai | ✓ | ✓ materinya | ✗ |
| Kumpulkan tugas (sebelum tenggat, belum dinilai) | ✗ | ✗ | ✓ miliknya |
| Kelola bank soal & jendela tes | ✓ | ✗ | ✗ |
| Kerjakan pre/post-test | ✗ | ✗ | ✓ miliknya, 1× per jenis |
| Pratinjau kelulusan & tutup pelatihan | ✓ | ✗ | ✗ |
| Terbitkan ulang / cabut sertifikat | ✓ | ✗ | ✗ |
| Unduh sertifikat | ✓ semua | ✗ | ✓ miliknya, bila lulus & tidak dicabut |
| Ekspor laporan | ✓ | ✓ rekap materinya | ✗ |
| Halaman verifikasi sertifikat | publik (tanpa login) | | |

Aturan lintas peran:
- Setelah status `closed`: semua peran **hanya baca**. Pengecualian: admin dapat mencabut atau menerbitkan ulang sertifikat.
- User nonaktif (`is_active=false`) tidak bisa login.
- Semua peran wajib mengganti password pada login pertama.
