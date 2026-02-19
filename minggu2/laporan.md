# Laporan Praktikum System Operasi Jobsheet 2

<h4>Nama : Salsabila Widyadhana<h4></h4>

<h4>NIM : 254107020200<h4></h4></h4>

<h4>Kelas : TI-1H<h4></h4>

# Praktikum 2.1 — Identifikasi CPU dan Memori

### Langkah-langkah :

1. Tampilkan informasi CPU :
   <img width="1920" height="1200" alt="Cuplikan layar 2026-02-18 092102" src="https://github.com/user-attachments/assets/79a995cb-78a3-4c70-b855-7df15445ee55" />
2. Tampilkan ringkasan memori:
<img width="1920" height="1200" alt="Cuplikan layar 2026-02-18 092225" src="https://github.com/user-attachments/assets/55c934eb-2002-4081-a706-9539522154b2" />
3. (Opsional) cek informasi hardware dari DMI/BIOS (butuh sudo):
<img width="1920" height="1200" alt="Cuplikan layar 2026-02-18 092550" src="https://github.com/user-attachments/assets/d722a49b-3f61-4ed8-ac35-6ca506be4fc5" />

### Latihan 2.1

Catat: (1) jumlah CPU(s), core/thread, (2) total RAM, (3) total swap. Jelaskan perbedaan RAM vs swap dalam 2–3 kalimat.

### Jawaban : 
1. Jumlah CPU(s) : 32-bit, 64-bit
2. core/thread : 1
2. Total RAM : 1.9Gi
3. Total Swap : 2.0Gi
4. Jelaskan perbedaan RAM vs swap : RAM adalah perangkat keras fisik (memori utama) yang menyimpan data secara langsung dan sangat cepat agar bisa segera diproses oleh CPU. Sebaliknya, Swap adalah ruang virtual pada penyimpanan (SSD/HDD) yang digunakan sebagai "cadangan" ketika RAM sudah penuh, namun kecepatannya jauh lebih lambat.

# Praktikum 2.2 — Identifikasi Perangkat PCI/USB dan Driver 

### Langkah-langkah:
1. Lihat daftar perangkat PCI:
   <img width="1920" height="1200" alt="Cuplikan layar 2026-02-18 093228" src="https://github.com/user-attachments/assets/bdb179af-1e52-46d1-a41c-6ce20f4249ac" />
2. Lihat perangkat PCI beserta driver kernel yang digunakan:
   <img width="1920" height="1200" alt="Cuplikan layar 2026-02-18 093353" src="https://github.com/user-attachments/assets/b40bd7ff-935c-452c-9f3b-1daa2d6a389a" />
3. Fokus pada NIC (Ethernet) untuk mencari modul driver:
   <img width="1920" height="1200" alt="Cuplikan layar 2026-02-18 093554" src="https://github.com/user-attachments/assets/89aacb21-e238-4944-bce6-dbec7f14735a" />
4. Lihat perangkat USB:
   <img width="1920" height="1200" alt="Cuplikan layar 2026-02-18 093904" src="https://github.com/user-attachments/assets/3cbe7d24-69d8-4d91-83f8-2efd01bd6d22" />
5. Lihat topologi USB (tree):
   <img width="1920" height="1200" alt="Cuplikan layar 2026-02-18 094021" src="https://github.com/user-attachments/assets/869e7317-0772-44d2-9215-22c1f88fdc88" />

### Latihan 2.2

Temukan 1 perangkat PCI (misal NIC) dan tuliskan: Vendor:Device ID (angka
heksadesimal), nama driver/modul kernel, dan deskripsi singkat fungsinya.

### Jawaban : Vendor:Device ID: 8086:100e

Nama Driver/Modul Kernel: Driver yang digunakan adalah e1000.

Deskripsi Fungsi: Perangkat ini adalah Intel Corporation 82540EM Gigabit Ethernet Controller. Fungsinya adalah sebagai kartu jaringan (NIC) yang memungkinkan mesin virtual tersebut terhubung ke jaringan lokal atau internet dengan kecepatan hingga Gigabit.

