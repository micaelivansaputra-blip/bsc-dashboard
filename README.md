# BSC Scorecard Cinema

Dashboard Balance Scorecard per cinema (bulanan + YTD) untuk Cinema Manager.

## Isi folder
- `index.html` — dashboard (HTML + CSS + JS, tanpa build step)
- `data.enc.js` — data BSC yang sudah dienkripsi (AES-256). Tanpa kode akses, isinya tidak bisa dibaca.
- `scripts/` — skrip untuk membuat dan mengenkripsi data bulanan

`data.js` (data mentah, tidak terenkripsi) sengaja tidak di-upload ke repo; lihat `.gitignore`.

## Fitur
Tampilan **Cinema**:
- Filter Region, Cinema, Periode (bulan atau YTD)
- Total Score, rank nasional & rank region
- 6 KPI: Ops Audit (30%), SPH (20%), GSAT (20%), % OI (15%), % Payroll (10%), % Electricity (5%)
- Tren Total Score vs median nasional, skor per region, tabel peringkat nasional
- Unduh PNG scorecard dan Excel (Scorecard, Riwayat Bulanan, Peringkat)

Tampilan **Region**:
- Total Score region, rank dari 6 region, selisih vs tahun lalu
- Cinema teratas & terbawah
- Pencapaian 6 KPI (skor vs budget + skor tahun lalu)
- Peta Ops Income per provinsi (warna = aktual vs budget) + tabel
- Radar Total Score per cinema (vs budget & vs tahun lalu)
- Tabel skor KPI per cinema, toggle vs Budget / vs Tahun Lalu; klik baris → detail cinema
- Unduh PNG dan Excel (Ringkasan, Skor vs Budget, Skor vs Tahun Lalu, OI per Provinsi)

## Deploy ke GitHub Pages
1. Buat repo baru, upload `index.html` dan `data.enc.js`.
2. Settings → Pages → Source: `Deploy from a branch`, branch `main`, folder `/root`.
3. Buka `https://<username>.github.io/<repo>/`.

## Update data bulanan
1. Isi bulan baru di file raw (`New_BSC_Raw.xlsx`, sheet Cinema).
2. Jalankan (butuh Python 3):
   ```
   pip install openpyxl cryptography
   python scripts/build_data.py New_BSC_Raw.xlsx data.js data.js "2026-10 (closing Oktober 2026)"
   python scripts/encrypt_data.py data.js data.enc.js "KODE-AKSES"
   ```
   `build_data.py` menghitung LY, skor, Total Score, rank, region, dan YTD dengan aturan yang sama seperti Google Sheet. `encrypt_data.py` mengunci hasilnya dengan kode akses.
3. Upload hanya `data.enc.js` yang baru ke repo.

Cinema baru harus ditambahkan dulu ke daftar `cinemas` di `data.js` (kode, nama, alias, region, provinsi).

## Kode akses
- Dashboard minta kode akses saat dibuka; kode diingat sampai tab ditutup.
- Ganti kode: jalankan lagi `encrypt_data.py` dengan kode baru, lalu upload `data.enc.js`. Kode lama otomatis tidak berlaku.
