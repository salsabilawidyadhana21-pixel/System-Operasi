# Laporan Praktikum System Operasi Jobsheet 6 Manajemen Proses

<h4>Nama : Salsabila Widyadhana<h4>
<h4>NIM : 254107020200<h4>
<h4>Kelas : TI-1H<h4>

## Praktikum 6.1 — Melihat Proses dan Thread

1. ps aux  :<img width="650" height="412" alt="image" src="https://github.com/user-attachments/assets/16801297-d3a7-4478-b58e-ba606cf8f8b3" />
2. ps aux -L : <img width="647" height="407" alt="image" src="https://github.com/user-attachments/assets/02487fbd-7b5c-4796-9494-5417d0239808" />
3. echo $$

ps -p $$ -f : <img width="648" height="53" alt="image" src="https://github.com/user-attachments/assets/0feab389-7baa-4f3f-ac85-faa81dff2df8" />

4. pstree -p : <img width="644" height="271" alt="image" src="https://github.com/user-attachments/assets/9fee86d5-7b85-4ea0-8c51-ca0b640dd40a" />

### Latihan 6.1

Jalankan ps aux dan amati outputnya:
1. Berapa total proses yang berjalan? Proses apa yang memiliki PID terkecil?
   - Total Proses: Untuk mengetahui jumlah total proses, Anda dapat menjalankan perintah ps aux | wc -l di terminal. Angka yang muncul (dikurangi satu untuk baris header) adalah total proses yang sedang aktif saat itu.
   - PID Terkecil: Proses dengan PID terkecil biasanya adalah PID 1, yang merupakan proses induk pertama yang dijalankan oleh kernel saat sistem melakukan booting (seringkali bernama systemd atau init).

2.  Jalankan pstree -p dan temukan proses bash Anda. Proses apa yang menjadi induk (PPID) dari bash tersebut?
    - Saat Anda menjalankan pstree -p, Anda akan melihat struktur pohon proses.
    - Proses induk (PPID) dari bash biasanya adalah proses terminal emulator yang Anda gunakan (seperti gnome-terminal, konsole, atau sshd jika Anda login via SSH). 

3. Bandingkan output ps aux dan ps aux -L. Apa perbedaan yang Anda lihat?
   - ps aux: Menampilkan snapshot dari proses yang sedang berjalan saja.
   - ps aux -L: Menampilkan proses beserta thread-nya. Perbedaan utamanya adalah munculnya kolom LWP (Light-Weight Process ID) yang menunjukkan ID unik untuk setiap thread di dalam satu proses yang sama. Jika sebuah proses memiliki banyak thread, proses tersebut akan muncul di beberapa baris dengan PID yang sama tetapi LWP yang berbeda. 

## Praktikum 6.2 — Mengamati Siklus Hidup Proses

1. <img width="645" height="50" alt="image" src="https://github.com/user-attachments/assets/aada5d9f-b749-4537-bbd5-43d404527160" />

2. <img width="646" height="110" alt="image" src="https://github.com/user-attachments/assets/0e796e21-d40d-4c9b-b7dc-911340c52a93" />

### Latihan 6.2 

1. Jalankan sleep 120 & dan amati kolom STAT pada ps aux. Kondisi
apa yang ditampilkan? Mengapa proses sleep berada di kondisi tersebut?
<img width="643" height="52" alt="image" src="https://github.com/user-attachments/assets/53f8f569-1e36-42a4-9d7a-b5ec63039b56" />

- Kondisi (STAT): Kondisi yang ditampilkan adalah S (Sleeping).
- Alasan: Proses sleep berada dalam kondisi ini karena ia sedang menunggu suatu event atau waktu tertentu (dalam hal ini, menunggu durasi 120 detik selesai) dan tidak sedang aktif menggunakan CPU. Kondisi Sleeping ini dapat diinterupsi oleh sinyal jika diperlukan.

