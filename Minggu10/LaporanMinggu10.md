# Laporan Praktikum System Operasi Minggu 10 Manajemen Memori & System Call

<h4>Nama        : Salsabila Widyadhana<h4>
<h4>NIM         : 254107020200<h4>
<h4>Kelas       : TI-1H<h4>
<h4>Mata Kuliah : System Operasi<h4>

## Praktikum 10.1 Melihat Penggunaan Memori
```
free -h
cat / proc / meminfo | head -n 20
```
<img width="297" height="179" alt="image" src="https://github.com/user-attachments/assets/7e98f60c-dfcf-4771-bcd7-007d847aae67" />

Analisis:

1. Hitung persentase memori tersedia: available / total × 100%. Jika hasilnya di bawah 10%, sistem mulai kekurangan memori.

2. Pada baris Swap, apakah kolom used bernilai 0? Jika lebih dari 0, kernel sudah pernah memindahkan data ke disk karena RAM tidak cukup.

3. Perhatikan field Cached dan Buffers di /proc/meminfo. Nilai ini sesuai dengan kolom buff/cache pada free -h.

Jawaban : 

Berdasarkan hasil eksekusi perintah free -h dan pembacaan file /proc/meminfo, berikut adalah analisis mendalam mengenai kondisi memori sistem:

### Analisis Praktikum 10.1 Melihat Penggunaan Memori

#### 1. Perhitungan Persentase Memori Tersedia

Untuk menentukan apakah sistem dalam kondisi sehat atau kritis, kita menggunakan rumus:
Persentase Tersedia = (Available / Total) × 100%

<img width="470" height="140" alt="image" src="https://github.com/user-attachments/assets/822ebc00-d858-4f82-a276-a5f856126712" />

Kesimpulan: Jika hasil perhitungan berada di bawah 10%, sistem akan mulai melakukan "Memory Pressure" yang bisa menyebabkan lag karena kernel harus bekerja keras membersihkan cache untuk memberikan ruang bagi aplikasi.

#### 2. Analisis Penggunaan Swap (Baris Swap)

- Pada baris Swap, kita memperhatikan kolom used:

- Jika used = 0: Sistem bekerja sepenuhnya di dalam RAM fisik. Ini adalah kondisi ideal untuk performa maksimal.

- Jika used > 0: Kernel telah memindahkan data yang jarang digunakan dari RAM ke disk (swap).

    - Implikasi: Hal ini menandakan RAM fisik pernah mencapai titik jenuh. Jika angka ini terus meningkat diikuti dengan aktivitas si (swap in) dan so (swap out) yang tinggi pada vmstat, maka performa sistem akan menurun drastis karena kecepatan disk jauh lebih lambat dibanding RAM.
 
#### 3. Korelasi Field Meminfo dan Free

- Terdapat hubungan langsung antara data mentah di kernel dengan tampilan perintah free -h:

- Field Cached: Berisi data file yang dibaca dari disk dan disimpan di RAM agar akses berikutnya lebih cepat.

- Field Buffers: Berisi metadata file dan struktur data sementara untuk operasi I/O.

- Kolom buff/cache pada free -h: Merupakan penjumlahan dari Buffers + Cached + SReclaimable.

  Analisis:
Besarnya nilai buff/cache bukan berarti RAM "habis" secara negatif. Sebaliknya, Linux menggunakan RAM yang menganggur untuk mempercepat sistem. Memori ini bersifat "pinjaman" dan akan langsung dilepaskan oleh kernel jika aplikasi membutuhkan memori tambahan.

## Studi Kasus 10.1 Server Lambat karena Memori
```
free -h
top
```
Tekan M : 
<img width="448" height="402" alt="image" src="https://github.com/user-attachments/assets/2a346af9-5a6d-4088-9b60-be5b61a5724c" />

Tekan q : untuk keluar.

Analisis:

1. Apakah nilai available sangat kecil (misalnya di bawah 200 MB pada server dengan RAM 2 GB)? Jika ya, server kemungkinan kekurangan memori.

2. Apakah kolom used pada baris Swap lebih dari 0? Jika ya, kernel sedang menggunakan swap, yang berarti performa menurun.

3. Di tampilan top, proses apa yang memiliki %MEM terbesar? Proses tersebut menjadi kandidat utama penyebab lambatnya server.

Jawaban : 

### Analisis Diagnosa Kesehatan Server (Aspek Memori)

Berdasarkan hasil pemantauan menggunakan script diagnosa-server.sh dan perintah top, berikut adalah parameter evaluasi untuk menentukan kondisi kesehatan server:

#### 1. Ambang Batas Memori Tersedia (Available Memory)

- Memori available adalah indikator paling akurat dibandingkan free. Indikator ini menghitung seberapa banyak memori yang bisa dialokasikan tanpa menyebabkan sistem melakukan swapping berat.

- Kondisi Kritis: Jika nilai available < 10% dari total RAM (Contoh: < 200 MB pada RAM 2 GB).

- Gejala: Aplikasi akan terasa lambat merespon (latensi tinggi), dan jika mencapai titik nadir, kernel akan memicu OOM (Out Of Memory) Killer yang akan mematikan proses secara paksa (biasanya database atau web server).

#### 2. Status Baris Swap (Used Swap)

Swap adalah "katup penyelamat" saat RAM penuh, namun memiliki konsekuensi performa.

- Analisis Kolom used:

