# SANJARA ABSENSI

Aplikasi absensi QR Code siswa SMP SSA Negeri Jenggrong Ranuyoso. **Repository ini berisi kode aplikasi, bukan data siswa**.

## Fitur
- Login admin; tidak ada kata sandi default.
- Data siswa dan kelas, impor Excel, ID card dengan QR unik.
- Scan QR dari kamera guru/petugas + verifikasi GPS area sekolah.
- Jam absensi memakai **waktu server Asia/Jakarta**: sampai **07:00:00 WIB** tepat waktu, mulai **07:00:01 WIB** terlambat; tersimpan waktu, detik, dan menit keterlambatan.
- Permohonan sakit/izin beserta surat PDF/JPG/PNG dan persetujuan admin.
- Monitoring kelas, rekap bulanan, ekspor Excel dengan header persis seperti contoh: **No, NIPD, NISN, NAMA, JENIS KELAMIN, KELAS, SAKIT, IJIN, ALPA, JUMLAH (SAKIT/IJIN/ALPA)**.
- Rekap detail waktu scan di sheet Excel terpisah, unduh arsip cadangan ZIP dan backup ke Google Drive melalui service account bila dikonfigurasi.

## Jalankan
Python 3.11+ direkomendasikan.
```bash
python -m venv .venv
# Linux/Mac: source .venv/bin/activate
# Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env    # isi variabelnya, lalu ekspor ke environment (atau gunakan Docker Compose)
python app.py
```
Buka http://localhost:5000. Untuk browser HP dan kamera/GPS, **gunakan HTTPS** di deployment. Jalankan lebih mudah dengan Docker Compose:
```bash
cp .env.example .env
# isi SECRET_KEY dan ADMIN_PASSWORD di .env
docker compose up --build -d
```
Untuk deployment produksi pastikan URL menggunakan HTTPS, volume `./data:/app/data` persisten, dan variabel `COOKIE_SECURE=1` diset. Jangan unggah `.env`, DB, backup, atau surat ke GitHub.

### Pengaturan GPS
Masuk Admin → Pengaturan. Set koordinat sekolah (lintang/bujur) dan radius di meter. **Absensi tidak bisa dicatat sebelum GPS sekolah dikonfigurasi**. Ponsel/komputer petugas scan harus memberikan izin lokasi. QR di ID card menyimpan kode acak, bukan NISN/NIPD. GPS browser tidak anti-pemalsuan lokasi; gunakan petugas sekolah sebagai operator perangkat scan untuk kontrol tambahan.

### Pengaturan waktu dan ALPA
Batas awal `07:00` WIB, akhir hari untuk ALPA `15:00` WIB. Status ALPA di laporan dihitung hanya pada tanggal hari sekolah yang sudah ditutup, sejak tanggal siswa didaftarkan, mengecualikan hari libur yang ditandai. Sesuaikan hari sekolah dan libur dari Pengaturan. Siswa terlambat tetap **HADIR** dan tidak dihitung ALPA.

### Format ekspor
- **REKAP**: kolom G-I menunjukkan SAKIT/IJIN/ALPA pada **tanggal yang dipilih**, sedangkan kolom J-L menunjukkan **JUMLAH SAKIT/IJIN/ALPA selama bulan yang dipilih**, meniru susunan dua baris header pada contoh.
- **DETAIL WAKTU**: satu baris per absensi, berisi tanggal, nama, kelas, jam scan WIB, status, sumber, menit terlambat.
- **LEGENDA** dan **KALENDER**: penjelasan dan hari sekolah/libur.
- Template impor siswa memakai baris header `NIPD, NISN, NAMA, JENIS KELAMIN, KELAS`, tidak memakai file format rekap yang berisi dua baris judul.

### Integrasi Google Drive (opsional)
Tambahkan `GOOGLE_SERVICE_ACCOUNT_JSON` (isi JSON service account, **hanya di environment server**) dan `DRIVE_FOLDER_ID`, lalu berikan akses folder Drive ke alamat service account yang sesuai. Tombol 'Backup ke Drive' akan mengunggah ZIP data pada saat itu. Belum ada jadwal backup otomatis; untuk otomatisasi, gunakan scheduler milik server dan endpoint autentikasi yang aman. **Service account tidak otomatis mendapatkan akses ke My Drive milik admin.**

### Pengujian
```bash
pytest -q
```

## Catatan privasi dan keamanan
Data siswa, lokasi petugas scan, dan surat izin berada di penyimpanan server privat, bukan pada repository publik. Konfigurasi SECRET_KEY/ADMIN_PASSWORD yang panjang dan unik wajib untuk produksi. Kode QR fisik dapat disalin; pelaksanaan scan sebaiknya diawasi petugas. Ini versi awal operasional yang membutuhkan pengujian kamera/GPS langsung di lingkungan sekolah sebelum penggunaan resmi.
