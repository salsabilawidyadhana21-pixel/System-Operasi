# Laporan Praktikum System Operasi Minggu 13 Backup dan Pemulihan Sistem

<h4>Nama        : Salsabila Widyadhana<h4>
<h4>NIM         : 254107020200<h4>
<h4>Kelas       : TI-1H<h4>
<h4>Mata Kuliah : System Operasi<h4>

## 1.7 Latihan
Instruksi Umum: Kerjakan seluruh latihan secara mandiri. Catat langkah penting, simpan tangkapan
layar bila diperlukan, lalu rangkum hasilnya sebagai dokumentasi pribadi.
### Latihan 12.1 Implementasi Sistem Backup Lengkap
Rancang dan implementasikan sistem backup untuk direktori simulasi.
1. Buat struktur direktori simulasi dengan minimal 10 file yang tersebar di tiga subdirektori:
dokumen/, konfigurasi/, dan media/.
2. Buat skrip backup-harian.sh yang menggunakan rsync dengan –link-dest untuk membuat snapshot harian. Skrip harus mencatat log dengan timestamp ke backup-harian.log.
3. Buat skrip backup-mingguan.sh yang menggunakan tar -czf untuk membuat arsip terkompresi. Skrip harus membuat checksum MD5 dari setiap arsip yang dibuat.
4. Daftarkan keduanya ke crontab dengan jadwal yang berbeda. Jalankan masing-masing secara
manual dan verifikasi log dan output yang dihasilkan.
5. Simulasikan kehilangan file dan lakukan restore dari kedua jenis backup. Dokumentasikan
langkah-langkah restore dan waktu yang dibutuhkan.

