# Referensi aplikasi asli — SANJARA HADIR (ChatGPT Work)

**Aplikasi asli di Work**: https://sanjara-absensi.smpssanegerijenggron.chatgpt.site

**Status migrasi: BELUM TERPINDAH**. Dokumen ini merupakan referensi versi aplikasi yang telah dibuat di ChatGPT Work, **bukan** kode sumber yang diekspor dari Work. Isi `app.py` dan `templates/` pada repository ini adalah implementasi Flask terpisah yang sebelumnya dibuat dari spesifikasi, **bukan salinan asli proyek Work**.

## Tampilan yang dikenali dari proyek Work
Judul tampilan: **SANJARA HADIR · SCHOOL EDITION**; subjudul **Ruang Sekolah — Absensi murid**.

Navigasi:
- Ringkasan
- Absensi harian
- Data siswa
- Data kelas
- ID card siswa
- Surat izin
- Rekap absensi

Ringkasan utama: “Satu murid, satu kehadiran”; hadir hari ini, total murid, izin/sakit, belum tercatat; catatan status waktu WIB; lokasi sekolah wajib disimpan sebelum scan; input QR murid dari perangkat petugas yang berada dalam radius lokasi sekolah. Di tabel kehadiran: Murid, Kelas, Status, Waktu (WIB), Metode, Keterangan/Bukti.

## Ketentuan fitur yang telah diminta
1. **QR saja** — tanpa swafoto/pengenalan wajah.
2. Scan QR otomatis mencatat absensi siswa.
3. Validasi GPS lokasi sekolah.
4. Batas tepat waktu **07:00:00 WIB**; mulai **07:00:01 WIB** termasuk terlambat; waktu scan harus terlihat dalam monitoring dan rekap detail.
5. Impor data siswa/kelas, ID card, surat izin/sakit dengan unggahan berkas dan alasan khusus.
6. Rekap Sakit, Ijin, Alpa mengikuti contoh `ABSENSI(2).xlsx`, laporan yang dapat diunduh, dan backup Google Drive.

## Untuk menyalin aplikasi Work **secara tepat**
Dibutuhkan **kode sumber asli** dari proyek Work (arsip ZIP atau folder proyek lengkap yang berisi kode dan aset, termasuk manifest seperti package.json/requirements, tanpa kredensial atau data pribadi siswa). Proyek Work dengan akses khusus tidak menyediakan source code melalui snapshot Library ini; file Library yang terlihat hanyalah teks tampilan antarmuka.

Setelah kode asli tersedia, unggah file tersebut ke repository dengan review perubahan dahulu; pertahankan nama aplikasi, desain, fitur, dan struktur proyek aslinya. Jangan mengganti aplikasi asli dengan kode Flask di repository ini lalu menyebutnya hasil ekspor.