- Bernilai 0: Ideal. RAM fisik masih mencukupi semua beban kerja.

- Lebih dari 0: Kernel sudah mulai memindahkan data dari RAM ke disk.

  - Dampak Performa: Karena kecepatan baca/tulis disk (SSD/HDD) jauh lebih lambat dibanding RAM, penggunaan swap yang aktif akan menyebabkan penurunan performa sistem secara signifikan (disk thrashing).

#### 3. Identifikasi Proses Dominan (%MEM)

Melalui perintah top (dengan menekan tombol M untuk mengurutkan berdasarkan memori), kita dapat melihat "pelaku utama" penggunaan sumber daya.

- Evaluasi Proses:

  - Jika satu proses mengambil > 50% RAM, periksa apakah itu normal (misalnya database besar) atau terjadi Memory Leak (kebocoran memori).

  - Kandidat Utama: Proses dengan %MEM terbesar adalah target pertama yang perlu dioptimalkan atau direstart jika server mengalami kemacetan total.
 
## Praktikum 10.2 Mengamati Aktivitas Paging
```
vmstat 1 5
```
<img width="363" height="75" alt="image" src="https://github.com/user-attachments/assets/1b7f2915-32db-4407-832e-19ee597f58d1" />

Analisis:

 1. Amati nilai si dan so pada kelima baris. Pada sistem normal dengan RAM cukup, kedua nilai ini selalu 0.

 2. Jika nilai si atau so sesekali muncul lebih dari 0, artinya pernah ada aktivitas swap. Ini masih wajar jika tidak terus-menerus.

 3. Jika si dan so terus-menerus lebih dari 0, sistem dalam kondisi memory pressure serius — performa turun drastis karena akses disk jauh lebih lambat dari RAM.

 4. Perhatikan juga kolom free (RAM kosong) dan buff (buffer) untuk memahami kondisi keseluruhan RAM saat itu.

Jawaban : 

### Analisis Aktivitas Paging dan Swap (vmstat)

Analisis ini merangkum hasil pengamatan terhadap aktivitas memori virtual melalui perintah vmstat 1 5. Fokus utama analisis terletak pada kolom si (Swap-In) dan so (Swap-Out).

#### 1. Interpretasi Nilai SI dan SO

Kolom ini mengukur jumlah memori yang dipindahkan antara RAM dan ruang swap (dalam KB/detik).

- si (Swap-In): Data yang dipindahkan dari disk kembali ke RAM.

- so (Swap-Out): Data yang dipindahkan dari RAM ke disk untuk membebaskan ruang.

#### 2. Dampak terhadap Performa

Akses ke disk (bahkan SSD sekalipun) ribuan kali lebih lambat daripada akses ke RAM fisik. Jika nilai so tinggi secara berkelanjutan:

- Sistem akan mengalami Disk Thrashing (disk sibuk terus menerus).

- Aplikasi akan terasa membeku (freezing) atau sangat lambat merespon input user.

- Penggunaan CPU akan terlihat tinggi pada bagian %wa (I/O Wait) karena CPU menunggu data dari disk.

#### 3. Hubungan dengan Kolom Free dan Buff

Untuk mendapatkan gambaran utuh, si/so harus dibaca bersamaan dengan:

- free: Jika nilai ini sangat rendah (mendekati 0) bersamaan dengan so yang tinggi, maka konfirmasi kekurangan memori valid.

- buff (Buffer): Menunjukkan memori yang digunakan untuk menyimpan metadata disk. Jika nilai ini mendadak turun drastis, artinya kernel sedang "berusaha keras" mengosongkan segala jenis cache demi menyelamatkan proses yang sedang berjalan.

Kesimpulan Diagnosa: Aktivitas swap yang intensif (bukan sekadar sesekali) adalah sinyal terkuat bahwa server atau komputer memerlukan optimasi aplikasi atau penambahan RAM fisik.

## Praktikum 10.3 Membuat dan Mengonfigurasi Swap File
```
sudo fallocate -l 512 M / swapfile - week10
sudo chmod 600 / swapfile - week10
sudo mkswap / swapfile - week10
sudo swapon / swapfile - week10
swapon -- show
free -h
cat / proc / sys / vm / swappiness
sudo sysctl vm . swappiness =10
cat / proc / sys / vm / swappiness
```
<img width="340" height="172" alt="image" src="https://github.com/user-attachments/assets/b576fc12-9b26-471b-bc86-6b78abfa58e1" />

Analisis:

1. Berapa nilai swappiness default? Apa artinya bagi perilaku kernel dalam menggunakan swap?

2. Setelah diubah ke 10, konfirmasi nilai berubah pada output cat kedua. Apa dampak nilai 10 terhadap penggunaan swap dibanding nilai 60?

3. Apakah entri /swapfile-week10 muncul di swapon –show? Jika tidak, pastikan Langkah 2 (chmod 600) sudah dijalankan sebelum Langkah 3.

Jawaban : 

### Analisis Konfigurasi Swappiness dan Verifikasi Swap

Laporan ini mengevaluasi pengaruh parameter kernel swappiness terhadap perilaku manajemen memori dan memverifikasi keberhasilan aktivasi file swap.

#### 1. Analisis Nilai Swappiness Default

- Nilai Default: Umumnya pada sebagian besar distro Linux (seperti Ubuntu/Debian), nilai default adalah 60.

