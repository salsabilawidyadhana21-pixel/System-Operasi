# Laporan Praktikum System Operasi Minggu 11 Manajemen File & User/Group

<h4>Nama        : Salsabila Widyadhana<h4>
<h4>NIM         : 254107020200<h4>
<h4>Kelas       : TI-1H<h4>
<h4>Mata Kuliah : System Operasi<h4>

## Praktikum 9.1 — Permissions
```
mkdir ~/ lab - permissions && cd ~/ lab - permissions
echo " data rahasia " > secret . txt
echo '#!/ bin/ bash ' > myscript . sh
echo 'echo Hello ' >> myscript . sh
ls - la
chmod 600 secret . txt
ls -l secret . txt
chmod 755 myscript . sh
ls -l myscript . sh
./ myscript . sh
mkdir shared - dir
chmod g + s shared - dir
ls - ld shared - dir
umask
umask 027
touch testfile -027
ls -l testfile -027
```
<img width="373" height="232" alt="image" src="https://github.com/user-attachments/assets/6e660205-8735-41bd-9869-4b64360d340e" />

### Analisis
1. Mengapa secret.txt tidak dapat dibaca oleh group dan others setelah chmod 600?
   Jawab:
   Hal ini terjadi karena notasi oktal 600 memberikan instruksi spesifik kepada sistem untuk mengatur hak akses sebagai berikut:
   - Angka pertama (6): Memberikan izin baca (read) dan tulis (write) kepada Owner (pemilik file).  
   - Angka kedua (0): Menghapus seluruh izin akses bagi Group.  
   - Angka ketiga (0): Menghapus seluruh izin akses bagi Others (pengguna lain).
   Dengan demikian, sistem secara ketat membatasi file tersebut agar hanya bisa diakses oleh pemiliknya saja.
  
2. Apa perbedaan arti 600 dan 755 terhadap file yang diuji?
   Jawab:
   Perbedaan utamanya terletak pada cakupan hak akses dan siapa saja yang boleh mengeksekusi file tersebut:
   - Notasi 600 (File Privat): Digunakan untuk file rahasia atau privat. Owner memiliki akses baca dan tulis (rw-), sementara pihak lain tidak memiliki akses sama sekali (---).
   - Notasi 755 (Program/Direktori Publik): Digunakan agar file bisa dijalankan sebagai program. Owner memiliki hak penuh (baca, tulis, eksekusi/rwx), sedangkan Group dan Others hanya memiliki hak baca dan eksekusi (r-x)

3. Setelah umask 027, permission apa yang dihasilkan untuk file baru, dan mengapa bukan 777?
   Jawab:
   - Proses Kalkulasi: File reguler secara default berangkat dari nilai dasar 666. Nilai ini kemudian dikurangi (dimasker) oleh nilai umask yang aktif. Dalam perhitungan sederhana: $666 - 027 = 640$ (untuk file).
   - Mengapa bukan 777? * Sistem Linux memiliki kebijakan keamanan di mana umask tidak pernah menambahkan izin eksekusi (execute) secara otomatis pada file reguler demi alasan keamanan.
     - Nilai 777 hanya digunakan sebagai titik dasar perhitungan untuk Direktori, bukan file reguler.
     - Jika sebuah file harus bisa dieksekusi, pengguna wajib memberikan izin tersebut secara eksplisit menggunakan perintah chmod. 

### Tantangan
Ubah owner atau group salah satu file uji ke akun atau group lain yang tersedia di sistem, kemudian jelaskan
perubahan output ls -l sebelum dan sesudahnya.

<img width="341" height="41" alt="image" src="https://github.com/user-attachments/assets/822a9415-fe03-4364-81c1-147645401678" />

##### Penjelasan
Meskipun string permission di depan tetap menunjukkan -rw-rw-r--, dampak nyata dari perubahan kolom grup tersebut adalah:

- Dampak pada Grup Lama: Semua pengguna yang sebelumnya berada di dalam grup salsabilawidyadhana (selain pemilik file) kini kehilangan hak akses grup. Mereka sekarang dikategorikan sebagai Others (hanya bisa membaca/r--).

- Dampak pada Grup Baru: Semua pengguna yang terdaftar sebagai anggota dari grup sudo di sistem kamu sekarang resmi mendapatkan hak akses grup tersebut, yaitu rw- (baca dan tulis isi file).

## Praktikum 9.2 — ACL
```
mkdir ~/ lab - acl && cd ~/ lab - acl
echo " Data penting " > confidential . txt
chmod 640 confidential . txt
ls -l confidential . txt
getfacl confidential . txt
setfacl -m u : userA : r confidential . txt
ls -l confidential . txt
getfacl confidential . txt
mkdir shared
setfacl -d -m u : userA : rwx shared
setfacl -d -m u : userB :r - x shared
getfacl shared
touch shared / inherited . txt
getfacl shared / inherited . txt
```
<img width="641" height="521" alt="image" src="https://github.com/user-attachments/assets/c983f88b-80aa-476e-8175-e100577e41a1" />

<img width="552" height="122" alt="image" src="https://github.com/user-attachments/assets/1ec2efc4-a50f-45f9-8a07-57e1b24d8cff" />

### Analisis
1. Mengapa getfacl confidential.txt awalnya tidak menampilkan user tertentu?
   Jawab:
   Pada kondisi awal, file confidential.txt baru dibuat dan hanya diatur menggunakan permission standar Unix klasik (chmod 640).
   - Di Linux, jika suatu file belum pernah diberi aturan Access Control List (ACL) tambahan, perintah getfacl secara default hanya akan menampilkan 3 entri dasar yang sepadan dengan permission tradisional, yaitu kepemilikan untuk owner, group, dan others.
   - Itulah mengapa tidak ada nama pengguna atau user spesifik (named user) yang muncul sebelum kamu menjalankan perintah setfacl.
2. Setelah setfacl -m u:userA:r confidential.txt, apa perbedaan output ls -l dan getfacl?
   Jawab:
   Setelah perintah tersebut berhasil dieksekusi, kamu akan melihat perbedaan indikator yang sangat jelas pada kedua output:
   - Perubahan pada ls -l: Di akhir string permission (sebelah kanan izin akses), sekarang akan muncul tanda plus (+), contohnya menjadi -rw-r-----+. Tanda + ini adalah indikator bahwa file tersebut memiliki aturan ACL tambahan yang aktif.
   - Perubahan pada getfacl: Outputnya menjadi lebih detail dan memunculkan baris baru khusus berupa user:userA:r--. Ini menunjukkan secara eksplisit bahwa userA sekarang memiliki hak akses membaca (read) secara personal pada file tersebut tanpa mengubah owner asli dari file.
3. Mengapa file inherited.txt mewarisi ACL dari direktori shared?
   Jawab:
   - Sesuai dengan teori sistem Linux, Default ACL adalah aturan khusus yang dipasang pada tingkat direktori dengan fungsi utama untuk diwariskan secara otomatis kepada setiap elemen baru (baik file reguler maupun sub-direktori) yang dibuat di dalam folder tersebut.
   - Karena file inherited.txt dibuat di dalam folder shared (touch shared/inherited.txt), maka sistem secara otomatis menurunkan (inherit) aturan akses milik userA dan userB ke file tersebut sejak detik pertama file diciptakan.

### Tantangan
Tambahkan satu ACL lagi agar group readonly-group hanya dapat membaca confidential.txt. Setelah
itu, hapus ACL untuk userA dan verifikasi hasil akhirnya dengan getfacl.
<img width="746" height="192" alt="image" src="https://github.com/user-attachments/assets/df8c7f48-2e22-4b8e-a40e-6978e12612f7" />