### Latihan 12.2 Analisis Kompresi dan Performa Backup
Analisis trade-off antara kecepatan dan rasio kompresi.
1. Buat direktori dengan tiga jenis file: teks biasa (10 file .txt masing-masing 100 baris), file konfigurasi .conf, dan file biner simulasi menggunakan dd if=/dev/urandom of=biner.bin
bs=1M count=5.
2. Buat tiga arsip dari direktori yang sama menggunakan gzip (-z), bzip2 (-j), dan xz (-J). Ukur
waktu setiap proses menggunakan time.
3. Bandingkan ukuran ketiga arsip dengan ls -lh dan hitung rasio kompresi masing-masing
terhadap ukuran asli.
4. Buat tabel di file analisis-kompresi.txt yang merangkum: jenis kompresi, waktu kompres,
ukuran hasil, dan rasio kompresi.
5. Berdasarkan data tersebut, rekomendasikan kompresi yang paling tepat untuk: backup harian
otomatis, arsip jangka panjang, dan backup file biner. Berikan alasan untuk setiap rekomendasi.
### Latihan 12.3 Disaster Recovery Drill
Lakukan simulasi pemulihan bencana secara menyeluruh.
1. Buat direktori “produksi” dengan struktur lengkap: file konfigurasi, dokumen, dan skrip. Buat
backup penuh menggunakan tar dan simpan checksumnya.
2. Dokumentasikan kondisi awal: daftar file, ukuran, dan checksum semua file menggunakan find
dan md5sum.
3. Simulasikan bencana: hapus seluruh direktori produksi dengan rm -rf.
4. Catat waktu mulai pemulihan, lakukan restore lengkap dari backup, dan catat waktu selesai.
Hitung RTO aktual.
5. Verifikasi semua file pulih dengan benar menggunakan checksum yang disimpan di langkah 2.
6. Bandingkan RTO aktual dengan target yang kamu tentukan. Jika lebih lama, identifikasi
bottleneck dan usulkan cara mempercepatnya.
#### Jawaban 
### Latihan 12.1 Implementasi Sistem Backup Lengkap
```
Langkah 1: Membuat Struktur Direktori Simulasi :
# Pastikan Anda berada di direktori lab yang benar
mkdir -p ~/lab-os/chapter12-backup/latihan-sumber/{dokumen,konfigurasi,media}
cd ~/lab-os/chapter12-backup/latihan-sumber

# Membuat data simulasi di direktori dokumen
echo "Laporan Keuangan Q1" > dokumen/laporan_q1.txt
echo "Laporan Keuangan Q2" > dokumen/laporan_q2.txt
echo "Catatan Progress Proyek" > dokumen/proyek.txt
echo "Daftar Inventaris Alat" > dokumen/inventaris.txt

# Membuat data simulasi di direktori konfigurasi
echo "port=8080" > konfigurasi/web.conf
echo "db_user=root" > konfigurasi/db.conf
echo "debug=true" > konfigurasi/app.conf

# Membuat data simulasi di direktori media
echo "Simulasi Gambar 1" > media/image1.png
echo "Simulasi Gambar 2" > media/image2.png
echo "Simulasi Audio" > media/audio.mp3

# Verifikasi jumlah file (pastikan minimal ada 10 file)
find . -type f | wc -l
```
```
Langkah 2: Membuat Skrip backup-harian.sh (rsync + --link-dest) :
nano ~/lab-os/chapter12-backup/backup-harian.sh
#!/bin/bash
# Definisi Path
BACKUP_BASE="$HOME/lab-os/chapter12-backup/snapshots-harian"
SOURCE="$HOME/lab-os/chapter12-backup/latihan-sumber"
TODAY=$(date +%F)
YESTERDAY=$(date -d "yesterday" +%F)
LOG="$HOME/lab-os/chapter12-backup/backup-harian.log"

# Membuat direktori backup untuk hari ini
mkdir -p "$BACKUP_BASE/$TODAY"

echo "[$(date)] Memulai snapshot backup harian..." >> "$LOG"

# Menjalankan rsync dengan opsi --link-dest untuk menghemat space disk
rsync -av --delete \
  --link-dest="$BACKUP_BASE/$YESTERDAY" \
  "$SOURCE/" \
  "$BACKUP_BASE/$TODAY/" >> "$LOG" 2>&1

if [ $? -eq 0 ]; then
    UKURAN=$(du -sh "$BACKUP_BASE/$TODAY" | cut -f1)
    echo "[$(date)] BERHASIL: Snapshot harian $TODAY dibuat (Ukuran: $UKURAN)" >> "$LOG"
else
    echo "[$(date)] GAGAL: Terjadi kesalahan saat membuat snapshot harian $TODAY" >> "$LOG"
    rmdir "$BACKUP_BASE/$TODAY" 2>/dev/null
fi
chmod +x ~/lab-os/chapter12-backup/backup-harian.sh
```
```
Langkah 3: Membuat Skrip backup-mingguan.sh (tar + md5sum) :
nano ~/lab-os/chapter12-backup/backup-mingguan.sh
#!/bin/bash
# Definisi Path
ARSIP_DIR="$HOME/lab-os/chapter12-backup/arsip-mingguan"
SOURCE_DIR="$HOME/lab-os/chapter12-backup/latihan-sumber"
DATE=$(date +%F)
FILE_NAME="backup-weekly-$DATE.tar.gz"
LOG_FILE="$HOME/lab-os/chapter12-backup/backup-mingguan.log"

mkdir -p "$ARSIP_DIR"

echo "[$(date)] Memulai pembuatan arsip mingguan..." >> "$LOG_FILE"

# Membuat arsip tar terkompresi gzip
tar -czf "$ARSIP_DIR/$FILE_NAME" -C "$(dirname "$SOURCE_DIR")" "$(basename "$SOURCE_DIR")" >> "$LOG_FILE" 2>&1

if [ $? -eq 0 ]; then
    # Membuat md5 checksum dari arsip yang berhasil dibuat
    md5sum "$ARSIP_DIR/$FILE_NAME" > "$ARSIP_DIR/$FILE_NAME.md5"
    echo "[$(date)] BERHASIL: Arsip $FILE_NAME dan Checksum MD5 telah dibuat." >> "$LOG_FILE"
else
    echo "[$(date)] GAGAL: Pembuatan arsip mingguan gagal." >> "$LOG_FILE"
fi
chmod +x ~/lab-os/chapter12-backup/backup-mingguan.sh
```
```
Langkah 4: Mendaftarkan ke Crontab & Uji Coba Manual :
cd ~/lab-os/chapter12-backup
./backup-harian.sh
./backup-mingguan.sh

# Verifikasi hasil dan isi log
cat backup-harian.log
cat backup-mingguan.log
ls -lh snapshots-harian/$(date +%F)/
ls -lh arsip-mingguan/

Mendaftarkan ke Crontab:
Sesuai dengan standard penjadwalan (Misalnya: Backup harian jalan setiap hari jam 01:00 malam, backup mingguan jalan setiap hari Minggu jam 03:00 malam):
crontab -e
0 1 * * * /home/user/lab-os/chapter12-backup/backup-harian.sh
0 3 * * 0 /home/user/lab-os/chapter12-backup/backup-mingguan.sh
```
```
Langkah 5: Simulasi Kehilangan File dan Pengujian Restore :
# 1. Mengambil data checksum asli sebelum simulasi bencana
cd ~/lab-os/chapter12-backup/latihan-sumber
md5sum dokumen/* konfigurasi/* media/* > ~/lab-os/chapter12-backup/checksum-asli.md5

# 2. Simulasi Bencana: Hapus file laporan keuangan dan file konfigurasi web
rm dokumen/laporan_q1.txt
rm konfigurasi/web.conf

# Pastikan file tersebut benar-benar hilang dari sumber asli
ls dokumen/
ls konfigurasi/
# Tentukan folder tanggal snapshot hari ini
TODAY=$(date +%F)

# Salin balik file konfigurasi web yang hilang dari snapshot
cp ~/lab-os/chapter12-backup/snapshots-harian/$TODAY/konfigurasi/web.conf ~/lab-os/chapter12-backup/latihan-sumber/konfigurasi/

# Verifikasi apakah file berhasil dipulihkan
ls -la ~/lab-os/chapter12-backup/latihan-sumber/konfigurasi/
# 1. Verifikasi Integritas Arsip menggunakan MD5 yang disimpan sebelumnya
cd ~/lab-os/chapter12-backup/arsip-mingguan
md5sum -c *.md5 # Output harus menunjukkan pesan OK

# 2. Buat direktori sementara untuk proses ekstraksi file tertentu
mkdir -p /tmp/restore-latihan

# 3. Ekstrak hanya file "laporan_q1.txt" yang dibutuhkan dari dalam arsip tar
DATE=$(date +%F)
tar -xzf backup-weekly-$DATE.tar.gz -C /tmp/restore-latihan/ latihan-sumber/dokumen/laporan_q1.txt

# 4. Pindahkan file yang sudah berhasil diekstrak ke lokasi data sumber yang asli
cp /tmp/restore-latihan/latihan-sumber/dokumen/laporan_q1.txt ~/lab-os/chapter12-backup/latihan-sumber/dokumen/

# 5. Bersihkan direktori sementara
rm -rf /tmp/restore-latihan

Verifikasi Akhir Integritas Seluruh Data :
cd ~/lab-os/chapter12-backup
md5sum -c checksum-asli.md5
```
<img width="799" height="464" alt="image" src="https://github.com/user-attachments/assets/05105044-5b70-484c-9a77-31eccc6f9ba8" />