2. Jalankan beberapa perintah yang berhasil dan yang gagal, lalu catat exit
code masing-masing. Pola apa yang Anda temukan?
<img width="645" height="110" alt="image" src="https://github.com/user-attachments/assets/7d618c7b-9b18-4924-b62f-ef8a5a8d55c5" />

- Pola Exit Code:
  - Jika perintah berhasil dijalankan, variabel $? akan menampilkan nilai 0.
  - Jika perintah gagal (misalnya karena direktori tidak ditemukan atau kesalahan sintaks), variabel $? akan menampilkan nilai selain 0 (seperti 1, 2, atau 127 tergantung jenis kesalahannya).
- Kesimpulan: Pola ini digunakan dalam scripting untuk mendeteksi status sukses atau gagalnya suatu instruksi secara otomatis.

## Praktikum 6.3 — Mengatur Prioritas Proses
<img width="646" height="118" alt="image" src="https://github.com/user-attachments/assets/bcada4c7-4949-4f7a-8360-0576d463cb81" />

### Latihan 6.3

1. Jalankan nice -n 5 sleep 200 & dan verifikasi nilai NI-nya dengan
ps.

<img width="631" height="60" alt="image" src="https://github.com/user-attachments/assets/e4d9c68e-6017-4cb2-a515-ab1d71b7bb5b" />

2. <img width="580" height="51" alt="image" src="https://github.com/user-attachments/assets/e149cbcc-b6ee-4bd0-80be-7309bfa68dc4" />

3. <img width="575" height="36" alt="image" src="https://github.com/user-attachments/assets/9d1a16e3-66f4-4d0f-874d-dd82e311d21e" />

- Apa yang terjadi?
  - Akan muncul pesan error seperti: “renice: failed to set priority for <PID> (process ID): Permission denied”.

Mengapa Linux membatasi hal ini?
- Keamanan & Keadilan: Hanya user root yang diperbolehkan memberikan nilai nice negatif (prioritas lebih tinggi).
- Mencegah Monopoli: Jika user biasa diizinkan menaikkan prioritas sesuka hati, mereka bisa mengambil alih seluruh jatah waktu CPU dan membuat sistem menjadi tidak stabil bagi user lain.
- Aturan User: User biasa hanya diperbolehkan menurunkan prioritas (menaikkan nilai nice) untuk proses miliknya sendiri.

## Praktikum 6.4 — Mengirim Sinyal ke Proses

1. <img width="539" height="92" alt="image" src="https://github.com/user-attachments/assets/60d8e8d8-1f81-4ddd-8f18-81ea49ba509e" />

2. <img width="581" height="61" alt="image" src="https://github.com/user-attachments/assets/51cc552d-1c83-4c5b-b316-70a3fe9050bc" />

3. <img width="621" height="116" alt="image" src="https://github.com/user-attachments/assets/87ab48b8-4854-4421-acec-c55020abd53a" />

4. <img width="444" height="36" alt="image" src="https://github.com/user-attachments/assets/91f89124-b8e2-4635-8941-2e80f6e394b3" />

### Latihan 6.4

1. <img width="498" height="75" alt="image" src="https://github.com/user-attachments/assets/0b0d43c7-463a-4f76-b471-aa26d2e87c0f" />

  - Hasil Pengamatan STAT: Kondisi yang muncul pada kolom STAT adalah T (Stopped). Ini menandakan proses sedang dijeda oleh sinyal atau debugger.

2. <img width="464" height="43" alt="image" src="https://github.com/user-attachments/assets/3e3d33bf-ae6f-4745-a72c-df9b8813e7f3" />

3. <img width="488" height="42" alt="image" src="https://github.com/user-attachments/assets/c8d09b87-109e-4364-9ce3-c0e74b4cbc32" />

Kapan Anda memilih SIGKILL daripada SIGTERM?
 - sebaiknya memilih SIGKILL hanya jika proses tersebut "bandel" atau macet dan tidak merespons perintah SIGTERM.

## Praktikum 6.5 — Manajemen Job Foreground dan Background
