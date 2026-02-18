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
      


