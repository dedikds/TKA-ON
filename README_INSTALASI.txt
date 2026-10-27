PATCH PENGATURAN WAKTU SIMULASI TKA - GARUDA CBT
Tanggal patch: 10 Oktober 2026
Basis kode: arsip .idea.zip yang diberikan pengguna (CodeIgniter 3).

FITUR
1. Admin bisa mengatur durasi Simulasi TKA PER PAKET BANK SOAL.
2. Durasi khusus 1 sampai 600 menit. Angka 0 = mengikuti durasi CBT
   (cbt_jadwal.durasi_ujian; jika tidak tersedia gunakan 45 menit).
3. Pengaturan tersedia di ADMIN > CBT > Bank Soal, di sebelah tombol Simulasi ON/OFF.
4. Durasi terlihat di halaman konfirmasi peserta sebelum tombol Mulai.
5. Hitung mundur ujian dikalibrasi dari jam server; refresh halaman tidak
   mengulangi waktu. POST Mulai berulang tidak mereset waktu ujian aktif.
6. Server menentukan apakah waktu benar-benar habis, bukan nilai timeout
   yang dikirim browser. Setelah tenggat, jawaban yang terkumpul tetap dinilai.
7. Mengubah durasi TIDAK mengubah waktu peserta yang sudah mulai;
   perubahan berlaku bagi peserta yang memulai setelah pengaturan tersimpan.

ISI PATCH (HANYA FILE YANG BERUBAH / BARU)
application/controllers/Simulasi.php
application/controllers/Simulasibank.php
application/views/cbt/banksoal/data.php
application/views/simulasi/konfirmasi.php
application/views/simulasi/ujian.php
database/20261010_simulasi_durasi.sql

CARA MEMASANG (URUTAN PENTING)
1. CADANGKAN seluruh file situs dan DATABASE terlebih dahulu. Lebih aman
   lakukan saat tidak ada siswa yang sedang mengerjakan simulasi.
2. Pastikan proyek Garuda CBT yang terpasang merupakan versi yang sama
   dengan arsip .idea.zip sumber patch ini.
3. Masuk ke phpMyAdmin > pilih database Garuda CBT > menu SQL.
   Jalankan isi database/20261010_simulasi_durasi.sql.
   SQL boleh dijalankan ulang; hanya menambah kolom durasi_simulasi jika belum ada.
   Semua data lama tetap; nilai kolom baru NULL secara default.
4. Unggah isi ZIP mengikuti struktur folder situs. Lakukan replace pada
   LIMA file PHP yang ada, bukan mengganti keseluruhan folder application.
   Jangan unggah folder database sebagai pengganti database MySQL.
5. Pada server, pastikan opcache/file cache terbarui jika aktif.
   Bila perlu reload PHP-FPM dengan bantuan penyedia hosting.
6. Masuk ke akun ADMIN > CBT > Bank Soal. Di setiap bank soal tersedia
   Simulasi ON/OFF dan tombol "Atur Waktu" / "xx menit".
7. Klik "Atur Waktu", isi contoh "60", lalu "Simpan Waktu".
   Untuk mengikuti jadwal CBT lagi isi "0".
8. Uji sebagai peserta baru: pilih bank soal -> login -> konfirmasi
   (durasi tertulis) -> mulai -> timer menghitung mundur sesuai pengaturan.

PERIKSA SQL
SHOW COLUMNS FROM `cbt_bank_soal` LIKE 'durasi_simulasi';

PENTING
- Durasi ujian CBT reguler tidak diubah.
- Jam server, bukan jam laptop/HP, menentukan tenggat ujian. Upayakan
  konfigurasi jam server dan zona waktu hosting tetap benar.
- Session CodeIgniter tetap wajib aktif. Jawaban sementara disimpan di
  browser sesuai mekanisme kode sumber yang digunakan.
- File baru diuji via PHP lint, pemeriksaan JavaScript admin, dan unit
  mock pengaturan durasi; BELUM diuji langsung di hosting / database Anda.
- Jika admin menerima pesan "kolom durasi_simulasi belum tersedia", jalankan SQL.
- Jika versi situs lebih baru/berbeda dari arsip unggahan, gabungkan perubahan
  dengan diff patch, jangan asal overwrite karena dapat menimpa perubahan lain.

ROLLBACK
1. Kembalikan 5 file PHP hasil backup.
2. Kolom durasi_simulasi boleh dibiarkan (tidak mengganggu versi lama).
   Jika benar-benar ingin menghapusnya, BACKUP database lalu jalankan:
   ALTER TABLE `cbt_bank_soal` DROP COLUMN `durasi_simulasi`;
   Tindakan DROP COLUMN MENGHAPUS semua nilai durasi khusus yang disimpan.
