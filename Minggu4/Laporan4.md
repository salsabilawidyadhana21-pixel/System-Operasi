# Laporan Praktikum System Operasi Jobsheet 4 

<h4>Nama : Salsabila Widyadhana<h4>
<h4>NIM : 254107020200<h4>
<h4>Kelas : TI-1H<h4>
<h4>Pertemuan ke- : 4<h4>
  
## Percobaan 1 : Directory
#### 1. Melihat direktori HOME 
```
$ pwd 

$ echo $HOME
```
<img width="643" height="183" alt="image" src="https://github.com/user-attachments/assets/eea95afd-952e-45f0-ad94-b346a683d7dc" />

#### 2.Melihat direktori aktual dan parent direktori 
```
$ pwd 

$ cd .. 

$ pwd 

$ cd .. 

$ pwd 

$ cd
```
<img width="643" height="183" alt="image" src="https://github.com/user-attachments/assets/eea95afd-952e-45f0-ad94-b346a683d7dc" />

#### 3.Membuat satu direktori, lebih dari satu direktori atau sub direktori 
```
$ pwd 

$ mkdir A B C A/D A/E B/F A/D/A 

$ ls -l 

$ ls -l A 

$ ls -l A/D
```
<img width="643" height="183" alt="image" src="https://github.com/user-attachments/assets/eea95afd-952e-45f0-ad94-b346a683d7dc" />
<img width="656" height="248" alt="image" src="https://github.com/user-attachments/assets/f57ea9eb-ace4-4215-995d-de172db985f5" />

#### 4.Menghapus direktori (hanya untuk direktori kosong)
```
$ rmdir B  (Muncul error karena B tidak kosong)

$ ls -l B

$ rmdir B/F B

$ ls -l B

#### Mengapa muncul pesan error : karena perintah rmdir hanya dapat digunakan untuk menghapus direktori yang benar-benar kosong, dan ketika menjalankan $ rmdir B, akan muncul error jika di dalam direktori B masih terdapat subdirektori atau file (seperti subdirektori F). Dan pesan error itu muncul untuk mencegah penghapusan data secara tidak sengaja.
```
<img width="647" height="114" alt="image" src="https://github.com/user-attachments/assets/429d8eac-ffb0-46af-90b7-ee61e3b07933" />

#### 5.Navigasi direktori dengan intruksi cd untuk pindah dari satu direktori ke direktori lain. 
```
$ pwd

$ ls -l

$ cd A

$ pwd

$ cd ..

$ pwd

$ cd /home/<user>/C

$ pwd

$ cd /<user/C

$ pwd

#### Muncul error karena Path Tidak Ditemukan, Jika direktori tujuan tidak ada atau path yang dimasukkan salah, Linux akan memberikan pesan error "No such file or directory".
```
<img width="650" height="261" alt="image" src="https://github.com/user-attachments/assets/dfd73287-ade6-4de6-8086-79ef068b3dca" />


## Percobaan 2 : Manipulasi File

#### 1.Perintah cp (Copy) 
```
$ cat > contoh

[Ctrl -d]

$ cp contoh contoh1

$ ls -l

$ cp contoh A 

$ ls -l A 

$ cp contoh contoh1 A/D 

$ ls -l A/D
```
<img width="638" height="204" alt="image" src="https://github.com/user-attachments/assets/95e32247-ecb6-45ba-bdb6-a1e42c58551b" />
<img width="646" height="117" alt="image" src="https://github.com/user-attachments/assets/c0814585-e976-4a11-ae8d-d05f60ae167f" />

#### 2. Perintah mv untuk memindah file
```
$ mv contoh contoh2

$ ls -l

$ mv contoh1 contoh2 A/D

$ ls -l A/D

$ mv contoh contoh1 C

$ ls -l C 
```
<img width="640" height="190" alt="image" src="https://github.com/user-attachments/assets/ad2f93b8-d494-4681-9760-c6359137c7df" />
<img width="639" height="102" alt="image" src="https://github.com/user-attachments/assets/c919c402-0ccc-4510-b8f8-d8ad216a150f" />

#### 3.Perintah rm untuk menghapus file 
```
$ rm contoh2 
$ ls -l 
$ rm -i contoh
$ rm -rf A C 
$ ls -l
```
<img width="655" height="345" alt="image" src="https://github.com/user-attachments/assets/16e2b1fb-aac2-40aa-b087-573ddf19dcbc" />

## Percobaan 3 : Symbolic Link 