- Makna Perilaku: Nilai 60 adalah nilai "seimbang". Artinya, kernel akan mulai memindahkan data yang jarang digunakan dari RAM ke swap bahkan sebelum RAM benar-benar penuh. Hal ini dilakukan untuk menjaga agar page cache (untuk mempercepat akses file) tetap memiliki ruang yang cukup di RAM.

#### 2. Perubahan ke Nilai 10

- Konfirmasi Perubahan: Setelah menjalankan sudo sysctl vm.swappiness=10, output cat /proc/sys/vm/swappiness seharusnya menunjukkan angka 10.

- Dampak Perubahan (10 vs 60):

  - Nilai 10: Membuat kernel sangat "enggan" untuk melakukan swapping. Kernel akan memaksimalkan penggunaan RAM fisik sebanyak mungkin dan hanya akan menggunakan swap jika RAM benar-benar hampir habis (kritis).

  - Keuntungan: Meningkatkan respon sistem pada desktop atau server dengan RAM kecil karena meminimalkan latensi akses disk yang lambat.

  - Kerugian: Jika RAM benar-benar habis secara mendadak, sistem mungkin akan terasa membeku sesaat karena harus melakukan swapping massal dalam waktu singkat.

## Praktikum 10.4 Monitoring Memory
```
ps aux -- sort = -% mem | head
top
```
Di dalam top: M = urutkan berdasarkan memori, P = urutkan berdasarkan
CPU, q = keluar.

<img width="640" height="88" alt="image" src="https://github.com/user-attachments/assets/aa05b156-2f6d-4d64-a3e5-798ef5a6db9e" />

<img width="637" height="401" alt="image" src="https://github.com/user-attachments/assets/a44ffb32-bfe2-4647-a56c-3dabc2e888cd" />

<img width="635" height="402" alt="Cuplikan layar 2026-04-29 083518" src="https://github.com/user-attachments/assets/6ac1891a-be10-408d-b225-de9bc5b38dd7" />

Analisis:

1. Proses apa yang berada di urutan pertama? Catat nilai %MEM dan RSS-nya.

2. Konversikan RSS dari KB ke MB (bagi 1024). Misalnya, RSS=524288 berarti proses menggunakan 512 MB RAM. Apakah wajar untuk jenis program tersebut?

3. Mengapa VSZ selalu lebih besar dari RSS pada proses yang sama?

4. Apakah urutan proses di ps konsisten dengan tampilan top saat diurutkan berdasarkan %MEM?

Jawaban : 

### Analisis Detail Penggunaan Memori per Proses

Berdasarkan hasil eksekusi perintah ps aux --sort=-%mem dan pengamatan pada panel top, berikut adalah poin-poin analisis mengenai konsumsi memori pada tingkat proses:

#### 1. Identifikasi Proses Teratas

Dari daftar proses yang muncul, proses di urutan pertama biasanya merupakan aplikasi dengan beban kerja tinggi (seperti Browser, Database, atau IDE).

- Contoh Temuan: Proses firefox atau mysqld.

- Nilai %MEM: (Misalnya: 8.5%) Menunjukkan persentase RAM fisik yang digunakan proses tersebut dari total RAM.

- Nilai RSS: (Misalnya: 685420 KB).

#### 2. Konversi RSS (Resident Set Size)

RSS adalah indikator paling penting karena menunjukkan berapa banyak RAM fisik yang benar-benar digunakan oleh proses saat ini (bukan sekadar yang dipesan).

- Rumus: MB = RSS / 1024

- Contoh: Jika RSS = 685420 KB, maka 685420 / 1024 ≈ 669.3 MB.

- Evaluasi Kewajaran: - Untuk web browser (seperti Chrome/Firefox), penggunaan 500MB - 1GB adalah wajar karena banyaknya tab dan konten media.

  - Untuk perintah sederhana seperti ls atau cat, jika RSS mencapai ratusan MB, maka ada indikasi ketidakwajaran.

#### 3. Perbedaan VSZ (Virtual Size) vs RSS

Pada output ps, nilai VSZ hampir selalu jauh lebih besar daripada RSS.

- Alasan: - VSZ mencakup seluruh memori yang bisa diakses proses, termasuk library yang di-share, memori di swap, dan memori yang sudah dialokasikan tapi belum pernah digunakan.

  - RSS hanya menghitung porsi yang saat ini berada di RAM fisik.

- Analogi: VSZ adalah ukuran lemari yang disediakan, sedangkan RSS adalah jumlah barang yang benar-benar ada di dalam lemari saat ini.

#### 4. Konsistensi Antara ps dan top

- Hasil: Urutan proses antara ps aux --sort=-%mem dan top (setelah menekan M) secara umum konsisten.

- Alasan: Keduanya mengambil data dari sumber yang sama di kernel, yaitu direktori /proc/[pid]/statm.

- Perbedaan Kecil: Sedikit perbedaan nilai mungkin terjadi karena perbedaan waktu pengambilan data (sampling rate) antara ps (snapshot sekali jalan) dan top (dinamis/real-time).

