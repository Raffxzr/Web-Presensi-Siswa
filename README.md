# Aplikasi Web Presensi Kelas

Aplikasi web presensi harian untuk **Siswa** dan **Admin Kelas** (Ketua Kelas, Sekretaris, serta Wali Kelas) yang dapat langsung dipakai dan disesuaikan oleh kelas atau sekolah mana saja.
Backend menggunakan **Flask + SQLite**, autentikasi penuh di sisi server, otorisasi berbasis peran, serta fitur tema gelap (*Dark Mode*), rekap mingguan, dan pencatatan cepat satu klik.

## 1. Fitur Utama

**Siswa**
- Login dengan nama lengkap + kata sandi. Wajib mengganti kata sandi sementara saat login pertama.
- Beranda: status hari ini, jendela waktu presensi + hitung mundur, rekap bulan ini (Hadir/Izin/Sakit/Alpa), dan riwayat terbaru.
- Presensi Hadir / Izin / Sakit (Izin & Sakit wajib keterangan). Satu kali per hari, mengikuti jadwal sesi dan verifikasi lokasi GPS sekolah (bila diaktifkan).
- Riwayat per bulan beserta tanda "dikoreksi admin" bila catatan diubah oleh pengurus kelas.

**Admin Kelas (Ketua Kelas, Sekretaris & Wali Kelas)**
- Login memakai Nama akun admin + Kata sandi + **Kunci akses admin** (`ADMIN_ACCESS_KEY`) yang diperiksa di server.
- **Tombol Cepat "Tandai Semua Hadir" & Ubah Cepat di Tabel**:
  - Tekan **Tandai Semua Hadir** untuk mencatat seluruh siswa aktif yang belum presensi menjadi **Hadir (`H`)** sekaligus.
  - Ubah langsung status siswa yang tidak masuk (**Izin `I`**, **Sakit `S`**, atau **Alpa `A`**) beserta keterangannya pada baris tabel di halaman Kelola Presensi dan klik **Simpan** tanpa harus berpindah halaman satu per satu.
- **Laporan Harian, Mingguan, dan Bulanan**:
  - **Harian**: Rekap per tanggal.
  - **Mingguan**: Rekap per minggu (**Senin s.d. Minggu**) dengan matriks status harian (`Sen`–`Jum`/`Min`), jumlah H/I/S/A/Belum, dan `% Hadir` per siswa.
  - **Bulanan & Riwayat Siswa**: Rekap bulanan dan riwayat per siswa.
  - Seluruh laporan dilengkapi tampilan cetak dan unduhan **CSV** serta **Excel (`.xlsx`)**.
- **Data Siswa & Pengaturan**: Tambah/edit siswa, aktif/nonaktif akun, reset kata sandi sementara siswa, ubah **Nama Kelas & Nama Sekolah**, atur jadwal sesi per hari, verifikasi lokasi GPS, dan tahun ajaran.
- **Logo Kelas & Switch Tema Gelap / Terang (*Dark Mode*)** di seluruh halaman (logo dapat diganti pada berkas `app/static/img/logo-kelas.png`).

## 2. Menjalankan Secara Lokal
Buka Terminal/CMD di lokasi folder

```bash
# linux : python -m venv .venv && source .venv/bin/activate        
# Windows : .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
```

Edit `.env`:
```env
APP_ENV=development
ADMIN_ACCESS_KEY=(Contoh: 123456789987654321)
SCHOOL_NAME=(Contoh: SMA Negeri 1 JMK)
CLASS_NAME=(X JMK 1)
```

Edit daftar siswa pada `data/daftar_siswa.md` sesuai nama siswa di kelas Anda, lalu inisialisasi database, buat akun admin pengurus kelas, dan impor daftar siswa:
```bash
python manage.py init-db
python manage.py create-admin --name "Wali Kelas"
python manage.py create-admin --name "Ketua Kelas"
python manage.py create-admin --name "Sekretaris"
python manage.py import-students data/daftar_siswa.md
```
Run web secara lokal
```bash
flask --app wsgi run
```

> **Tips Kustomisasi:** Nama Kelas, Nama Sekolah, dan Tahun Ajaran juga dapat diedit kapan saja langsung melalui halaman web pada menu **Pengaturan** di bagian Admin.
