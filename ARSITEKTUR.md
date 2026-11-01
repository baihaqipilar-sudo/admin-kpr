# Arsitektur Admin KPR

## Backend (FastAPI + MongoDB)
- `server.py` — rute API, autentikasi pilih akun (token sesi), validasi, Riwayat Perubahan.
- `logic.py` — perhitungan otomatis: tahap saat ini, progres, SLA (hari kerja/kalender + hari libur), jeda, peringatan, PBB, status konsumen.
- `reports.py` — penyusunan laporan, ekspor Excel (openpyxl) dan PDF (reportlab).
- `seed.py` — akun Admin/Head Admin, pengaturan awal, master data awal, 5 konsumen contoh (hanya bila kosong).

### Koleksi
- `users` — `{id, name, role: admin|head_admin, password_hash: null}` (siap untuk kata sandi Fase 2).
- `sessions` — `{token, user_id}`.
- `settings` (`id: main`) — stages `[{key, name, sla_days, sla_kind, sla_ref}]`, pbb_name, status_labels, checklist {fix, nonfix}, delay_reasons, cancel_reasons, slik_options, holidays, warn_days, max_pause_days.
- `master` — `{id, kind: bank|project|type|block, name, project_id (type/block), active}`. Tidak pernah dihapus permanen; nama unik per daftar (per proyek untuk tipe/blok), tanpa beda huruf besar-kecil/spasi.
- `consumers` — data konsumen (project_id, type_id, block_id, bank_id merujuk `master`), `stages.{key}` = `{status, date, notes[], pauses[], late_reason, late_note}`, berkas, slik_result, loan_note, cancel, archived.
- `audit` — `{ts, user, entity, label, field, old, new}`.

### Aturan inti
- Tahap saat ini = tahap berurutan pertama yang belum Lolos/Selesai/Tidak Perlu. Progres = Selesai ÷ (jumlah tahap − Tidak Perlu). PBB di luar hitungan.
- SLA: mulai dari acuan (`prev` = tanggal selesai tahap sebelumnya, `booking`, `stage:<key>`). Tenggat disesuaikan = tenggat awal + hari jeda (hari kerja).
- Jeda SLA hanya bila tahap ber-SLA, tenggat terhitung, status Belum/Proses. Jeda aktif otomatis ditutup saat status menjadi Selesai/Tidak Perlu/Cancel.
- Tahap Cancel → konsumen Cancel. Akad Kredit Selesai → konsumen Akad.

## Frontend (React + Tailwind + shadcn/ui)
- `context/AppContext.js` — akun, pengaturan, master data (satu sumber untuk semua dropdown & filter).
- `components/kpr/MasterSelect.jsx` + `AddInline.jsx` — dropdown master dengan "+ Tambahkan Baru" inline.
- `pages/` — ConsumersPage (Tabel Ringkas / Database Lengkap), SettingsPage (termasuk tab Arsip Konsumen), ReportsPage, LoginPage.
- `modules/progres/` — modul Progres: `ProgresPage` (tab Harian, Mingguan, Bulanan, Log Detail khusus Head Admin), `DailyTab`, `PeriodTab`, `LogTab`; semua teks antarmuka di `texts.js`.
- Gaya modul Konsumen dibatasi kelas `.kpr-module` di `App.css` (navy utama, aksen teal, tinggi baris 56px).

## Modul Progres
- `backend/history.py` — layanan riwayat: `record()` (dipakai semua pencatatan audit, cara pencatatan tidak berubah), `fetch()` (ambil riwayat per rentang tanggal WIB), `last_activity()` (pergerakan terakhir per konsumen). `/api/audit` (Log Detail) juga memakai layanan ini.
- `backend/progres/` — modul terpisah:
  - `service.py` membaca riwayat lewat `history`, mengubah entri menjadi peristiwa (status, tanggal, catatan, jeda, berkas, dll.), menggabungkan perubahan berulang (ambil yang terakhir; berkas per konsumen), menghitung angka harian/periode, perbandingan periode sebelumnya, konsumen mandek (`settings.stale_days`, awal 7), dan "Masih perlu perhatian".
  - `texts.py` — template kalimat dan Kesimpulan (berbasis aturan, tanpa AI).
  - `router.py` — `GET /api/progres/daily?date=`, `GET /api/progres/period?preset=minggu_ini|minggu_lalu|bulan_ini|bulan_lalu|kustom&start=&end=`, `GET /api/progres/export/{excel|pdf}?start=&end=` (memakai `reports.py`). Terbuka untuk Admin & Head Admin.
- Status keterlambatan/tepat waktu diambil dari perhitungan SLA saat ini (`logic.compute`).