## Praktikum 10.5 Script Monitor Memori
```
cd ~/ praktikum - os / week10 - memory
nano monitor - memori . sh
#!/ bin / bash
set - euo pipefail
THRESHOLD =20
echo "=== Monitor Memori ==="
date
echo
free -h
echo
AVAIL = $ ( free | awk '/ Mem / { printf "% d " , $7 / $2 *100} ')
if [ " $AVAIL " - lt " $THRESHOLD " ]; then
echo " PERINGATAN : Memori tersedia hanya $ { AVAIL }%!"
else
echo " Status : Memori tersedia $ { AVAIL }% ( normal ) "
fi
echo
echo " - - - 5 Proses Memori Tertinggi - - -"
ps aux -- sort = -% mem | head -n 6 | tail -n 5
chmod + x monitor - memori . sh
bash monitor - memori . sh
```
<img width="643" height="138" alt="image" src="https://github.com/user-attachments/assets/a20168a3-2ff7-418c-b5a4-11c82f8bcbbb" />

Analisis:

1. Variabel THRESHOLD=20 menetapkan batas persentase. Perintah free | awk ’/Mem/ {printf "%d", $7/$2*100}’ mengambil kolom ke-7 (available) dibagi kolom ke-2 (total) dari baris Mem, lalu dikalikan 100 untuk menghasilkan persentase bilangan bulat.

2. Kondisi if [ "$AVAIL" -lt "$THRESHOLD" ] bernilai benar jika persentase memori tersedia di bawah 20.

3. Ubah THRESHOLD menjadi 90 dan jalankan ulang. Apa yang berubah pada output? Mengapa demikian?

Jawaban : 

### Analisis Logika dan Thresholding Script Monitoring

Analisis ini menjelaskan bagaimana script monitor-memori.sh atau diagnosa-server.sh mengambil keputusan mengenai status kesehatan sistem berdasarkan parameter yang ditentukan.

#### 1. Mekanisme Pengambilan Data (AWK)

Baris perintah free | awk '/Mem/ {printf "%d", $7/$2*100}' bekerja dengan cara sebagai berikut:

- free: Mengeluarkan data penggunaan memori dalam satuan bytes.

- awk '/Mem/': Memilih baris yang mengandung kata "Mem".

- $7/$2*100: Mengambil nilai kolom ke-7 (available) dibagi kolom ke-2 (total), lalu dikalikan 100.

- printf "%d": Memastikan output berupa bilangan bulat (integer) agar bisa dibandingkan oleh operator Bash -lt.

#### 2. Logika Kondisional Bash

Perintah if [ "$AVAIL" -lt "$THRESHOLD" ] adalah penentu peringatan:

- $AVAIL: Nilai persentase memori tersedia hasil perhitungan AWK.

- $THRESHOLD: Batas aman yang ditetapkan (default = 20).

- Interpretasi: Jika memori yang tersedia lebih kecil dari 20%, maka kondisi dianggap benar (TRUE) dan script akan menjalankan perintah di dalam blok if (biasanya mencetak pesan "KRITIS").

#### 3. Eksperimen Perubahan Threshold (20 menjadi 90)

- Apa yang berubah pada output?

Output script hampir dipastikan akan berubah dari "Normal" menjadi "KRITIS - perlu tindakan segera", meskipun beban kerja server sebenarnya ringan.

- Mengapa demikian?

  - Logika Perbandingan: Dengan THRESHOLD=90, Anda memerintahkan script untuk menganggap sistem dalam bahaya jika memori tersedia kurang dari 90%.

  - Kondisi Realistis: Sangat jarang sebuah sistem Linux memiliki memori tersedia di atas 90% karena Linux secara cerdas menggunakan sisa RAM untuk cache (agar sistem lebih cepat).

  - Hasil: Karena nilai $AVAIL kemungkinan besar berada di kisaran 10% - 70%, maka nilai tersebut hampir pasti lebih kecil dari 90 ($AVAIL < 90), sehingga kondisi if selalu terpenuhi dan memicu peringatan palsu (false positive).

## Studi Kasus 10.2 Gagal Akses File
```
mkdir -p ~/ praktikum - os / week10 - memory / syscall - case
cd ~/ praktikum - os / week10 - memory / syscall - case
echo " PORT =8080" > app . conf
ls -l app . conf
cat app . conf
chmod 000 app . conf
cat app . conf
chmod 644 app . conf
cat app . conf
```
<img width="488" height="113" alt="image" src="https://github.com/user-attachments/assets/9360a1aa-8684-49fe-b598-00cf85b43f65" />

Analisis:

1. Mengapa cat menghasilkan Permission denied setelah chmod 000? System call apa yang gagal?

2. Apa perbedaan pesan error Permission denied vs No such file or directory? Coba rm app.conf lalu cat app.conf untuk melihat perbedaannya.

3. Permission 644 berarti apa untuk owner, group, dan others?

Jawaban : 

### Analisis Izin File dan Interaksi System Call

Analisis ini mengevaluasi bagaimana sistem operasi mengelola hak akses dan memberikan respon terhadap permintaan user space yang tidak valid.

#### 1. Analisis "Permission Denied" pada chmod 000

Ketika menjalankan chmod 000 app.conf, kita menghapus semua hak akses (read, write, execute) untuk semua orang, termasuk pemilik file.

- Mengapa gagal? Meskipun Anda adalah pemilik file, perintah cat mencoba membuka file untuk dibaca. Karena izin baca (r) telah dicabut, kernel menolak permintaan tersebut.

- System Call yang gagal: - System call utama yang gagal adalah openat() (atau open()).

  - Sebelum proses cat dapat membaca isi file, ia harus mendapatkan file descriptor dari kernel melalui openat(). Kernel memeriksa metadata file (inode), melihat bahwa izin akses adalah 000, dan mengembalikan error -EACCES (Permission denied).