##### Penjelasan
- group:readonly-group:r--: Kelompok readonly-group sekarang sudah resmi terdaftar dan sukses diberikan hak akses hanya untuk membaca (read-only).

- UserA Hilang: Baris user:userA:r-- yang sebelumnya ada, sekarang sudah bersih terhapus dari daftar ACL sesuai instruksi tantangan.

- mask::r--: Nilai mask otomatis menyesuaikan batas efektivitas izin akses maksimal menjadi read-only.

## Praktikum 9.3A — Membuat dan Mengelola User
```
# buat dua user
sudo useradd -m -s / bin / bash userA
sudo useradd -m -s / bin / bash userB
sudo passwd userA
sudo passwd userB
# verifikasi
id userA
getent passwd userA
# modifikasi shell userA
sudo usermod -s / bin / zsh userA
getent passwd userA
# lock dan unlock userB
sudo usermod -L userB
sudo passwd -S userB
sudo usermod -U userB
sudo passwd -S userB
```
<img width="608" height="523" alt="Cuplikan layar 2026-05-06 085428" src="https://github.com/user-attachments/assets/177cc5fa-f796-4da6-aeb9-9ee75ec99e8c" />
<img width="648" height="469" alt="Cuplikan layar 2026-05-06 085502" src="https://github.com/user-attachments/assets/2cec76cf-3eaf-47e8-b361-44db8a13e157" />
<img width="659" height="517" alt="Cuplikan layar 2026-05-06 085524" src="https://github.com/user-attachments/assets/7c5267b3-09e7-414a-a80e-43d3f21b2b44" />

### Pertanyaan:
1. Apa perbedaan output id userA sebelum dan sesudah menambah group?
   Jawab:
   - Sebelum Menambah Group: Output id userA menunjukkan bahwa userA hanya menjadi anggota dari primary group-nya sendiri, yang ditandai dengan tulisan groups=1001(userA).
   - Sesudah Menambah Group: Setelah menjalankan perintah sudo usermod -aG labgroup,readonly-group userA, output id userA berubah secara signifikan menjadi groups=1001(userA),1003(labgroup),1004(readonly-group).
   - Kesimpulan: Perbedaannya terletak pada cakupan keanggotaannya; sebelum ditambahkan, ia hanya memiliki satu grup utama (primary group), sedangkan sesudah perintah dieksekusi, sistem menampilkan tambahan seluruh grup sekunder (supplementary groups) yang berhasil diikuti oleh userA.
2. Bagaimana status passwd -S userB berubah saat akun di-lock?
   Jawab:
   - Sebelum Di-lock (Akun Aktif): Output perintah passwd -S userB akan menampilkan indikator huruf P setelah nama user, yang berarti Password usable (kata sandi aktif dan dapat digunakan).
   - Saat Akun Di-lock: Setelah kamu mengeksekusi perintah sudo usermod -L userB, status pada output passwd -S userB akan otomatis berubah menampilkan kode huruf L. Huruf L ini merupakan indikator resmi dari sistem Linux yang menandakan Password locked (akses autentikasi akun tersebut sedang dikunci/dinonaktifkan).
   - Setelah Di-unlock Kembali: Ketika kunci dibuka kembali dengan opsi -U, statusnya akan pulih kembali memunculkan huruf P.

## Praktikum 9.3B — Group Management
```
# buat dua group
sudo groupadd labgroup
sudo groupadd readonly - group
# tambahkan userA ke kedua group
sudo usermod - aG labgroup , readonly - group userA
# tambahkan userB hanya ke readonly - group
sudo usermod - aG readonly - group userB
# verifikasi
id userA
id userB
getent group labgroup
getent group readonly - group
```
<img width="643" height="141" alt="image" src="https://github.com/user-attachments/assets/06830b41-2862-4e6c-bfea-a6e1aca8bfd1" />