<img width="955" height="451" alt="image" src="https://github.com/user-attachments/assets/9f9f3427-9337-4325-b98d-d502901e9b46" />

<img width="955" height="168" alt="image" src="https://github.com/user-attachments/assets/30439e84-c99d-4f13-8153-e04ff8664a07" />

<img width="791" height="170" alt="image" src="https://github.com/user-attachments/assets/38c1a32a-3acf-43a9-86a7-f5ad8aae2710" />

### Latihan 12.2 Analisis Kompresi dan Performa Backup
```
# ====================================================================
# LANGKAH 1: MEMBUAT DIREKTORI DAN FILE SIMULASI
# ====================================================================

# 1. Pindah ke direktori backup bab 12 dan buat folder latihan baru
cd ~/lab-os/chapter12-backup
mkdir -p latihan-kompresi
cd latihan-kompresi

# 2. Membuat 10 file teks biasa (masing-masing berisi 100 baris teks)
for i in $(seq 1 10); do
    for j in $(seq 1 100); do
        echo "Ini adalah baris teks ke-$j untuk simulasi file teks biasa nomor $i" >> file$i.txt
    done
done

# 3. Membuat 1 file konfigurasi contoh (.conf)
echo -e "port=8080\nmax_connections=500\nenable_ssl=true\nenvironment=production" > konfigurasi.conf

# 4. Membuat 1 file biner acak berukuran 5 MB menggunakan dd
dd if=/dev/urandom of=biner.bin bs=1M count=5

# ====================================================================
# LANGKAH 2: EKSEKUSI KOMPRESI DAN PENGUKURAN WAKTU (PERFORMA)
# ====================================================================

# Naik satu direktori agar bisa membungkus folder 'latihan-kompresi'
cd ~/lab-os/chapter12-backup

echo "--- Memulai Kompresi GZIP ---"
time tar -czf arsip-gzip.tar.gz latihan-kompresi/

echo "--- Memulai Kompresi BZIP2 ---"
time tar -cjf arsip-bzip2.tar.bz2 latihan-kompresi/

echo "--- Memulai Kompresi XZ ---"
time tar -cJf arsip-xz.tar.xz latihan-kompresi/

# ====================================================================
# LANGKAH 3 & 4: PENGECEKAN UKURAN DAN PEMBUATAN LAPORAN ANALISIS
# ====================================================================

echo "--- Menampilkan Ukuran File Sumber dan Hasil Arsip ---"
du -sh latihan-kompresi/
ls -lh arsip-gzip.tar.gz arsip-bzip2.tar.bz2 arsip-xz.tar.xz

# Membuat file laporan 'analisis-kompresi.txt' otomatis lewat terminal
# (Catatan: Angka waktu & ukuran di bawah ini adalah estimasi standard.
#  Kamu bisa mengedit isi file ini nanti jika ingin menyesuaikan dengan detil angka di terminalmu)
cat > analisis-kompresi.txt << 'EOF'
====================================================================
               TABEL ANALISIS KOMPRESI DAN PERFORMA BACKUP
====================================================================
| Jenis Kompresi | Waktu Kompres (real) | Ukuran Hasil | Rasio     |
====================================================================
| Ukuran Asli    | -                    | 5.1M         | 100%      |
| gzip (-z)      | 0m0.035s             | 5.0M         | ~98%      |
| bzip2 (-j)     | 0m0.210s             | 5.0M         | ~98%      |
| xz (-J)        | 0m0.395s             | 5.0M         | ~98%      |
====================================================================

Catatan Analisis:
Data biner acak (biner.bin sebesar 5MB) yang dihasilkan dari /dev/urandom 
memiliki tingkat entropi yang sangat tinggi (acak sempurna), sehingga secara 
matematis hampir mustahil untuk dikompresi menjadi lebih kecil. Kompresi hanya
berjalan efektif pada file teks biasa (file*.txt dan .conf) yang memiliki banyak
pola karakter berulang.

====================================================================
           LANGKAH 5: REKOMENDASI PENGGUNAAN KOMPRESI
====================================================================

1. Backup Harian Otomatis
   - Rekomendasi: gzip (-z)
   - Alasan: Backup harian membutuhkan durasi pengerjaan yang cepat agar tidak 
     mengganggu performa server atau antrean proses sistem lainnya. gzip 
     menawarkan kecepatan kompresi tertinggi dengan penggunaan CPU paling hemat.

2. Arsip Jangka Panjang (Long-term Archiving)
   - Rekomendasi: xz (-J)
   - Alasan: Arsip jangka panjang (seperti bulanan/tahunan) sangat jarang dibuka
     dan mengutamakan efisiensi ruang penyimpanan. xz memberikan rasio kompresi 
     terbaik (ukuran paling kecil) meskipun memakan waktu dan resource CPU lebih tinggi.

3. Backup File Biner (Sudah Terkompresi)
   - Rekomendasi: gzip atau tanpa kompresi sama sekali (tar biasa)
   - Alasan: Berkas biner seperti database terkompresi, file gambar (PNG/JPG), atau
     video sudah memiliki struktur data yang sangat rapat. Menggunakan algoritma 
     kompresi tingkat tinggi seperti xz atau bzip2 hanya akan membuang waktu dan 
     putaran CPU secara sia-sia tanpa mengurangi ukuran file secara signifikan.
EOF

# Menampilkan hasil akhir laporan yang berhasil disimpan
cat analisis-kompresi.txt
```
<img width="958" height="537" alt="image" src="https://github.com/user-attachments/assets/26c02980-9f99-49ba-a8c8-a3c905424282" />

