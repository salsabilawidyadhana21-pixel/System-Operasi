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
Kode : 

#!/bin/bash

for i in {1..12}

do

   {
   
     echo "--- Monitoring $(date) ---"
     
     echo "CPU Load: $(uptime | awk '{print $10 $11 $12}')"
     
     free -h
     
     echo ""
   
   } | tee -a monitor.log
   
   sleep 5

done
<img width="1272" height="804" alt="Cuplikan layar 2026-02-25 121803" src="https://github.com/user-attachments/assets/adf734ef-9f93-4a91-bf39-9098ac8ffc4c" />
### Latihan 3.4
Buat perintah yang:
1. Mencari semua file .conf di sistem
2. Membuang pesan "Permission denied"
3. Menghitung jumlah file yang ditemukan
4. Menyimpan daftar path lengkap ke file

#### Jawaban : 
Kode : find / -name "*.conf" 2> /dev/null | tee config-paths.txt | wc -l
<img width="1112" height="116" alt="Cuplikan layar 2026-02-25 122623" src="https://github.com/user-attachments/assets/5e7a3e93-31d2-4c98-8d1e-95606713ef34" />

### Latihan 3.5
Implementasikan script backup yang:
1. Menggunakan tar untuk backup direktori
2. Menampilkan progress dengan tee
3. Mencatat stdout ke backup-success.log
4. Mencatat stderr ke backup-error.log
5. Menambahkan timestamp di setiap log entry

#### Jawaban :
Kode : 
#!/bin/bash

"mengganti /path/tujuan dengan folder yang ingin di-backup"

tar -cvf backup.tar /path/tujuan \

  > >(tee -a backup-success.log) \
  
  2> >(tee -a backup-error.log)
  <img width="1264" height="132" alt="Cuplikan layar 2026-02-25 123326" src="https://github.com/user-attachments/assets/c20e8a46-b337-4430-b084-087034263bc3" />