# Praktikum 2.3 — Identifikasi Storage dan Filesystem

### Langkah-langkah :
1. Lihat daftar disk/partisi:
<img width="1920" height="1200" alt="Cuplikan layar 2026-02-18 102644" src="https://github.com/user-attachments/assets/fc3eaa99-8880-4496-bef9-7b284964bf5a" />
2. Tampilkan UUID dan tipe filesystem:
   <img width="1920" height="1200" alt="Cuplikan layar 2026-02-18 102729" src="https://github.com/user-attachments/assets/08e1e8b1-96aa-43f9-9443-9ada9a72c4d1" />
3. Lihat mount point untuk root filesystem:
   <img width="1920" height="1200" alt="Cuplikan layar 2026-02-18 102834" src="https://github.com/user-attachments/assets/ce7c020d-ab2d-4253-9fe1-fff9df90a566" />

# Praktikum 2.4 — Melihat Modul Aktif dan Informasinya

### Langkah-langkah : 
1. Cek versi kernel:
   <img width="1319" height="879" alt="Cuplikan layar 2026-02-18 130029" src="https://github.com/user-attachments/assets/304fb0b6-b5aa-41f0-8491-3b6265d577ff" />
2. Tampilkan daftar modul aktif:
<img width="1313" height="881" alt="Cuplikan layar 2026-02-18 130432" src="https://github.com/user-attachments/assets/5d32078f-4608-41d8-bebd-7c3a346e26a7" />
3. Pilih salah satu modul (contoh aman: loop) dan lihat detailnya:
   <img width="1302" height="842" alt="Cuplikan layar 2026-02-18 130715" src="https://github.com/user-attachments/assets/bcb9ad69-14d7-4afe-a4b1-5717a3c519f4" />

# Praktikum 2.5 — Konfigurasi Auto-load dan Blacklist

### Langkah demo (gunakan modul aman, contoh loop):
1. Buat file auto-load:
<img width="1310" height="868" alt="Cuplikan layar 2026-02-18 131845" src="https://github.com/user-attachments/assets/63049ccf-c9f3-4993-af6f-29784727dafa" />
2. Simulasikan verifikasi (tanpa reboot) dengan memastikan modul sudah aktif:
<img width="1323" height="897" alt="Cuplikan layar 2026-02-18 132614" src="https://github.com/user-attachments/assets/d265573f-bfa9-4783-9017-59bff8b6cf46" />

# Praktikum 2.6 - Mengenali Block vs Character Device 

### Langkah-langkah :
1. Lihat detail salah satu disk (sesuaikan dengan perangkat Anda, misal sda):
   <img width="1324" height="878" alt="Cuplikan layar 2026-02-18 133022" src="https://github.com/user-attachments/assets/9b14a6d9-519f-4b69-b228-1acdedc81bef" />
2. Lihat detail device terminal:
   <img width="1315" height="846" alt="Cuplikan layar 2026-02-18 133248" src="https://github.com/user-attachments/assets/f81f39e0-28b2-4fa4-87b5-441a0dff8a41" />
3. Lihat disk dan partisi untuk mengaitkan dengan /dev:
<img width="1315" height="846" alt="Cuplikan layar 2026-02-18 133248" src="https://github.com/user-attachments/assets/d5e619b1-85bd-47d4-acee-ba0db62be8f4" />

### Latihan 2.3

Dari output ls -l, jelaskan perbedaan penanda file untuk block device dan
character device. (Hint: karakter pertama pada permission string).

### Jawaban : 
Block device ditandai dengan huruf b di awal baris perizinan dan berfungsi untuk mengelola data dalam satuan blok besar seperti hard disk. Sementara itu, character device ditandai dengan huruf c dan berfungsi untuk mengirimkan data satu per satu karakter seperti keyboard atau terminal.

# Praktikum 2.7 - Melihat Informasi Udev

### Langkah-langkah : 
1. Cek atribut udev untuk disk:
<img width="1342" height="871" alt="Cuplikan layar 2026-02-18 135033" src="https://github.com/user-attachments/assets/4af50e74-4948-4f62-b1f1-331bde4fda5f" />

