# Laporan Praktikum System Operassi Jobsheet 2

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
2. (Opsional) monitor event udev (jalankan, lalu colok/lepas USB pada mesin
fisik):
# Praktikum 2.8 - Membuat Workspace Praktikum
### Langkah-langkah :


      




