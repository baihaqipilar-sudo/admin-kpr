# Backup Admin KPR — PT Baihaqi Pilar Kreasi

Tanggal backup: 30/10/2026

## Isi arsip `admin-kpr-backup-2026-10-30.zip`
- `backend/` — FastAPI (server.py, logic.py, reports.py, seed.py, history.py, progres/, requirements.txt)
- `frontend/` — React (src/, public/, package.json, tailwind.config.js, dll.; tanpa node_modules)
- `data/` — ekspor database (JSON): users, settings, master, consumers, audit
- `README.md`, `ARSITEKTUR.md`, `PRD.md`, `BACKUP.md`

File `.env` tidak disertakan. Buat ulang:
- `backend/.env`: `MONGO_URL="mongodb://localhost:27017"`, `DB_NAME="nama_database"`, `CORS_ORIGINS="*"`
- `frontend/.env`: `REACT_APP_BACKEND_URL=https://alamat-backend-anda`

## Cara memulihkan
1. Backend: `cd backend && pip install -r requirements.txt && uvicorn server:app --host 0.0.0.0 --port 8001`
2. Frontend: `cd frontend && yarn install && yarn start` (atau `yarn build` untuk produksi)
3. Impor data (jalankan sebelum backend pertama kali dinyalakan agar data contoh tidak dibuat ulang):
```
for c in users settings master consumers audit; do
  mongoimport --uri "$MONGO_URL" --db "$DB_NAME" --collection $c --jsonArray --file data/$c.json
done
```
Login: pilih akun Admin atau Head Admin (tanpa kata sandi).

## Jumlah data saat backup
users 2 · settings 1 · master 21 · consumers 6 · audit 39
