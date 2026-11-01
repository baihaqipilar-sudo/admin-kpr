# Admin KPR — PT Baihaqi Pilar Kreasi

Aplikasi web internal untuk mencatat dan memantau progres KPR setiap konsumen (pengganti Excel database progres konsumen).

## Masuk
Pilih akun di halaman masuk (tanpa kata sandi):
- **Admin** — akses penuh data KPR, bisa menambah Bank/Proyek/Tipe/Blok lewat dropdown "+ Tambahkan Baru".
- **Head Admin** — semua akses Admin + Pengaturan, Riwayat Perubahan (dan arsip), Laporan Excel/PDF.

## Fitur utama
- Tabel Ringkas & Database Lengkap (header dan kolom Nama/Unit menempel), filter proyek/bank/tahap/status/penghasilan, pencarian.
- Detail Konsumen (panel samping): tab Data (+ BI Checking), Tahapan (status, tanggal, SLA, jeda, catatan, alasan keterlambatan), Berkas.
- 13 tahap berurutan + PBB (fleksibel, wajib, tanpa SLA, peringatan "PBB belum ada").
- SLA per tahap (hari kerja/kalender, acuan mulai), hari libur, Jeda SLA/Lanjutkan, tenggat awal & disesuaikan, waktu bersih.
- Tombol Jeda SLA hanya muncul bila tahap punya SLA, tenggat sudah bisa dihitung, dan status Belum/Proses.
- Master data dinamis Bank, Proyek, Tipe, Blok (Tipe/Blok terikat Proyek), cegah nama kembar, nonaktifkan tanpa hapus permanen.
- Arsip (hapus = arsip, bisa dipulihkan), Riwayat Perubahan otomatis (WIB).
- Laporan: ringkasan, database lengkap, keterlambatan SLA, cancel/ditolak — unduh Excel & PDF.

Field NIK, tempat/tanggal lahir, dan alamat tidak dipakai lagi di form, detail, tabel, maupun laporan.

## Menjalankan
- Backend: FastAPI (`backend/server.py`) di port 8001, semua rute berawalan `/api`. MongoDB via `MONGO_URL` & `DB_NAME`.
- Frontend: React (`frontend/`), memakai `REACT_APP_BACKEND_URL`.
- Data contoh (5 konsumen berlabel "contoh") dibuat otomatis saat database kosong.

Lihat `ARSITEKTUR.md` untuk detail struktur.