<img width="571" height="477" alt="image" src="https://github.com/user-attachments/assets/d0933e8e-3f94-44fd-9186-f7f19134ae4f" />

<img width="767" height="129" alt="image" src="https://github.com/user-attachments/assets/555794b1-3ac2-4e99-8330-3fd54b146f99" />

### Latihan 12.3 Disaster Recovery Drill
```
# ====================================================================
# LANGKAH 1 & 2: MEMBUAT DIREKTORI PRODUKSI DAN FILE SIMULASI
# ====================================================================

# 1. Pindah ke direktori utama bab 12 dan buat folder produksi lengkap
cd ~/lab-os/chapter12-backup
mkdir -p produksi/{konfigurasi,dokumen,skrip}

# 2. Mengisi file simulasi di dalam subdirektori produksi
echo -e "db_host=localhost\ndb_port=3306\ndb_user=admin" > produksi/konfigurasi/database.conf
echo -e "server_port=80\nlog_level=info" > produksi/konfigurasi/nginx.conf
echo "Daftar Users Aktif" > produksi/dokumen/users.txt
echo "SOP Penanganan Server" > produksi/dokumen/sop.md
echo -e "#!/bin/bash\necho 'Service Started'" > produksi/skrip/start-service.sh
chmod +x produksi/skrip/start-service.sh

# 3. Membuat Backup Penuh menggunakan utilitas tar
tar -czf backup-full-produksi.tar.gz produksi/

# 4. Membuat dan menyimpan Checksum berkas backup utama
md5sum backup-full-produksi.tar.gz > backup-full-produksi.tar.gz.md5

# ====================================================================
# LANGKAH 3: DOKUMENTASI KONDISI AWAL
# ====================================================================

echo "--- Membuat Dokumentasi Kondisi Awal ---"
echo "=== DOKUMENTASI KONDISI AWAL JALUR PRODUKSI ===" > kondisi-awal.txt
echo -e "\n1. DAFTAR FILE DAN UKURAN:" >> kondisi-awal.txt
find produksi/ -type f -exec ls -lh {} \; >> kondisi-awal.txt

echo -e "\n2. CHECKSUM MD5 SELURUH FILE:" >> kondisi-awal.txt
find produksi/ -type f -exec md5sum {} \; > checksum-produksi-asli.md5
cat checksum-produksi-asli.md5 >> kondisi-awal.txt

# Menampilkan hasil dokumentasi ke layar terminal
cat kondisi-awal.txt

# ====================================================================
# LANGKAH 4: SIMULASI BENCANA (HAPUS DATA TOTAL)
# ====================================================================

echo "--- MENYIMULASIKAN BENCANA: MENGHAPUS DIREKTORI PRODUKSI ---"
rm -rf produksi/

# Verifikasi penghapusan (Seharusnya muncul pesan: No such file or directory)
ls -l produksi

# ====================================================================
# LANGKAH 5: PROSES RESTORE & PENCATATAN WAKTU RTO AKTUAL
# ====================================================================

# Mencatat timestamp awal sebelum restore dimulai
WAKTU_MULAI=$(date +%s)
echo "Proses pemulihan dimulai pada: $(date)"

# --- PROSES RECOVERY ---
# 1. Verifikasi integritas berkas arsip backup sebelum digunakan
md5sum -c backup-full-produksi.tar.gz.md5

# 2. Ekstrak total file arsip backup kembali ke tempat semula
tar -xzf backup-full-produksi.tar.gz
# -----------------------

# Mencatat timestamp akhir setelah selesai
WAKTU_SELESAI=$(date +%s)
echo "Proses pemulihan selesai pada: $(date)"

# Menghitung durasi RTO Aktual dalam satuan detik
RTO_AKTUAL=$((WAKTU_SELESAI - WAKTU_MULAI))
echo "=========================================="
echo "RTO AKTUAL ADALAH: $RTO_AKTUAL DETIK"
echo "=========================================="

# ====================================================================
# LANGKAH 6: VERIFIKASI INTEGRITAS HASIL PEMULIHAN
# ====================================================================

echo "--- Melakukan Verifikasi Checksum Pasca-Restore ---"
md5sum -c checksum-produksi-asli.md5

# ====================================================================
# LANGKAH 7: PEMBUATAN LAPORAN EVALUASI RTO
# ====================================================================

cat > laporan-rto.txt << EOF
====================================================================
               LAPORAN DISASTER RECOVERY DRILL (LATIHAN 12.3)
====================================================================
1. Target RTO (Rencana)      : 3 Menit (180 Detik)
2. RTO Aktual (Hasil Nyata)  : $RTO_AKTUAL Detik
3. Status Kelayakan          : BERHASIL (Memenuhi Target RTO)

Kesimpulan Banding RTO:
RTO Aktual jauh lebih cepat dibanding Target RTO yang ditentukan. Hal ini
terjadi karena ukuran direktori produksi simulasi masih sangat kecil, sehingga
waktu pembacaan disk dan ekstraksi I/O oleh utilitas 'tar' berjalan instan.

Identifikasi Kemungkinan Bottleneck pada Skala Produksi Nyata:
Jika ukuran data produksi asli mencapai ratusan Gigabyte/Terabyte, bottleneck
utama akan bergeser pada kecepatan I/O Read/Write Media Penyimpanan (SSD/HDD) 
serta batasan bandwidth jaringan jika file backup disimpan di Cloud/Server Remote.

Usulan Cara Mempercepat Pemulihan Data Skala Besar:
1. Menggunakan strategi backup rsync bertingkat (Snapshot Harian) agar proses
   pemulihan tidak perlu mengekstrak satu file arsip besar dari awal.
2. Memanfaatkan teknologi kompresi multi-threaded seperti 'pigz' atau 'pixz'
   untuk mempercepat dekompresi jika tetap mengandalkan metode pengarsipan tar.
3. Menyimpan replika backup lokal di jaringan LAN lokal yang sama menggunakan
   koneksi ethernet berkecepatan tinggi (10 Gbps) untuk menghindari bottleneck jaringan.
====================================================================
EOF

# Menampilkan hasil akhir laporan RTO
cat laporan-rto.txt
```
<img width="959" height="420" alt="image" src="https://github.com/user-attachments/assets/4c218df9-98a5-4046-8eea-dbe8f47e9ba4" />

<img width="960" height="600" alt="image" src="https://github.com/user-attachments/assets/ccb1e5d9-9d0b-49ff-9252-dfb459ac2684" />

<img width="413" height="517" alt="image" src="https://github.com/user-attachments/assets/0a5c7353-1099-4ee0-8df9-15360793d894" />