#### 2. Perbedaan Pesan Error
<img width="468" height="129" alt="image" src="https://github.com/user-attachments/assets/6db00e2e-216d-45ea-b311-c29322fdc3b3" />

#### 3. Arti Permission 644

Angka 644 adalah representasi oktal dari bit izin akses:

- Digit 1 (6): Owner (Pemilik)

  - 6 dalam biner adalah 110 (Read + Write).

  - Pemilik bisa membaca dan mengubah isi file.

- Digit 2 (4): Group

  - 4 dalam biner adalah 100 (Read Only).

  - Anggota grup yang sama dengan file hanya bisa membaca tanpa bisa mengubah.

- Digit 3 (4): Others (Lainnya)

  - 4 dalam biner adalah 100 (Read Only).

  - Semua pengguna lain di sistem hanya bisa membaca file tersebut.

## Praktikum 10.6 Mengamati System Call dengan strace
```
strace ls 2 >&1 | head -n 30
strace -c ls
strace -c ls / etc 2 >&1 | tail -5
```
<img width="641" height="402" alt="image" src="https://github.com/user-attachments/assets/c7a87ce9-fa37-4f2b-9f09-c1293fcdd7f4" />

<img width="465" height="266" alt="image" src="https://github.com/user-attachments/assets/c3f93b1b-b507-4801-a829-e9a72a917e05" />

Analisis:

1. Dari output Langkah 1, identifikasi minimal 4 system call berbeda. Jelaskan fungsi singkat masing-masing berdasarkan argumen yang terlihat.

2. Dari ringkasan strace -c, system call mana yang paling sering dipanggil? Mengapa?

3. Apakah ada system call dengan errors lebih dari 0? Apakah itu berarti program bermasalah, ataukah bagian normal dari logika program?

4. Apakah jumlah system call berbeda antara ls dan ls /etc? Faktor apa yang menyebabkan perbedaan tersebut?

Jawaban : 

### Analisis Praktikum 10.6 Mengamati System Call dengan strace

#### 1. Identifikasi dan Fungsi System Call

Berdasarkan output dari langkah-langkah praktikum, berikut adalah 4 system call utama beserta fungsinya:
<img width="470" height="242" alt="image" src="https://github.com/user-attachments/assets/c2040ecf-fa7f-4a39-88ce-f0375c6a32b2" />

#### 2. Analisis Statistik (strace -c)

Berdasarkan ringkasan statistik, system call yang paling sering dipanggil biasanya adalah openat, fstat, atau mmap.

- Alasan: Program ls perlu memeriksa banyak library sistem (shared objects) saat pertama kali berjalan. Selain itu, jika menjalankan ls pada direktori yang padat, kernel harus melakukan operasi buka (openat) dan ambil status (fstat) untuk setiap file yang ada di dalam direktori tersebut agar bisa menampilkan informasi seperti ukuran, pemilik, dan izin file.

#### 3. Penanganan Error pada System Call

Dalam output strace, sering terlihat nilai kembalian -1 diikuti kode error seperti ENOENT (No such file or directory).

- Apakah program bermasalah? Tidak selalu.

- Analisis: Hal ini biasanya merupakan bagian normal dari logika program. Misalnya, program mencoba mencari file konfigurasi di beberapa lokasi standar (seperti /etc/ld.so.preload). Jika file tersebut tidak ditemukan di lokasi pertama, kernel mengembalikan error ENOENT, lalu program secara otomatis beralih mencari di lokasi berikutnya tanpa berhenti (crash).

#### 4. Perbandingan ls vs ls /etc

Jumlah system call antara keduanya akan menunjukkan perbedaan yang signifikan.

- Faktor Penyebab Perbedaan:

  - Volume Data: Direktori /etc biasanya berisi ratusan file, sementara direktori kerja saat ini mungkin hanya berisi sedikit file.

  - Iterasi Kernel: Untuk setiap file di /etc, perintah ls harus memanggil getdents64 (untuk membaca entri direktori) dan fstat berkali-kali. Semakin banyak file yang harus dibaca, semakin banyak permintaan system call yang dikirimkan ke kernel.

  - Metadata: Jika menggunakan opsi -l (misal: ls -l /etc), jumlah system call akan membengkak lebih jauh karena program harus meminta informasi detail (UID, GID, Timestamp) untuk setiap entri.

## Tugas Praktikum
```
mkdir -p ~/ praktikum - os / week10 - memory
cd ~/ praktikum - os / week10 - memory
```
<img width="434" height="25" alt="image" src="https://github.com/user-attachments/assets/baa4197e-c4d6-4921-9766-f58051b9ec8e" />

### Tugas 10.1 Audit Penggunaan Memori Sistem
```
nano ~/ praktikum - os / week10 - memory / memory - audit . sh
#!/ bin / bash
set - euo pipefail
LAPORAN =" memory - report . txt "
{
echo "=== LAPORAN MEMORI SISTEM ==="
date
echo
echo " - - - Ringkasan free -h - - -"
free -h
echo
echo " - - - / proc / meminfo - - -"
cat / proc / meminfo | head -n 20
} > " $LAPORAN "
echo " Laporan disimpan ke : $LAPORAN "
cat " $LAPORAN "
chmod + x ~/ praktikum - os / week10 - memory / memory - audit . sh
cd ~/ praktikum - os / week10 - memory
bash memory - audit . sh
```
<img width="565" height="38" alt="image" src="https://github.com/user-attachments/assets/2627f97c-c6c1-4255-bcb5-127d63768fa2" />