### Pertanyaan:
1. Apa yang ditampilkan id userA vs groups userA?
   Jawab:
   - groups userA: Perintah ini hanya menampilkan daftar nama-nama grup tempat userA bergabung dalam bentuk teks biasa, tanpa detail tambahan. Contoh outputnya: userA : userA labgroup readonly-group.

   - id userA: Perintah ini jauh lebih detail dan akurat karena menampilkan seluruh identitas numerik (ID) yang dikenali oleh sistem Linux. Outputnya mencakup:

     - uid: User ID unik milik userA.

     - gid: Group ID dari primary group (grup utama) milik userA.

     - groups: Daftar seluruh GID beserta nama dari supplementary groups (grup tambahan/sekunder) yang diikuti oleh userA.
2. Mengapa -a pada usermod -aG penting?
   Jawab:
   - Opsi -a merupakan singkatan dari append (menambahkan).

   - Penggunaan -a bersamaan dengan -G (-aG) sangat penting agar grup baru yang kita masukkan akan ditambahkan ke dalam daftar keanggotaan user tanpa menghapus grup-grup sekunder yang sudah diikuti oleh user tersebut sebelumnya.

   - Peringatan jika tanpa -a: Jika kamu hanya mengetik usermod -G tanpa menyertakan -a, sistem Linux secara ekstrem akan menghapus semua supplementary group lama milik user tersebut dan menggantinya mentah-mentah hanya dengan grup yang baru kamu ketik. Hal ini sangat berbahaya jika user tersebut awalnya memiliki hak akses penting (seperti grup sudo) karena akses lamanya akan langsung hilang seketika.

## Praktikum 9.3C — Password Aging Policy
```
# set aging policy untuk userA
sudo chage -M 60 -W 7 -m 1 userA
sudo chage -l userA
# paksa userA ganti password saat login pertama
sudo chage -d 0 userA
# kunci password userB
sudo passwd -l userB
sudo passwd -S userB
# unlock kembali
sudo passwd -u userB
sudo passwd -S userB
```
<img width="534" height="256" alt="image" src="https://github.com/user-attachments/assets/c10aa3c4-75d8-4e96-9cf7-f839e7572c50" />

### Pertanyaan:
1. Apa arti nilai yang ditampilkan chage -l userA?
   Jawab:
   - Last password change: Menunjukkan tanggal terakhir kali userA mengubah password-nya. Jika tertulis password must be changed, berarti sistem mendeteksi bahwa password tersebut telah dipaksa kedaluwarsa dan harus diganti pada login berikutnya.

   - Password expires: Tanggal di mana password akan kedaluwarsa secara otomatis.

   - Password inactive: Jumlah hari masa tenggang setelah password kedaluwarsa sebelum akun tersebut dinonaktifkan atau dikunci sepenuhnya jika pengguna tidak kunjung mengganti password.

   - Account expires: Tanggal kedaluwarsa akun secara keseluruhan. Jika diatur never, berarti akun tersebut aktif selamanya dan tidak akan hangus.

   - Minimum number of days between password change: Batas waktu minimal (dalam hari) yang harus dilewati sebelum user diizinkan untuk mengganti password-nya lagi sejak pergantian terakhir.

   - Maximum number of days between password change: Masa aktif maksimal password (dalam hari). Pengguna wajib memperbarui password-nya sebelum melewati batas waktu ini.

   - Number of days of warning before password expires: Jumlah hari sebelum password kedaluwarsa di mana sistem akan mulai memberikan pesan peringatan (warning) saat user melakukan login untuk segera mengganti kata sandinya.
2. Bagaimana cara membuktikan userB terkunci dari output passwd -S?
   Jawab:
   - Saat Akun Terkunci (Locked): Output akan memunculkan huruf L (contoh: userB L ...), yang merupakan singkatan resmi dari Locked. Ini membuktikan bahwa mekanisme autentikasi kata sandi milik userB sedang dinonaktifkan oleh sistem.

   - Saat Akun Aktif (Usable): Jika statusnya normal atau sudah dibuka kembali (unlocked), indikator tersebut akan berubah menjadi huruf P (artinya Password usable / kata sandi dapat digunakan).
