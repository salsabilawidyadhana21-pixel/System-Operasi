# Laporan Praktikum System Operasi Minggu 3 
<h4>Nama : Salsabila Widyadhana</h4>
<h4>NIM : 254107020200</h4>
<h4>Kelas : TI-1H</h4>

## Latihan 1.11
### Latihan 3.1
Buatlah script yang:
1. Menampilkan daftar 10 file terbesar di direktori /var/log/
2. Menyimpan hasilnya ke file large-logs.txt
3. Menampilkan output juga di terminal menggunakan tee
4. Menangani error dengan redirect ke error.log
#### Jawaban : 
Kode : 
#!/bin/bash

"Menampilkan 10 file terbesar, simpan ke file, tampilkan di terminal"

ls -lS /var/log/ 2> error.log | head -n 11 | tee large-logs.txt
<img width="1280" height="328" alt="Cuplikan layar 2026-02-25 095234" src="https://github.com/user-attachments/assets/decf3f4c-63f5-43c3-af11-872673130bbf" />

### Latihan 3.2
Buat pipeline yang:
1. Membaca /etc/passwd
2. Mengekstrak username (kolom pertama)
3. Mengurutkan alfabetis
4. Menyimpan ke file sorted-users.txt
Hint: Gunakan cut, sort, dan operator redirect.

#### Jawaban :
Kode : 
cut -d: -f1 /etc/passwd | sort > sorted-users.txt

Setelah menjalankan kode diatas terminal tidak memunculkan apa-apa, karena operator > memindahkan semua output ke dalam file sorted-users.txt.

### Latihan 3.3
Tulis script monitoring yang:
1. Mencatat penggunaan CPU dan memory setiap 5 detik
2. Menyimpan log dengan timestamp
3. Berjalan selama 1 menit (12 iterasi)
4. Output ditampilkan di terminal DAN disimpan ke file

#### Jawaban : 

