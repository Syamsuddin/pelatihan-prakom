# 09 — Stack
Pemilik daftar teknologi & versi. Menambah/mengganti dependensi = perubahan lewat `docs/22_CHANGE_POLICY.md`.

| Lapisan | Teknologi | Versi |
|---|---|---|
| Bahasa | PHP | 8.3 |
| Framework | Laravel | 12.x |
| UI reaktif | Livewire (class-based) | 3.x |
| Interaksi kecil sisi klien | Alpine.js (bawaan Livewire) | ikut Livewire |
| CSS | Tailwind CSS (plugin `@tailwindcss/vite`) | 4.x |
| Build aset | Vite + Node.js | Vite 6 / Node 22 |
| Basis data | MySQL (produksi), SQLite in-memory (tes) | 8.0 / 3 |
| Tes | Pest (+ `pestphp/pest-plugin-laravel`, `pest-plugin-livewire`) | 3.x |
| Format kode | Laravel Pint | bawaan |
| Excel | `maatwebsite/excel` | 3.1 |
| PDF | `barryvdh/laravel-dompdf` | 3.x |
| QR | `simplesoftwareio/simple-qrcode` (keluaran SVG) | 4.x |
| Ikon | Heroicons outline (SVG inline di `resources/views/components/icon/`) | 2.x |
| Font | Inter (self-host, `public/fonts/`) | 4.x |
| Server | Ubuntu 24.04, Nginx, PHP-FPM 8.3, Supervisor, cron | — |

## Teknologi terlarang
| Teknologi | Alasan |
|---|---|
| jQuery | Alpine/Livewire sudah mencukupi |
| Bootstrap / pustaka CSS lain | Bentrok dengan token Tailwind di `docs/26_UI_CONVENTIONS.md` |
| React / Vue / Inertia / SPA apa pun | Arsitektur server-rendered Livewire (`docs/08_ARCHITECTURE.md`) |
| Filament / Nova / Backpack / admin generator | UI & otorisasi dikendalikan sendiri |
| Livewire Volt & Flux UI | Satu gaya komponen class-based + komponen `x-ui.*` sendiri |
| spatie/laravel-permission | Peran tetap 3, berbasis kolom `role` + Policy |
| CDN eksternal untuk JS/CSS/font | Semua aset dibundel Vite/self-host |