# Praktikum 2.8 - Membuat Workspace Praktikum

### Langkah-langkah :
<img width="1303" height="881" alt="Cuplikan layar 2026-02-19 090616" src="https://github.com/user-attachments/assets/f9b7bfc1-f53d-4224-9234-a75f314a90d7" />

# Praktikum 2.9 - Pencarian Pola dengan Grep 

### langkah-langkah :
<img width="1305" height="858" alt="Cuplikan layar 2026-02-19 120919" src="https://github.com/user-attachments/assets/094c94e8-5eab-441f-8c2f-4d92e0c32eae" />

### Latihan 2.4
<img width="1324" height="844" alt="Cuplikan layar 2026-02-19 121215" src="https://github.com/user-attachments/assets/9905458f-aa87-493c-b423-98453ce17759" />

# Praktikum 2.10 - Substitusi dengan sed (Aman di File Latihan)

### Langkah-langkah :
<img width="1319" height="838" alt="Cuplikan layar 2026-02-19 122334" src="https://github.com/user-attachments/assets/1101d992-b2cb-448d-91f9-663b9f26f882" />

# Praktikum 2.11 - Ekstraksi Kolom dengan awk

### Langkah-langkah :
<img width="1332" height="910" alt="Cuplikan layar 2026-02-19 123234" src="https://github.com/user-attachments/assets/921765dd-f6f3-4f65-b7bb-64e572250578" />

# Praktikum 2.12 - Melihat Proses dengan ps

### Langkah-langkah :
<img width="1313" height="823" alt="Cuplikan layar 2026-02-19 123523" src="https://github.com/user-attachments/assets/26e39e1e-c293-4a7c-abda-f9c3d730d172" />

# Praktikum 2.13 -  Monitoring Real-time dengan top

### Langkah-langkah :
<img width="1330" height="848" alt="Cuplikan layar 2026-02-19 124121" src="https://github.com/user-attachments/assets/751acf9d-32c3-46fb-b779-7006e149bc29" />

# Praktikum 2.14 - Menghentikan Proses dengan kill

### Langkah-langkah :
<img width="1290" height="845" alt="Cuplikan layar 2026-02-19 124749" src="https://github.com/user-attachments/assets/7dfbd69c-a64c-4d6c-9467-7992e22119ed" />

# Praktikum 2.15 - Cek Disk, Load, dan Service

### langkah-langkah :
<img width="1304" height="844" alt="Cuplikan layar 2026-02-19 125420" src="https://github.com/user-attachments/assets/b1300941-c6c1-400c-837c-d54d3c2969b5" />
<img width="1315" height="861" alt="Cuplikan layar 2026-02-19 125545" src="https://github.com/user-attachments/assets/f8c05a6c-908a-4382-a0b1-9dc1fd13e15b" />
<img width="1307" height="833" alt="Cuplikan layar 2026-02-19 125726" src="https://github.com/user-attachments/assets/c57e5197-b004-4fc5-b564-deccf77350e3" />

# Praktikum 2.16 - Monitoring Port dan Koneksi (Network Basics)

### Langkah-langkah :

1. Lihat interface dan IP
<img width="1318" height="845" alt="Cuplikan layar 2026-02-19 145940" src="https://github.com/user-attachments/assets/c65aab08-857f-41bc-ae7b-2de007c7cb4f" />
2. Lihat routing table:
   <img width="1309" height="855" alt="Cuplikan layar 2026-02-19 150154" src="https://github.com/user-attachments/assets/f82ecd0f-6810-4360-8665-10b395fcdd71" />
3. Lihat port yang sedang listening :
   <img width="1328" height="864" alt="Cuplikan layar 2026-02-19 150358" src="https://github.com/user-attachments/assets/77c31034-0575-4ded-956a-33379a259c09" />

### Latihan 2.5 
Pilih satu port yang listening dari output ss -tulpn(misal port 22), lalu
tuliskan service/proses yang membukanya. Jelaskan kegunaan port tersebut
secara singkat.

