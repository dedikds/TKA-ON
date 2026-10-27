# Ekspor bank soal admin

Salin `application/views/cbt/banksoal/data.php` ke lokasi yang sama pada instalasi Garuda CBT. Cadangkan file lama sebelum menggantinya.

Di halaman Bank Soal, setiap bank memiliki tombol **DOCX** dan **PDF**. DOCX langsung diunduh. PDF membuka dialog cetak; pilih **Simpan sebagai PDF / Save as PDF** sebagai tujuan. Jika tab tidak terbuka, izinkan popup untuk situs aplikasi.

Perubahan ini memakai endpoint soal dan pustaka JavaScript yang sudah ada pada proyek. Tidak ada perubahan basis data atau dependensi baru.
