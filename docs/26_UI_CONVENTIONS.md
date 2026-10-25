# 26 — UI Conventions
Rumah tunggal fakta antarmuka. Nilai di luar tabel token **TERLARANG**. Bila token tidak menjawab kebutuhan → berhenti & tanya.

## Jangkar
Tailwind CSS 4 dengan preset netral *slate* + satu aksen *teal*. Token didefinisikan sekali di blok `@theme` pada `resources/css/app.css` dan dipakai lewat kelas utilitas (`bg-primary`, `text-muted`, …).

## Token
| Kategori | Nama (`@theme`) | Nilai | Kelas |
|---|---|---|---|
| Warna | `--color-primary` | `#0F766E` | `bg-primary` `text-primary` `border-primary` |
| Warna | `--color-primary-hover` | `#115E59` | `hover:bg-primary-hover` |
| Warna | `--color-primary-soft` | `#CCFBF1` | latar badge/sorot aktif sidebar |
| Warna | `--color-bg` | `#F8FAFC` | latar halaman |
| Warna | `--color-surface` | `#FFFFFF` | kartu, tabel, modal |
| Warna | `--color-border` | `#E2E8F0` | garis, border input |
| Warna | `--color-text` | `#0F172A` | teks utama |
| Warna | `--color-muted` | `#475569` | teks sekunder, label |
| Warna | `--color-success` / `--color-success-soft` | `#15803D` / `#DCFCE7` | hadir, lulus, valid |
| Warna | `--color-warning` / `--color-warning-soft` | `#B45309` / `#FEF3C7` | izin, sakit, sementara |
| Warna | `--color-danger` / `--color-danger-soft` | `#B91C1C` / `#FEE2E2` | alpa, tidak lulus, dicabut, aksi hapus |
| Warna | `--color-info` / `--color-info-soft` | `#1D4ED8` / `#DBEAFE` | berjalan, informasi |
| Tipografi | `--font-sans` | `Inter, ui-sans-serif, system-ui, sans-serif` | |
| Tipografi | teks dasar | 14 px / 20 px, 400 | `text-sm` |
| Tipografi | teks kecil (label tabel, bantuan) | 12 px / 16 px, 500 | `text-xs` |
| Tipografi | H1 judul halaman | 24 px / 32 px, 600 | `text-2xl font-semibold` |
| Tipografi | H2 judul kartu/seksi | 18 px / 28 px, 600 | `text-lg font-semibold` |
| Tipografi | angka statistik | 30 px / 36 px, 600 | `text-3xl font-semibold` |
| Spasi | skala yang diizinkan | 4, 8, 12, 16, 24, 32 px | `1 2 3 4 6 8` |
| Kepadatan | sel tabel / tinggi input & tombol | padding 12×8 px / 40 px | `px-3 py-2` / `h-10` |
| Radius | `--radius-ui` | 8 px | `rounded-lg` (input, tombol, kartu, modal); badge `rounded-full` |
| Bayangan | satu-satunya | `shadow-sm` | kartu, dropdown, modal |
| Fokus | cincin fokus | 2 px `#0F766E`, offset 2 px | `focus-visible:ring-2 ring-primary ring-offset-2` |
| Ikon | Heroicons outline | 20 px (tombol/menu), 16 px (inline) | `x-icon.*` |
| Lebar konten | maksimum | 1280 px | `max-w-7xl` |

## Target perangkat & breakpoint
Desktop-first. **Wajib layak mulai 360 px.** Breakpoint Tailwind default: `sm` 640, `md` 768, `lg` 1024. Di bawah `lg`, sidebar menjadi drawer (tombol menu di topbar). Di bawah `md`, tabel lebar berubah menjadi daftar kartu (komponen `x-ui.table` varian `stacked`). Halaman peserta dirancang dahulu untuk 360 px.

## Navigasi
| Peran | Pola |
|---|---|
| Admin, Narasumber | Sidebar kiri 256 px (logo MyCORPU, menu) + topbar (nama user, Keluar) |
| Peserta | Topbar saja + daftar pelatihan sebagai beranda; menu tab di dalam halaman pelatihan |