#### 1. Membuat Shortcut (file link)
```
$ echo "Hallo apa khabar" > halo.txt 

$ ls -l $ ln halo.txt z  (Membuat hard link bernama z yang menunjuk ke halo.txt)

$ ls -l $ cat z 

$ mkdir mydir $ ln z mydir/halo.juga 

$ cat mydir/halo.juga 

$ ln -s z bye.txt  (Membuat soft link atau symbolic link bernama bye.txt yang menunjuk ke z) 

$ ls -l bye.txt 

$ cat bye.txt 
```
<img width="647" height="326" alt="image" src="https://github.com/user-attachments/assets/ebab287b-fdfb-49db-b639-5da6c8cc4183" />
<img width="647" height="102" alt="image" src="https://github.com/user-attachments/assets/3c3767e5-42d9-4fcf-a131-0f750cfa7777" />

## Percobaan 4 : Melihat isi File 
```
$ ls -l

$ file halo.txt 

$ file bye.txt
```
<img width="642" height="200" alt="image" src="https://github.com/user-attachments/assets/bb1ec887-17e8-4c91-8713-65c14a9b2039" />

## Percobaan 5 : Mencari file 

#### 1. Perintah find 
```
$ find /home -name "*.txt" -print > myerror.txt 

$ find . -name "*.txt" -exec wc -l '{}' ';' 
```
<img width="648" height="209" alt="image" src="https://github.com/user-attachments/assets/7ed57ec5-9bb4-45c5-b376-0ae8b2df82bb" />

#### 2. Perintah which 
```
$ which ls 
```
<img width="641" height="51" alt="image" src="https://github.com/user-attachments/assets/2171f333-36f8-40f7-9007-7d43d48c7053" />

#### 3. Perintah locate 
```
$ locate "*.txt"
```
<img width="641" height="51" alt="image" src="https://github.com/user-attachments/assets/2171f333-36f8-40f7-9007-7d43d48c7053" />

## Percobaan 6 : Mencari Text pada File 
```
$ grep Hallo *.txt
```
<img width="643" height="37" alt="image" src="https://github.com/user-attachments/assets/59311666-db91-4f35-a300-18e82a7d8b74" />

## Latihan !
### Latihan 1
```
#Pindah ke home directory
$ cd 
#Melihat path direktori saat ini
$ pwd 
#Melihat semua file termasuk yang tersembunyi
$ ls -al 
#Pindah ke direktori aktif (tidak berubah)
$ cd . 
#Pindah ke parent directory (satu level di atas)
$ cd .. 
#Kembali ke direktori sebelumnya
$ cd - 
#Identifikasi tipe file
$ file halo.txt 
```
<img width="640" height="344" alt="image" src="https://github.com/user-attachments/assets/34cea75c-59f4-4745-a1b0-b9fbeb2ca3b1" />

### Latihan 2 & 3: Penelusuran Direktori Sistem dan Hardware
```
#Menelusuri direktori biner dan sistem
$ ls /bin 
$ ls /usr/bin 
$ ls /sbin
```
<img width="655" height="415" alt="image" src="https://github.com/user-attachments/assets/08e13fa0-e9fb-44d9-975c-cf2c8c5c914c" />
<img width="655" height="415" alt="image" src="https://github.com/user-attachments/assets/08e13fa0-e9fb-44d9-975c-cf2c8c5c914c" />
<img width="649" height="414" alt="image" src="https://github.com/user-attachments/assets/0d1ac87c-5874-411d-a789-0ffd2df9e686" />

```
#Menelusuri direktori sementara dan boot
$ ls /tmp 
$ ls /boot
```
<img width="646" height="101" alt="image" src="https://github.com/user-attachments/assets/f47ece21-70fe-4534-b7f2-e30f94cdde7f" />

```
#Identifikasi perangkat terminal (tty) Anda
$ who am i 
```
<img width="642" height="36" alt="image" src="https://github.com/user-attachments/assets/fd3c14b2-4b82-4ad5-94e3-ed634973fe19" />

```
#Melihat pemilik file perangkat tty (ganti ttyX sesuai hasil who am i)
$ ls -l /dev/tty
```
<img width="618" height="36" alt="image" src="https://github.com/user-attachments/assets/2e852e66-19f8-4faa-a3ff-c08c3b504f96" />