3. Kapan sebaiknya menggunakan chage -d 0 vs passwd -e?
   Jawab:
  - chage -d 0: Sebaiknya digunakan oleh Administrator saat baru saja membuatkan akun massal/baru untuk pengguna atau saat melakukan reset berkala.
  - passwd -e (Expire): Lebih tepat digunakan untuk aspek manajemen keamanan darurat/insidentil.

### Tantangan
Buat user bernama intern yang:
• memiliki shell /bin/bash;
• menjadi anggota labgroup;
• dipaksa ganti password pada login pertama;
• password expired setelah 45 hari dengan warning 7 hari sebelumnya.
<img width="655" height="255" alt="image" src="https://github.com/user-attachments/assets/2f2a64c2-8f16-481a-a8e6-6ff35d55bcff" />

## Praktikum 9.4 — Konfigurasi sudo
```
sudo visudo -f / etc / sudoers . d / lab - userA
userA ALL =( root ) NOPASSWD : / usr / bin / apt update , / usr / bin / apt
upgrade
userA ALL =( root ) / bin / systemctl status *
sudo -l -U userA
sudo grep " userA " / var / log / auth . log | tail -10
```
<img width="455" height="179" alt="image" src="https://github.com/user-attachments/assets/cecea9fc-3bd6-4150-9252-63a469fae2ac" />

### Analisis
1. Mengapa aturan disimpan di /etc/sudoers.d//, bukan langsung di /etc/sudoers?
   Jawab:
   - Modularitas dan Kerapian: Menyimpan konfigurasi di /etc/sudoers.d/ memungkinkan Administrator untuk memisahkan aturan privilege berdasarkan user, grup, atau proyek tertentu ke dalam file-file terpisah (modular). Hal ini menjaga file utama /etc/sudoers tetap bersih dan bawaan pabrik (default).

   - Keamanan saat Upgrade Sistem: Jika ada pembaruan (upgrade) paket sistem atau distro Linux, file utama /etc/sudoers sering kali otomatis ditimpa atau diperbarui oleh manajer paket (apt). Dengan menaruh konfigurasi kustom di dalam folder /etc/sudoers.d/, aturan yang sudah kita buat tidak akan terhapus atau rusak saat sistem diperbarui.

   - Kemudahan Manajemen Akun: Sangat mudah untuk menambah atau mencabut hak akses seorang user secara spesifik. Jika hak akses ingin dihapus, admin cukup menghapus file milik user tersebut di /etc/sudoers.d/ tanpa risiko merusak baris konfigurasi milik user lain.
2. Mana perintah yang bisa dijalankan tanpa password, dan mana yang masih perlu autentikasi?
   Jawab:
   - Perintah yang memerlukan autentikasi (perlu password)
   - Perintah yang bisa dijalankan TANPA password
3. Informasi apa saja yang dicatat di log sudo?
   Jawab:
  - Waktu Kejadian (Timestamp): Tanggal, jam, menit, dan detik persis saat perintah tersebut dieksekusi.

  - Nama Host (Hostname): Identitas mesin/komputer tempat perintah dijalankan.

  - User Pengeksekusi: Nama akun user asli yang mengetikkan perintah sudo (misalnya: userA).

  - Direktori Kerja (PWD): Lokasi folder tempat user berada saat menjalankan perintah tersebut.

  - Identitas Target (RunAs): Hak akses target yang digunakan untuk menjalankan perintah (secara default adalah root).

  - Perintah yang Dieksekusi (COMMAND): Path lengkap beserta argumen/perintah spesifik yang dijalankan (misalnya: COMMAND=/usr/bin/apt update).

  - Status Keberhasilan: Keterangan apakah perintah tersebut sukses dijalankan, atau gagal karena salah password (authentication failure), atau diblokir karena user tidak terdaftar di sudoers (user not in sudoers).