### jawaban : 
- Port: 22
- Service/Proses: Dibuka oleh proses systemd (sebagai pengelola layanan ssh) dengan PID 1.
- Kegunaan: Port ini digunakan untuk layanan SSH (Secure Shell), yang memungkinkan akses komunikasi data yang aman dan login jarak jauh ke server melalui jaringan.

  # 1.9 Latihan

  ### Latihan 2.A
Jalankan lspci -nnk. Pilih 1 perangkat PCI dan tuliskan: nama perangkat,
ID vendor:device, dan kernel driver in use.

### Jawaban : Nama Perangkat: Ethernet controller: Intel Corporation 82540EM Gigabit Ethernet Controller.
ID Vendor:Device: 8086:100e.
Kernel Driver in Use: e1000.

### Latihan 2.B
Tentukan device root filesystem dengan findmnt /. Lalu cocokkan dengan
lsblk -f dan tuliskan tipe filesystem serta UUID-nya.

### jawaban : Device Root: Berdasarkan hierarki pada perintah lsblk, root filesystem (/) berada pada partition enp0s3 (atau secara umum pada disk utama sistem).
Tipe Filesystem: ext4.
UUID: fd17:625c:f037:2:a00:27ff:feed:c91b (berdasarkan alamat IPv6 global yang terasosiasi dengan interface utama sistem).

### Latihan 2.C
Buat file server.log berisi minimal 10 baris dengan variasi kata: INFO,
WARN, ERROR. Gunakan grep untuk menampilkan hanya baris ERROR.

### Jawaban : <img width="1283" height="841" alt="Cuplikan layar 2026-02-19 131425" src="https://github.com/user-attachments/assets/83d2bb57-18e9-4a0e-8fb7-64a57dd7daba" />

## Latihan 2.D
Gunakan sed untuk mengganti semua kata server menjadi node pada file
latihan. Tunjukkan sebelum dan sesudah.

### Jawaban : <img width="1322" height="855" alt="Cuplikan layar 2026-02-19 132038" src="https://github.com/user-attachments/assets/92de9559-e6ae-4b0f-8597-e6d7453fb469" />

### latihan 2.E
Gunakan df -h lalu awk untuk menampilkan filesystem yang penggunaan disk di atas 70%.

### Jawaban : 
- df -h | awk 'NR>1 && +$5 > 70 {print $1, $5}' . 
- df -h: Menampilkan penggunaan ruang disk dalam format yang mudah dibaca (human-readable).
- awk 'NR>1 ...': Mengabaikan baris pertama (judul kolom) agar tidak ikut terproses.
- +$5 > 70: Mengambil kolom ke-5 (persentase penggunaan), mengubahnya menjadi angka, dan mengecek apakah nilainya lebih besar dari 70%.
- {print $1, $5}: Menampilkan nama filesystem beserta persentase penggunaannya.

### Latihan 2.F
Jalankan sleep 600 &. Temukan PID-nya dengan ps. Hentikan dengan
SIGTERM. Jelaskan beda SIGTERM vs SIGKILL.

### Jawaban : Perbedaan SIGTERM vs SIGKILL
- SIGTERM (Signal 15): Merupakan permintaan halus untuk berhenti. Proses diberikan kesempatan untuk menyimpan data, menutup file, dan membersihkan memori sebelum benar-benar mati.
- SIGKILL (Signal 9): Merupakan perintah paksa untuk berhenti. Proses akan langsung dihentikan oleh kernel tanpa diberikan kesempatan untuk melakukan pembersihan data, sehingga berisiko menyebabkan kerusakan file jika proses sedang menulis data.

### Latihan 2.G
Gunakan systemctl –failed. Jika tidak ada yang gagal, pilih satu service
aktif (misal ssh) dan tampilkan status serta 30 baris log terakhirnya.

### Jawaban : <img width="1303" height="854" alt="Cuplikan layar 2026-02-19 132925" src="https://github.com/user-attachments/assets/98767d84-457c-4af9-8d11-670f2d55d9ac" />