### Latihan 4 Eksplorasi Direktori /proc (Pseudo-filesystem)
```
# Melihat informasi hardware dan sistem dari kernel
cat /proc/interrupts 
cat /proc/devices 
cat /proc/cpuinfo 
cat /proc/meminfo 
cat /proc/uptime
```
<img width="668" height="426" alt="image" src="https://github.com/user-attachments/assets/2de3a77c-91cd-4af4-9c60-9498bc833a1a" />
<img width="655" height="416" alt="image" src="https://github.com/user-attachments/assets/40d59ccf-1cde-4b0f-b9eb-7367cffd75e0" />
<img width="659" height="415" alt="image" src="https://github.com/user-attachments/assets/b9dae89c-08d4-4f55-ace5-290bcbba1316" />
<img width="654" height="44" alt="image" src="https://github.com/user-attachments/assets/2e84d95f-6bf1-498d-a6aa-70a3a1781c36" />

### Latihan 5 - 10: Manipulasi Direktori dan File
```
# 5. Pindah ke home user lain (contoh: user 'student')
cd ~student
- Hasil "No such file or directory", karena secara sistem jika user student memang tidak ada.
```
<img width="648" height="36" alt="image" src="https://github.com/user-attachments/assets/a793a0a5-8118-4e1b-a4b7-efc5345e9048" />

 ```
# 6. Kembali ke home sendiri
cd ~
```
<img width="614" height="135" alt="image" src="https://github.com/user-attachments/assets/b490c4a8-71bc-4872-bcb4-a70b22fb9f25" />

```
# 7. Membuat subdirektori work dan play
mkdir work play
```
<img width="614" height="94" alt="image" src="https://github.com/user-attachments/assets/24b56d87-6a8b-4812-ba4c-d02c9ca1104f" />

``` 
# 8. Menghapus subdirektori work
rmdir work
```
<img width="614" height="94" alt="image" src="https://github.com/user-attachments/assets/81879373-adff-4411-b0dc-fa285fa7277c" />

```
# 9. Copy file passwd ke home
cp /etc/passwd ~
```
<img width="614" height="94" alt="image" src="https://github.com/user-attachments/assets/e44e6787-5838-4669-bb8e-169cc930af63" />

```
# 10. Pindahkan file tersebut ke subdirektori play
mv passwd play/
```
<img width="614" height="94" alt="image" src="https://github.com/user-attachments/assets/a265eb9d-123b-4e9b-bea6-6139902a809d" />


### Latihan 11 - 15: Link dan Operasi Terminal
```
# 11. Membuat symbolic link ke tty di dalam direktori play
#(Asumsi terminal anda adalah /dev/pts/0 atau sesuai hasil 'who am i')
cd ~/play 
ln -s /dev/tty terminal 
# 12. Membuat file hello.txt
echo "hello word" > hello.txt 
# 13. Copy isi file ke terminal (output akan muncul di layar console)
cp hello.txt terminal 
# 14. Copy direktori play ke work menggunakan symbolic link
cd ~ 
ln -s play work 
# 15. Hapus direktori work (link-nya)
rm work
# Jika ingin menghapus direktori play beserta isinya:
rm -rf play
```
<img width="647" height="85" alt="image" src="https://github.com/user-attachments/assets/8589c602-a735-4bf9-9216-23f47fecab3b" />

#### Jawaban untuk Pertanyaan Latihan:
```
Pertanyaan No. 11: Apa yang terjadi jika melakukan hard link ke perangkat tty?

Jawaban: Hard link terbatas pada partisi disk yang sama dan tidak dimungkinkan untuk file spesial seperti perangkat sistem. Sistem akan mengeluarkan pesan error karena hard link tidak diizinkan untuk file karakter seperti /dev/tty.


Pertanyaan No. 12: Dapatkah menggunakan cp dengan terminal sebagai file asal?

Analisa: Tidak bisa menghasilkan efek yang sama dengan mudah, karena terminal (tty) berfungsi sebagai input/output real-time. Jika digunakan sebagai file asal, perintah cp akan terus menunggu input dari keyboard Anda.
```

# 1. Analisa Hasil Percobaan (1 - 6)

### Percobaan 1 (Direktori):

- Analisa: Perintah pwd dan echo $HOME digunakan untuk memverifikasi lokasi direktori kerja saat ini dan direktori asal pengguna. Pembuatan struktur direktori bertingkat menggunakan mkdir berhasil membangun hirarki folder seperti pohon.