Analisis

1. Hitung persentase memori tersedia (available / total × 100%). Apakah sistem dalam kondisi normal?

2. Mengapa buff/cache tidak dihitung sebagai memori yang terpakai dari sudut pandang ketersediaan untuk aplikasi?

3. Dari /proc/meminfo, apakah SwapTotal lebih besar dari 0? Berapa nilai SwapFree?

Jawaban : 

### Analisis Lanjutan Audit Memori Sistem

Berdasarkan pengamatan pada file /proc/meminfo dan perintah free -h, berikut adalah penjelasan teknis mengenai manajemen memori di Linux:

#### 1. Mengapa buff/cache Tidak Dihitung sebagai Memori "Terpakai"?

- Sifatnya "Pinjaman": Linux menggunakan RAM yang sedang menganggur untuk menyimpan data file yang sering diakses (cache) dan metadata disk (buffer). Ini bertujuan untuk mempercepat performa sistem secara keseluruhan.

- Dapat Dilepaskan Seketika: Memori ini bersifat evictable (dapat diusir). Jika ada aplikasi (seperti browser atau database) yang tiba-tiba membutuhkan RAM dalam jumlah besar, kernel akan langsung menghapus data cache tersebut dan memberikannya kepada aplikasi tanpa perlu melakukan proses pembersihan yang rumit.

- Ketersediaan Nyata: Oleh karena itu, kolom available pada free -h dihitung dengan rumus: free + (porsi buff/cache yang bisa dilepaskan).

#### 2. Analisis Swap dari /proc/meminfo

- SwapTotal > 0? - Ya, jika sudah menjalankan langkah pembuatan swap file sebelumnya, nilai SwapTotal akan menunjukkan angka di atas 0 (misalnya dalam kB). Ini menandakan ruang laci cadangan di disk sudah terdaftar di kernel.

- Nilai SwapFree:

  - SwapFree menunjukkan berapa banyak ruang di swap file yang belum terisi.

  - Jika sistem memiliki RAM yang melimpah dan beban kerja ringan, nilai SwapFree biasanya akan sama atau hampir sama dengan SwapTotal. Hal ini menunjukkan kernel belum merasa perlu memindahkan data ke disk karena RAM fisik masih sangat mencukupi.

### Tugas 10.2 Identifikasi Proses dengan Memori Tertinggi
```
ps aux -- sort = -% mem | head -n 10 > top - memory - process . txt
cat top - memory - process . txt
```
<img width="641" height="107" alt="image" src="https://github.com/user-attachments/assets/eb1811eb-e513-4852-86d9-19d98f91052b" />

Analisis

1. Proses apa di urutan pertama? Catat nilai %MEM dan RSS.

2. Konversikan RSS ke MB (bagi 1024). Apakah wajar?

3. Jumlahkan %MEM dari 5 proses teratas. Berapa persen RAM yang mereka gunakan bersama?

Jawaban : 

### Analisis Tugas 10.2: Identifikasi Proses Memori Tertinggi

Analisa ini menganalisis data proses yang diambil menggunakan perintah ps aux --sort=-%mem sebagaimana terlihat pada hasil eksekusi script diagnosa Anda.

#### 1. Identifikasi Proses Urutan Pertama

- Nama Proses: node (atau proses lain yang muncul paling atas di tabel "5 Proses Teratas").

- Nilai %MEM: (Contoh: 4.2%) — Ini adalah persentase RAM fisik yang digunakan proses tersebut.

- Nilai RSS: (Contoh: 168432 KB) — Resident Set Size atau jumlah RAM fisik yang dikunci oleh proses ini.

#### 2. Konversi RSS ke Megabyte (MB)

- Rumus: RSS / 1024

- Perhitungan: 168432 / 1024 = 164.48 MB

- Evaluasi Kewajaran: Penggunaan sebesar ~164 MB untuk proses seperti Node.js atau aplikasi sistem adalah sangat wajar. Proses baru dianggap mencurigakan (indikasi memory leak) jika menggunakan RAM hingga hitungan GB secara tidak terduga untuk tugas sederhana.

#### 3. Akumulasi Penggunaan 5 Proses Teratas

- Proses 1: 4.2%

- Proses 2: 1.5%

- Proses 3: 1.2%

- Proses 4: 0.8%

- Proses 5: 0.5%

- Total Akumulasi: 8.2%

### Tugas 10.3 Membuat dan Memverifikasi Swap File
```
sudo fallocate -l 256 M / swapfile - tugas - week10
sudo chmod 600 / swapfile - tugas - week10
sudo mkswap / swapfile - tugas - week10
sudo swapon / swapfile - tugas - week10
{
echo "=== VERIFIKASI SWAP ==="
swapon -- show
echo
free -h
} > swap - check . txt
cat swap - check . txt
```
<img width="367" height="176" alt="image" src="https://github.com/user-attachments/assets/12b3c93e-2f07-4a4e-9f06-51f929c57819" />

Bersihkan setelah selesai:
sudo swapoff /swapfile-tugas-week10 && sudo rm /swapfile-tugas-week10

Analisis

1. Identifikasi kolom NAME, TYPE, SIZE, dan USED pada output swapon –show.

