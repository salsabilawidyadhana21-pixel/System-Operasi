# Laporan Praktikum System Operasi Jobsheet 4 
## Percobaan 1 : Directory
#### 1. Melihat direktori HOME 
```
$ pwd 

$ echo $HOME
```
#### 2.Melihat direktori aktual dan parent direktori 
```
$ pwd 

$ cd .. 

$ pwd 

$ cd .. 

$ pwd 

$ cd
```
#### 3.Membuat satu direktori, lebih dari satu direktori atau sub direktori 
```
$ pwd 

$ mkdir A B C A/D A/E B/F A/D/A 

$ ls -l 

$ ls -l A 

$ ls -l A/D
```
#### 4.Menghapus direktori (hanya untuk direktori kosong)
```
$ rmdir B  (Muncul error karena B tidak kosong)

$ ls -l B

$ rmdir B/F B

$ ls -l B 
```
#### Mengapa muncul pesan error : karena perintah rmdir hanya dapat digunakan untuk menghapus direktori yang benar-benar kosong, dan ketika menjalankan $ rmdir B, akan muncul error jika di dalam direktori B masih terdapat subdirektori atau file (seperti subdirektori F). Dan pesan error itu muncul untuk mencegah penghapusan data secara tidak sengaja.

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
```
#### Muncul error karena Path Tidak Ditemukan, Jika direktori tujuan tidak ada atau path yang dimasukkan salah, Linux akan memberikan pesan error "No such file or directory".

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
#### 2. Perintah mv untuk memindah file
```
$ mv contoh contoh2

$ ls -l

$ mv contoh1 contoh2 A/D

$ ls -l A/D

$ mv contoh contoh1 C

$ ls -l C 
```
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
## Percobaan 4 : Melihat isi File 
```
$ ls -l

$ file halo.txt 

$ file bye.txt
```
## Percobaan 5 : Mencari file 

#### 1. Perintah find 
```
$ find /home -name "*.txt" -print > myerror.txt 

$ find . -name "*.txt" -exec wc -l '{}' ';' 
```
#### 2. Perintah which 
```
$ which ls 
```
#### 3. Perintah locate 
```
$ locate "*.txt"
```
## Percobaan 6 : Mencari Text pada File 
```
$ grep Hallo *.txt
```
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
### Latihan 2 & 3: Penelusuran Direktori Sistem dan Hardware
```
#Menelusuri direktori biner dan sistem
$ ls /bin 
$ ls /usr/bin 
$ ls /sbin 
#Menelusuri direktori sementara dan boot
$ ls /tmp 
$ ls /boot 
#Identifikasi perangkat terminal (tty) Anda
$ who am i 
#Melihat pemilik file perangkat tty (ganti ttyX sesuai hasil who am i)
$ ls -l /dev/tty
```
<img width="1273" height="817" alt="Cuplikan layar 2026-03-04 094936" src="https://github.com/user-attachments/assets/b5d6f865-658f-4a4f-a7a5-2e700813a32c" />

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
# 6. Kembali ke home sendiri
cd ~ 
# 7. Membuat subdirektori work dan play
mkdir work play 
# 8. Menghapus subdirektori work
rmdir work 
# 9. Copy file passwd ke home
cp /etc/passwd ~ 
# 10. Pindahkan file tersebut ke subdirektori play
mv passwd play/
```
<img width="652" height="40" alt="image" src="https://github.com/user-attachments/assets/c0312c66-7a52-4ea3-96ab-686aef99173f" />
<img width="646" height="71" alt="image" src="https://github.com/user-attachments/assets/45662dc3-33fd-435a-85db-9e635708b29a" />

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












