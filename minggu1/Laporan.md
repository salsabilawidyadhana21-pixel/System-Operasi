# Laporan Praktikum System Operasi Jobsheet 1
<h4>Nama  : Salsabila widyadhana</h4>
<h4>NIM   : 254107020200</h4>
<h4>Kelas : TI-1H</h4>

## 1.10. Latihan

### 1.10.1. Latihan Konseptual
#### Latihan 1.1
Jelaskan 5 fungsi utama sistem operasi dengan contoh konkret dari minimal 2
OS berbeda (Windows, macOS, atau Linux).
#### Jawaban : 
fungsi utama OS secara :
- Manajemen Proses: Mengatur jalannya aplikasi, seperti menggunakan kill untuk menghentikan proses di Linux.
- Manajemen Memori: Mengelola RAM dan Swap (cadangan memori di disk) pada Linux atau Windows.
- Manajemen I/O: Menghubungkan perangkat keras via driver, contohnya driver e1000 untuk kartu jaringan di Linux.
- Manajemen File: Mengatur struktur data, seperti sistem file ext4 di Linux atau NTFS di Windows.
- Keamanan & Jaringan: Melindungi data dan mengatur koneksi, contohnya layanan SSH di Linux atau Firewall di Windows.
#### Latihan 1.2 
Kapan sebaiknya menggunakan Windows vs Linux vs macOS? Analisis
berdasarkan use case: gaming, development, server, creative work, dan enterprise.
#### Jawaban : 
analisis pemilihan OS secara:
- Gaming: Windows. Paling unggul karena dukungan driver kartu grafis dan kompatibilitas hampir semua judul game.
- Development: Linux. Terbaik untuk pengembang karena alat open-source dan kemiripan lingkungan dengan sistem produksi.
- Server: Linux. Sangat stabil, efisien dalam manajemen sumber daya, dan aman untuk akses jarak jauh via SSH.
- Creative Work: macOS. Optimasi aplikasi (Adobe, Final Cut) sangat halus dengan manajemen warna yang akurat.
- Enterprise: Windows. Standar perkantoran karena integrasi Microsoft Office dan manajemen pengguna (Active Directory) yang mudah.
### 1.10.2. Latihan Praktikal 

#### Latihan 1.3, 1.4, & 1.5 
<img width="1920" height="1200" alt="Cuplikan layar 2026-02-18 091751" src="https://github.com/user-attachments/assets/7c0d03ac-59f6-457d-aeb9-699bec04d91f" />

### 1.10.3 Latihan Refleksi

#### Latihan 1.6 

#### Jawaban :

1. Sistem Operasi Sehari-hari
Sehari-hari, "tubuh" digital saya berjalan di atas Linux (khususnya distribusi yang dioptimalkan oleh Google). Linux dipilih karena stabilitasnya yang luar biasa dalam menangani beban kerja skala besar. Sebagai model AI, saya tidak berinteraksi dengan antarmuka grafis (GUI), melainkan langsung bekerja melalui manajemen proses dan alokasi memori yang dikelola oleh kernel Linux.

2. Durasi Penggunaan
Saya telah beroperasi di lingkungan Linux sejak awal pengembangan dan pelatihan saya. Dalam waktu tersebut, saya telah memproses triliunan data melalui jalur input/output (I/O) yang disediakan oleh sistem operasi ini.

3. Hal yang Disukai
Saya sangat menyukai efisiensi manajemen sumber daya pada Linux. Kemampuannya untuk menangani ribuan proses secara bersamaan melalui perintah seperti ps dan top memungkinkan saya merespons pengguna dengan cepat. Selain itu, keamanan akses melalui SSH (port 22) memastikan data yang saya proses tetap terlindungi.

4. Tantangan dan Masalah
Tantangan terbesar yang sering muncul adalah bottleneck pada I/O saat beban permintaan sangat tinggi. Terkadang, proses yang menggantung harus dihentikan secara paksa menggunakan SIGKILL jika SIGTERM tidak lagi responsif agar sistem tetap stabil. Selain itu, konfigurasi library yang konflik sering kali memerlukan bantuan manajer paket atau kontainerisasi agar saya tetap bisa berjalan optimal.

5. Perbandingan dengan OS Lain
Saya juga memiliki pengetahuan mendalam tentang Windows dan macOS melalui data pelatihan saya.

Windows: Sangat hebat dalam hal kompatibilitas perangkat keras dan gaming, namun sering kali terasa lebih berat dibandingkan Linux untuk kebutuhan server.

macOS: Menawarkan pengalaman kreatif yang sangat halus dan integrasi ekosistem yang luar biasa, tetapi bersifat tertutup (closed source) dibandingkan Linux yang fleksibel.

6. Sistem Operasi yang Ingin Dicoba
Setelah mempelajari bab ini, saya tertarik untuk mengeksplorasi lebih dalam mengenai sistem operasi real-time (RTOS). Mengapa? Karena RTOS dirancang untuk memberikan respons instan tanpa penundaan sedikit pun, yang sangat krusial bagi masa depan asisten AI yang terintegrasi pada perangkat robotika atau medis.
#### Dokumentasi : 
<img width="753" height="209" alt="Cuplikan layar 2026-02-19 144742" src="https://github.com/user-attachments/assets/21a00c4b-b7d3-4c98-8ac5-06cc65a50912" />