2. Apakah nilai total pada baris Swap di free -h bertambah 256 MB?

3. Mengapa permission 600 penting? Apa risiko jika diatur ke 644?

Jawaban : 

### Analisis Tugas 10.3: Verifikasi dan Keamanan Swap File

Analisa ini mengevaluasi hasil konfigurasi swap file tambahan sebesar 256 MB serta pentingnya aspek keamanan pada file tersebut.

#### 1. Identifikasi Kolom swapon --show

Berdasarkan output perintah swapon --show, berikut adalah penjelasan untuk setiap kolom:

- NAME: Jalur lokasi file swap (Contoh: /swapfile-tugas-week10). Ini menunjukkan identitas file yang digunakan sebagai area swap.

- TYPE: Jenis area swap. Untuk praktikum ini, tipenya adalah file. Jika menggunakan partisi khusus, tipenya adalah partition.

- SIZE: Kapasitas total yang dialokasikan untuk swap file tersebut (dalam unit seperti KB atau MB).

- USED: Jumlah memori yang saat ini sedang dipindahkan dari RAM ke file swap tersebut. Jika sistem memiliki banyak RAM bebas, nilai ini biasanya 0B.

#### 2. Verifikasi Penambahan Kapasitas

- Hasil Pengamatan: Setelah menjalankan sudo swapon, baris Swap pada output free -h seharusnya menunjukkan kenaikan nilai pada kolom total.

- Analisis: Jika sebelumnya total swap adalah 512 MB dan Anda menambahkan file 256 MB, maka total baru akan menjadi sekitar 768 MB. Hal ini mengonfirmasi bahwa kernel telah berhasil menggabungkan semua area swap yang aktif ke dalam satu total kapasitas virtual.

#### 3. Analisis Keamanan (Permission 600)

Keamanan pada file swap sangatlah krusial karena isi dari file tersebut adalah salinan data langsung dari RAM.

### Tugas 10.4 Analisis System Call dengan strace
```
strace -c ls 2 > strace - summary . txt
strace ls / etc 2 > strace - ls - etc . txt
cat strace - summary . txt
```
<img width="632" height="252" alt="image" src="https://github.com/user-attachments/assets/3250217b-f516-4f28-86ec-918abefb3785" />

<img width="323" height="214" alt="image" src="https://github.com/user-attachments/assets/d1e2cb0b-78b4-4f10-b875-1c18de0d5e37" />

Analisis

1. Sebutkan minimal 5 system call dari strace-summary.txt beserta fungsi singkatnya.

2. System call mana yang paling sering dipanggil? Mengapa?

3. Apakah ada errors lebih dari 0? Apakah program tetap berjalan normal meskipun ada kegagalan tersebut?

Jawaban : 

### Analisis Tugas 10.4: Statistik System Call (strace -c)

Analisa ini menganalisis ringkasan statistik interaksi antara program ls dan kernel Linux berdasarkan data yang dihasilkan dalam file strace-summary.txt.

#### 1. Identifikasi 5 System Call dan Fungsinya

Berdasarkan data statistik strace -c ls, berikut adalah 5 system call yang sering muncul beserta fungsi teknisnya:
<img width="467" height="215" alt="image" src="https://github.com/user-attachments/assets/2983d12b-ed4e-4ec8-a4b2-a4b06b14e80f" />
#### 2. System Call Paling Dominan

- Observasi: Biasanya openat atau fstat menduduki urutan teratas dalam jumlah panggilan (calls).

- Alasan: Program ls harus memanggil banyak library sistem (file .so) sebelum mulai bekerja. Selain itu, program ini harus memeriksa setiap objek di dalam direktori satu per satu. Jika terdapat 50 file, maka program mungkin melakukan 50 panggilan fstat untuk mengetahui properti masing-masing file tersebut.

#### 3. Analisis Error pada Statistik

- Apakah ada errors > 0? Ya, biasanya kolom errors akan menunjukkan angka di atas 0 untuk system call seperti access atau openat.

- Interpretasi: Hal ini tidak berarti program bermasalah.

- Alasan: Ini adalah bagian dari logika program yang bersifat eksploratif. Misalnya, program mencoba mencari file konfigurasi opsional di jalur /etc/ld.so.preload. Jika file tersebut tidak ada, kernel mengembalikan error ENOENT. Program sudah dirancang untuk menangani hal ini dengan cara melanjutkan pencarian ke lokasi lain atau menggunakan nilai default. Program tetap berjalan normal selama error yang terjadi bukan pada komponen kritikal sistem.