## Inventaris komponen (`resources/views/components/ui/`)
| Komponen | Varian / props | Catatan |
|---|---|---|
| `x-ui.button` | `primary`, `secondary` (border), `danger`, `ghost`; `size` md/sm; `loading` | Teks kata kerja: Simpan, Batal, Hapus, Unggah, Unduh, Mulai, Tutup Pelatihan |
| `x-ui.input` `x-ui.select` `x-ui.textarea` `x-ui.file` | `label` wajib, `hint`, `error` (otomatis dari `$errors`) | Label di atas field |
| `x-ui.table` | `default`, `stacked` (<768 px); slot `empty` | Header `text-xs uppercase text-muted` |
| `x-ui.card` | `title`, `actions` slot | `bg-surface border rounded-lg shadow-sm p-6` |
| `x-ui.stat` | `label`, `value`, `hint` | Dasbor |
| `x-ui.badge` | status → warna: draft=netral, ongoing=info, closed=success; hadir=success, izin/sakit=warning, alpa=danger; passed=success, failed=danger, pending=netral; valid=success, dicabut=danger | Peta kelas lengkap (jebakan #7 di `docs/16_DEBUGGING_GUIDE.md`) |
| `x-ui.modal` | `size` sm/md/lg | Fokus terkunci, Esc menutup |
| `x-ui.confirm` | `title`, `message`, `confirm-text`, `require-text` (ketik untuk konfirmasi) | Wajib untuk setiap aksi destruktif |
| `x-ui.toast` | success/danger/info; listen event `toast` | Pojok kanan atas, hilang 5 detik |
| `x-ui.alert` | success/warning/danger/info | Pesan di dalam halaman |
| `x-ui.tabs` | item + aktif | Halaman detail pelatihan |
| `x-ui.empty-state` | `icon`, `title`, `message`, `action` slot | |
| `x-ui.skeleton` | `rows` | State memuat (`wire:loading`) |
| `x-ui.pagination` | — | 25 baris per halaman |

## Peta halaman → pola
| Halaman | Pola | Peran |
|---|---|---|
| Login, Ganti password | auth | semua |
| Dasbor (pelatihan berjalan, sesi hari ini, tugas menunggu dinilai) | dasbor | per peran |
| Daftar pelatihan | daftar+filter (status, tahun, cari) | admin |
| Form pelatihan | formulir | admin |
| Detail pelatihan, tab: Ringkasan · Peserta · Materi & Jadwal · Presensi · Tes · Kelulusan · Laporan | detail | admin |
| Impor peserta (+ hasil impor) | formulir | admin |
| Form materi & sesi | formulir (modal) | admin |
| Grid presensi sesi | formulir tabel (`Shared\AttendanceGrid`) | admin, narasumber |
| Bank soal | daftar + formulir (modal) | admin |
| Pratinjau kelulusan + Tutup | daftar + `x-ui.confirm require-text` | admin |
| Daftar sertifikat (+ cabut, terbitkan ulang) | daftar+filter | admin |
| Kelola user | daftar+filter + formulir | admin |
| Materi saya (bahan, tugas, penilaian) | detail | narasumber |
| Pelatihan saya, tab: Jadwal · Bahan · Tugas · Tes · Hasil & Sertifikat | detail | peserta |
| Kerjakan tes (1 soal per layar + navigasi nomor + sisa waktu) | formulir | peserta |
| Verifikasi sertifikat | detail (layout guest) | publik |
| Error 403/404/419/500/503 | auth (layout guest) | semua |

## Empat state wajib (setiap halaman/komponen ber-data)
| State | Wujud |
|---|---|
| Kosong | `x-ui.empty-state` dengan kalimat penjelas + aksi utama (mis. "Belum ada peserta. [Impor Peserta]") |
| Memuat | `x-ui.skeleton` / tombol `loading` (`wire:loading`) |
| Gagal | `x-ui.alert danger` atau toast danger (`docs/14_ERROR_HANDLING.md`) |
| Sukses | toast success "… berhasil disimpan." |

## Bahasa & mikroteks
Bahasa Indonesia, nada formal dinas, kalimat aktif & singkat. Sapaan "Anda". Tanggal `25 Okt 2026`, waktu `08.00 WITA`, angka desimal koma (`80,00%`). Satu istilah per konsep sesuai glosarium `docs/04_DOMAIN_MODEL.md` (mis. selalu "Materi", bukan kadang "Mata Pelatihan").

## Aksesibilitas dasar
Kontras pasangan token di atas lulus WCAG AA (teks `text`/`muted` di `bg`/`surface`; putih di `primary`/`danger`). Setiap input berlabel; fokus keyboard selalu terlihat; ikon tanpa teks wajib `aria-label`; status tidak hanya dibedakan warna (badge selalu berteks). Lanjutan: [TERBUKA] pasca-MVP.

## Sertifikat (PDF)
A4 lanskap, latar `#FFFFFF`, bingkai 2 px `#0F766E`, teks `#0F172A`, font Inter. Isi: judul "SERTIFIKAT", nomor, nama + NIP, judul pelatihan & angkatan, tanggal pelaksanaan, **total JP**, tempat/tanggal terbit, penandatangan, QR 96 px di kiri bawah. Layout memakai `<table>` (jebakan #4 di `docs/16_DEBUGGING_GUIDE.md`). [TERBUKA] latar/logo instansi.

## Larangan UI
- Nilai warna/ukuran/font di luar tabel token; kelas arbitrer (`bg-[#123456]`, `p-[13px]`).
- Gradien, glassmorphism, bayangan selain `shadow-sm`, animasi dekoratif (yang diizinkan hanya transisi 150 ms pada hover/fokus, skeleton pulse, dan toast).
- Emoji di antarmuka; ikon dari set selain Heroicons outline.
- Teks placeholder/lorem tersisa; istilah Inggris di UI (Save, Submit, Dashboard → Simpan, Kirim, Dasbor).
- Pustaka UI tambahan tanpa lewat `docs/22_CHANGE_POLICY.md`.
