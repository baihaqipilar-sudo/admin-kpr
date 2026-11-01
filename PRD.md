# PRD — Admin KPR PT Baihaqi Pilar Kreasi

## Original problem statement
Aplikasi web internal untuk mencatat dan memantau progres KPR setiap konsumen (pengganti Excel). Perbaikan yang diminta: (A) hapus field NIK, tempat lahir, tanggal lahir, alamat dari seluruh aplikasi; (B) master data dinamis Bank, Proyek, Tipe, Blok dengan "+ Tambahkan Baru" inline, Tipe/Blok mengikuti Proyek, cegah nama kembar, nonaktifkan tanpa hapus (Head Admin), satu sumber untuk semua dropdown/filter; (C) perbaikan Jeda SLA (tombol hanya untuk tahap ber-SLA yang berjalan, Lanjutkan saat dijeda, alasan + follow up wajib, riwayat jeda, peringatan). Jangan hapus 5 konsumen contoh; jangan ubah tahapan, progres, SLA. (Lihat spesifikasi lengkap di percakapan awal.)

## Architecture
FastAPI + MongoDB (server.py, logic.py, reports.py, seed.py); React + Tailwind + shadcn (context/AppContext, components/kpr, pages). Detail di /app/ARSITEKTUR.md.

## User personas
- Admin: kelola data KPR, tambah master data lewat dropdown.
- Head Admin: + Pengaturan, Riwayat/arsip, Laporan, ubah/nonaktifkan master data.

## Core requirements (static)
13 tahap berurutan + PBB, progres, tahap saat ini, SLA hari kerja/kalender + libur, jeda SLA, alasan keterlambatan, cancel, berkas, BI Checking, Tabel Ringkas, Database Lengkap, Detail Konsumen, Pengaturan, Riwayat, Laporan Excel/PDF, Bahasa Indonesia, DD/MM/YYYY, WIB.

## What's been implemented
- 2026-10-30: Repo was an empty template, so the full Phase 1 app was built with fixes A/B/C applied. 5 sample consumers seeded. Testing iteration 1: backend 20/20, frontend flows pass; fixed sheet close button, mobile overflow, PDF escaping, pause/resume date validation, readable cancel audit.
- 2026-10-30 (2): E — Konsumen module restyled (navy + teal accent, 15px body, 13px uppercase headers, uniform 56px rows, 13px badges, icon summary cards, faded zero cards, sticky Nama/Unit, big sheet title + current stage badge, underline tabs, loading/empty states). F — "Riwayat" replaced by "Progres" module (backend/history.py service + backend/progres/; frontend/src/modules/progres/): Harian, Mingguan, Bulanan tabs for both roles, Log Detail for Head Admin, rule-based Kesimpulan, PDF/Excel export per period; Arsip Konsumen moved to Pengaturan; new setting stale_days (7). Tested iteration 2: all flows pass; fixed unknown preset 500, raw stage key in stale table, net-0 berkas line.

## Prioritized backlog
- P1 (Fase 2): password per akun, Head Admin tambah akun Admin.
- P2 (Fase 3): daftar "follow up hari ini", impor Excel lama.
- P2: session expiry (TTL).

## Next tasks
Konfirmasi checklist berkas Fix/Non Fix final dari user (saat ini daftar awal yang bisa diubah).
