# Laporan Praktikum System Operasi Jobsheet 4 
## Percobaan 1 : Directory
#### 1. Melihat direktori HOME 

$ pwd 

$ echo $HOME

#### 2.Melihat direktori aktual dan parent direktori 

$ pwd 

$ cd .. 

$ pwd 

$ cd .. 

$ pwd 

$ cd

#### 3.Membuat satu direktori, lebih dari satu direktori atau sub direktori 

$ pwd 

$ mkdir A B C A/D A/E B/F A/D/A 

$ ls -l 

$ ls -l A 

$ ls -l A/D

#### 4.Menghapus direktori (hanya untuk direktori kosong)

$ rmdir B  (Muncul error karena B tidak kosong)

$ ls -l B

$ rmdir B/F B

$ ls -l B 

#### Mengapa muncul pesan error : karena perintah rmdir hanya dapat digunakan untuk menghapus direktori yang benar-benar kosong, dan ketika menjalankan $ rmdir B, akan muncul error jika di dalam direktori B masih terdapat subdirektori atau file (seperti subdirektori F). Dan pesan error itu muncul untuk mencegah penghapusan data secara tidak sengaja.

#### 5.Navigasi direktori dengan intruksi cd untuk pindah dari satu direktori ke direktori lain. 

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
