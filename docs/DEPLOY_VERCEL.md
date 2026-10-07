# Deploy SANJARA HADIR di Vercel

## 1. Mengapa aplikasi sebelumnya tidak muncul?

Aplikasi menggunakan **Flask** dengan entry point `app.py`. Vercel mengenali Flask otomatis, tetapi filesystem deployment hanya bisa ditulis di `/tmp`. Kode sebelumnya menginisialisasi SQLite di `data/absensi.sqlite3` pada saat import sehingga bisa memunculkan galat fungsi saat server mulai.

Pada versi ini, saat runtime Vercel terdeteksi, path sementara dipindahkan ke `/tmp/sanjara-preview/`. Aset CSS disalin ke `public/static/` sesuai pola static assets Vercel. `vercel.json` mengoptimalkan bundle; **tidak perlu** override Build Command, output directory, atau memilih framework Next.js.

## 2. Cara menampilkan aplikasi

1. Buka https://vercel.com/new
2. Import repo `smpssanegerijenggrong-cmyk/absensi` dari GitHub.
3. Pilih **Root Directory: ./ (root repository)** dan **Framework Preset: Other/Flask (deteksi otomatis)**, **jangan Next.js**.
4. Biarkan **Build Command** dan **Output Directory** kosong/default.
5. Deploy dari branch `main`; cek tab **Deployments** untuk status build.
6. Buka URL yang Vercel berikan. Halaman `/` menampilkan dashboard SANJARA HADIR sebagai **PRATINJAU saja**; `/health` mengembalikan JSON status proses.

Jika project Vercel sudah pernah diimpor, setelah push GitHub terdeteksi, buka **Deployments → Redeploy**. Periksa **Settings → Git → Production Branch = main**, **Settings → General → Root Directory = ./ / kosong**, dan build logs bila masih gagal.

## 3. Penting: data absensi siswa **belum aman** di Vercel

Di Vercel, `/tmp` bukan storage permanen. Proyek ini secara sengaja **menolak POST** ke data siswa, QR, izin, pengaturan, dan laporan yang mengubah data; statusnya **HTTP 503**, bukan sukses palsu. Dashboard dan navigasi GET tersedia sebagai demo publik dengan database kosong lokal instance. Tidak boleh memasukkan data asli siswa ke mode Vercel saat ini.

Untuk sistem absensi sekolah yang siap produksi, kita perlu:
- Database persisten (misalnya PostgreSQL terkelola) alih-alih SQLite filesystem.
- Penyimpanan privat surat izin (object storage seperti Vercel Blob/S3 atau Drive dengan izin akses benar).
- Autentikasi admin yang kuat, kontrol akses, HTTPS, dan audit pencatatan.
- Penyesuaian backend dan tes end-to-end untuk DB dan storage baru.

**Jangan** cukup menambahkan `DATABASE_PATH=/tmp/...` atau `UPLOAD_DIR=/tmp/...` untuk absensi resmi: data akan hilang saat instance dihapus/diganti.

Untuk pengujian lokal persisten, tetap gunakan Docker Compose atau Python sesuai README utama.

## 4. Variabel lingkungan

Untuk mode pratinjau publik: tidak wajib mengatur kredensial; dashboard dapat dilihat tanpa login, namun tidak menyimpan data. Untuk menguji login admin lokal/di Vercel, atur:
- `SECRET_KEY`: string acak minimal 32 karakter (secret, jangan commit).
- `ADMIN_USERNAME`: username admin.
- `ADMIN_PASSWORD`: password unik yang kuat.
- `COOKIE_SECURE=1`: HTTPS (pada Vercel akan selalu dipaksa).

Masukkan env var melalui **Vercel Project → Settings → Environment Variables**. Jangan menyimpan `.env` atau data siswa di GitHub.

## 5. Diagnosis

- **FUNCTION_INVOCATION_FAILED**: cek Functions logs. Pastikan proyek menggunakan revision dengan `/tmp` yang writable.
- **404 / NOT_FOUND**: pastikan Root Directory menunjuk ke root repository yang memiliki `app.py` dan `requirements.txt`.
- **Blank / CSS tidak tampil**: cek `/static/work.css` dan `/static/style.css`; kedua file ditempatkan di `public/static/`.
- **Masuk ke halaman login**: `/login` memerlukan `ADMIN_PASSWORD` untuk autentikasi, tetapi pratinjau dashboard dapat dibuka di `/`.
- **Tidak bisa absen / import**: memang dinonaktifkan dalam mode pratinjau agar data siswa tidak hilang.

Dokumentasi resmi Flask: https://vercel.com/docs/frameworks/backend/flask