### Tugas 10.5 Studi Kasus Diagnosa Server Lambat
```
nano ~/ praktikum - os / week10 - memory / diagnosa - server . sh
#!/ bin/ bash
set - euo pipefail
LAPORAN =" diagnosa -server - lambat .txt"
WARN_MEM = false
WARN_SWAP =0
cek_memori () {
echo " --- Kondisi Memori ---"
free -h
echo
AVAIL_PCT = $ ( free | awk '/Mem/ { printf "%d" , $7/$2 *100}
')
if [ " $AVAIL_PCT " - lt 20 ]; then
echo " PERINGATAN : Memori tersedia hanya ${
AVAIL_PCT }%"
WARN_MEM = true
fi
}
cek_swap () {
echo " --- Penggunaan Swap ---"
swapon -- show 2 >/ dev / null || echo " Tidak ada swap
aktif "
echo
WARN_SWAP = $ ( free | awk '/ Swap / { print $3}')
if [ " $WARN_SWAP " - gt 0 ]; then
echo " INFO : Swap digunakan (${ WARN_SWAP } kB)"
fi
}
cek_proses () {
echo " --- 10 Proses Memori Tertinggi ---"
ps aux -- sort = -% mem | head -n 11
echo
}
cek_paging () {
echo " --- Aktivitas Paging (5 sampel ) ---"
vmstat 1 5
echo
}
ringkasan () {
echo "=== RINGKASAN ==="
if [ " $WARN_MEM " = true ]; then
echo "- Memori : KRITIS - perlu tindakan segera "
else
echo "- Memori : normal "
fi
if [ " $WARN_SWAP " - gt 0 ]; then
echo "- Swap : aktif - pantau aktivitas paging "
else
echo "- Swap : tidak digunakan "
fi
}
{echo "=== LAPORAN DIAGNOSA SERVER ==="
date
echo
cek_memori
cek_swap
cek_proses
cek_paging
ringkasan
} | tee " $LAPORAN "
echo
echo " Laporan disimpan ke: $LAPORAN "
chmod + x ~/ praktikum - os / week10 - memory / diagnosa - server . sh
cd ~/ praktikum - os / week10 - memory
bash diagnosa - server . sh
```
<img width="940" height="496" alt="image" src="https://github.com/user-attachments/assets/01122e05-ecb6-4cad-9f71-894866585f57" />

<img width="341" height="79" alt="image" src="https://github.com/user-attachments/assets/0a8cf34b-3d99-40b6-b741-bfaaa41965c1" />

Analisis

1. Jelaskan peran masing-masing fungsi: cek_memori, cek_swap, cek_proses, cek_paging, dan ringkasan. Mengapa diagnosa dipecah menjadi fungsi terpisah?

2. Berdasarkan bagian RINGKASAN, apakah kondisi sistem normal atau kritis? Jelaskan berdasarkan nilai threshold yang digunakan script.

3. Mengapa script menggunakan tee "$LAPORAN" bukan redirection biasa > "$LAPORAN"? Apa keuntungannya?

4. Dari output cek_paging, apakah ada aktivitas si atau so? Jika ada, apa implikasinya terhadap performa server?

Jawaban : 

### Analisis Tugas 10.5 Studi Kasus Diagnosa Server Lambat

#### 1. Peran Fungsi dalam Script

Diagnosa dipecah menjadi beberapa fungsi untuk menerapkan prinsip Modular Programming (pemrograman terstruktur).

- cek_memori: Mengambil data penggunaan RAM fisik (Total, Used, Available) menggunakan perintah free -h. Fungsi ini menghitung persentase memori yang benar-benar siap digunakan oleh aplikasi baru.

- cek_swap: Mengidentifikasi apakah sistem menggunakan Swap Space. Swap yang terisi adalah indikator awal bahwa RAM fisik mulai penuh.

- cek_proses: Mengurutkan dan menampilkan 5 proses yang paling banyak mengonsumsi memori agar administrator tahu "siapa" penyebab server lambat.

- cek_paging: Mengambil data dari vmstat untuk melihat aktivitas si (swap-in) dan so (swap-out) secara real-time.

- ringkasan: Mengolah data dari fungsi-fungsi sebelumnya untuk memberikan kesimpulan status (Normal/Kritis) berdasarkan logika threshold.

Mengapa dipecah? Agar kode lebih rapi, mudah dalam proses debugging (mencari kesalahan), dan memungkinkan kita untuk memanggil salah satu bagian saja tanpa harus menjalankan seluruh diagnosa jika diperlukan di masa depan.

#### 2. Kondisi Sistem (Normal vs Kritis)

Berdasarkan screenshot eksekusi terakhir:

- Status: Normal.

- Penjelasan: Script menggunakan THRESHOLD=20. Artinya, jika memori tersedia (available) masih di atas 20% dari total RAM, sistem dianggap sehat. Pada hasil praktikum, memori tersedia menunjukkan angka yang jauh lebih besar dari 20%, sehingga status yang keluar adalah "normal".

#### 3. Keuntungan Perintah tee

Script menggunakan { ... } | tee "$LAPORAN" daripada redirection biasa (>).

- Redirection (>): Hanya menyimpan hasil ke dalam file. Layar terminal akan kosong saat script dijalankan, sehingga user tidak tahu apa yang sedang terjadi.

- tee: Mengalirkan output ke dua arah sekaligus (terminal dan file).

- Keuntungan: Administrator dapat melihat hasil diagnosa secara langsung (real-time) di layar untuk tindakan cepat, namun tetap memiliki salinan permanen dalam file log untuk laporan atau audit di kemudian hari.

#### 4. Analisis Aktivitas Paging (si & so)

- Hasil: Nilai si (swap-in) dan so (swap-out) pada output praktikum adalah 0.

- Implikasi: Nilai 0 menunjukkan bahwa sistem tidak sedang melakukan paging aktif. Data tidak perlu bolak-balik dipindahkan antara RAM dan Disk.

- Jika > 0 secara terus menerus: Artinya terjadi Memory Pressure yang serius. Performa server akan turun drastis karena CPU harus menunggu proses I/O disk yang sangat lambat dibandingkan kecepatan RAM (gejala ini sering disebut thrashing).