- Analisa Error: Muncul pesan error saat rmdir B karena direktori tersebut berisi subdirektori F. Sesuai teori, rmdir hanya menghapus direktori yang benar-benar kosong untuk keamanan data. Pada navigasi cd /<user/C, error terjadi karena kesalahan penulisan path (sintaksis tidak valid).

### Percobaan 2 (Manipulasi File):

- Analisa: Perintah cat > contoh memungkinkan pembuatan file teks secara interaktif. Perintah cp (copy) dan mv (move/rename) bekerja dengan prinsip silent success—tidak ada output jika berhasil, sehingga perubahan harus diperiksa manual dengan ls -l.

- Analisa Operasi: Penghapusan dengan rm -rf sangat efisien namun berisiko karena menghapus seluruh direktori dan isinya tanpa konfirmasi (kecuali ditambahkan opsi -i).

### Percobaan 3 (Symbolic Link):

- Analisa: Percobaan ini membuktikan perbedaan antara hard link dan soft link. Hard link (ln) menciptakan nama file baru yang berbagi INODE yang sama, sehingga meningkatkan jumlah link count pada file asli.

- Analisa Soft Link: Symbolic link (ln -s) berfungsi sebagai penunjuk (shortcut) ke nama file asal tanpa mengubah data asli atau link count sumber.

### Percobaan 4 (Melihat Isi File):

Analisa: Perintah file memberikan deskripsi tipe data. Hasil menunjukkan halo.txt sebagai ASCII text dan bye.txt sebagai symbolic link, yang mengonfirmasi identitas file berdasarkan isinya, bukan sekadar ekstensi.

### Percobaan 5 (Mencari File):

Analisa: Perintah find sangat kuat untuk pencarian berdasarkan pola nama. Penggunaan 2> myerror.txt sering digunakan untuk memisahkan hasil pencarian dari pesan error "Permission Denied" pada direktori sistem.

### Percobaan 6 (Mencari Teks):

Analisa: Perintah grep berhasil memfilter baris teks tertentu di dalam file. Ini memudahkan pencarian informasi spesifik di dalam file konfigurasi atau log yang besar.

# 2. Analisa Latihan (1 - 15)

### Latihan 1 - 4 (Eksplorasi Sistem):
Analisa: Navigasi menggunakan cd .. (ke atas) dan cd - (kembali) mempermudah mobilitas di struktur direktori Linux. Direktori /proc diidentifikasi sebagai pseudo-filesystem karena menyediakan akses langsung ke struktur data kernel melalui RAM.

### Latihan 5 - 10 (Manajemen User & File):

Analisa: Perintah cd ~username akan gagal jika user tersebut tidak ada di sistem lokal. Penyalinan file /etc/passwd ke home dan pemindahannya ke folder play menunjukkan fleksibilitas dalam mengelola file administrative untuk keperluan latihan.

### Latihan 11 - 15 (Link & Output Perangkat):
- Analisa Perangkat: Membuat symbolic link ke /dev/tty memungkinkan file tersebut mewakili terminal aktif.

- Hard Link ke tty: Tidak dapat dilakukan karena hard link tidak diizinkan pada file karakter/spesial dan terbatas pada satu partisi saja.

- Efek Output: Perintah cp hello.txt terminal menyebabkan teks langsung tercetak di layar. Hal ini membuktikan konsep utama Linux: "Segala sesuatu adalah file", termasuk perangkat keras output.

- Penghapusan Akhir: Penggunaan rm -rf play di akhir latihan membersihkan seluruh sisa percobaan secara permanen.

# 3. Pohon Struktur Direktori (Percobaan 1.3)

Berdasarkan instruksi mkdir ABCA/D A/E B/F A/D/A:
/ (Home)

├── A/

│   ├── D/

│   │   └── A/

│   └── E/

├── B/

│   └── F/

└── C/

# 4. Kesimpulan

Sistem file Linux diatur secara hirarkhikal dimulai dari root (/) yang menyerupai struktur pohon. Setiap file dan direktori memiliki properti unik (tipe, ijin akses, pemilik, dan jumlah link) yang menentukan perilaku sistem terhadap objek tersebut. Symbolic link lebih fleksibel daripada hard link karena dapat melintasi partisi disk yang berbeda. Linux memperlakukan peralatan hardware (seperti terminal di /dev/tty) sama seperti file biasa, yang memungkinkan manipulasi input/output menggunakan perintah standar seperti cp atau echo. Pemahaman tentang navigasi dan manajemen direktori adalah fondasi utama dalam administrasi sistem operasi Linux.








