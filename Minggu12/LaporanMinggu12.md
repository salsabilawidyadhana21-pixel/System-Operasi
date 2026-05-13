# Laporan Praktikum System Operasi Minggu 12 Manajemen Service

<h4>Nama        : Salsabila Widyadhana<h4>
<h4>NIM         : 254107020200<h4>
<h4>Kelas       : TI-1H<h4>
<h4>Mata Kuliah : System Operasi<h4>

## Praktek 10.1: Amati Layanan Aktif Saat Boot
```
1. systemctl list - units -- type = service -- state = running
```
<img width="487" height="358" alt="image" src="https://github.com/user-attachments/assets/3a06ef20-b001-4496-80bb-e27555e38cc5" />

```
2. systemctl list - unit - files -- type = service | head -30
```
<img width="607" height="301" alt="image" src="https://github.com/user-attachments/assets/d86c1d46-79b1-437d-94a3-90eff48e4ff9" />

```
3. systemd - analyze
systemd - analyze blame | head -15
```
<img width="305" height="188" alt="image" src="https://github.com/user-attachments/assets/9585aed8-711c-41b8-831e-31cf1b6114a4" />

## Praktek 10.2: Kelola Layanan SSH
```
1. systemctl status ssh
systemctl is - active ssh
systemctl is - enabled ssh
```
<img width="305" height="247" alt="image" src="https://github.com/user-attachments/assets/4d5ca632-30d1-489b-8775-ac953db5f6cd" />

```
2. sudo systemctl restart ssh
systemctl status ssh
```
<img width="311" height="227" alt="image" src="https://github.com/user-attachments/assets/b9451f05-b714-4b97-b8e4-66e3befc28da" />

```
3. systemctl list - dependencies ssh
```
<img width="260" height="310" alt="image" src="https://github.com/user-attachments/assets/f0462257-0dda-4605-ac77-99e8cb36190b" />

```
4. systemctl -- failed
```
<img width="296" height="40" alt="image" src="https://github.com/user-attachments/assets/95013a8a-6d11-4763-964a-c18543b1ad56" />

430-1912bfbf0819" />

## Praktek 10.3: Buat Layanan Sederhana dari Skrip Bash
```
cd ~/lab-os/chapter10-services
mkdir -p situs-demo
nano situs-demo/index.html
<!DOCTYPE html>
<html>
<body>
<h1>Halo dari layanan systemd kustom!</h1>
<p>Layanan ini dibuat pada praktek Bab 10.</p>
</body>
</html>
nano ~/lab-os/chapter10-services/jalankan-server.sh
#!/bin/bash
DIREKTORI="$HOME/lab-os/chapter10-services/situs-demo"
PORT=9090
echo "Memulai server di port $PORT..."
exec python3 -m http.server $PORT --directory "$DIREKTORI"
chmod +x ~/lab-os/chapter10-services/jalankan-server.sh
nano ~/lab-os/chapter10-services/demo-web.service
[Unit]
Description=Demo Web Server Praktek Bab 10
After=network.target

[Service]
Type=simple
User=salsabilawidyadhana
Working Directory=/home/salsabilawidyadhana/lab-os/chapter10-services/situs-demo
ExecStart=/usr/bin/python3 -m http.server 9090
Restart=on-failure
RestartSec=3s

[Install]
WantedBy=multi-user.target
sudo cp ~/lab-os/chapter10-services/demo-web.service /etc/systemd/system/demo-web.service
sudo systemctl daemon-reload
sudo systemctl start demo-web
systemctl status demo-web
curl http://localhost:9090
```

<img width="308" height="193" alt="image" src="https://github.com/user-attachments/assets/2ceb96c4-15ea-497a-aceb-d65397a430cf" />

## Praktek 10.4: Filter dan Analisis Log Layanan
```
journalctl -u ssh -- since "1 hour ago" --no - pager
# jika tidak ada log SSH , gunakan layanan lain yang aktif
journalctl -u cron -- since "1 hour ago" --no - pager
```

<img width="269" height="300" alt="image" src="https://github.com/user-attachments/assets/4f89159b-97f7-4f12-8245-bdc85a087165" />

```
journalctl -b -p err --no - pager
# ini menampilkan semua error dan yang lebih serius sejak boot
```

<img width="309" height="78" alt="image" src="https://github.com/user-attachments/assets/6f1a972c-a7f9-48af-9f66-dca111c15b79" />

```
# jalankan di terminal pertama :
journalctl -u ssh -f
# di terminal kedua , coba login SSH ke localhost
# ssh localhost
# lalu lihat apa yang muncul di terminal pertama
```

<img width="308" height="92" alt="image" src="https://github.com/user-attachments/assets/e2ae2b43-1250-4c7b-8540-7b7880854e06" />

```
cd ~/ lab - os / chapter10 - services
# simpan semua log layanan ssh dari hari ini ke berkas
journalctl -u ssh -- since today --no - pager > log - ssh - hari - ini . txt
# hitung jumlah baris log
wc -l log - ssh - hari - ini . txt
# jika ada , cari baris yang mengandung kata " error " atau " failed "
grep -i " error \| failed " log - ssh - hari - ini . txt | head -20
```

<img width="609" height="109" alt="image" src="https://github.com/user-attachments/assets/94c7cdbf-befe-4c46-b179-03d9f51addd6" />

## Praktek 10.5: Konfigurasi SSH Server
```
sudo grep -n "^ Port \|^# Port " / etc / ssh / sshd_config
ss - tlnp | grep ssh
sudo cp / etc / ssh / sshd_config / etc / ssh / sshd_config . backup . lab12
# ubah port dari 22 ke 2222 ( atau port lain yang belum dipakai )
sudo sed -i 's /^# Port 22/ Port 2222/ ' / etc / ssh / sshd_config
# jika baris Port 22 tidak dikomentari :
# sudo sed -i 's/^ Port 22/ Port 2222/ ' /etc/ssh/ sshd_config
# verifikasi perubahan
grep "^ Port " / etc / ssh / sshd_config
# WAJIB : validasi sintaks sebelum restart
sudo sshd -t
echo " Kode keluar sshd -t: $?"
# kode 0 berarti sintaks valid
# restart layanan
sudo systemctl restart ssh
systemctl status ssh
```

<img width="278" height="291" alt="image" src="https://github.com/user-attachments/assets/d289d9dd-8825-42a9-ba04-1a3a276e52ed" />

```
ss - tlnp | grep ssh
# seharusnya menampilkan port 2222 , bukan 22
# simpan hasil ke berkas bukti
ss - tlnp | grep ssh > ~/ lab - os / chapter10 - services / bukti - port - ssh . txt
cat ~/ lab - os / chapter10 - services / bukti - port - ssh . txt
sudo cp / etc / ssh / sshd_config . backup . lab12 / etc / ssh / sshd_config
sudo sshd -t
sudo systemctl restart ssh
ss - tlnp | grep ssh
# harus kembali ke port 22
```

<img width="376" height="105" alt="image" src="https://github.com/user-attachments/assets/e520e644-96cb-4356-92d3-df5cc3511d64" />

##  Latihan

#### Latihan 10.1 Audit Layanan dan Analisis Boot
Lakukan audit menyeluruh terhadap layanan yang berjalan di sistem.
1. Jalankan systemctl list-units –type=service –state=running dan catat semua
layanan aktif. Pilih tiga layanan yang kamu kenal, periksa status masing-masing dengan
systemctl status, dan jelaskan fungsinya.
2. Jalankan systemd-analyze blame dan identifikasi lima layanan dengan waktu inisialisasi
terlama. Tampilkan hasilnya menggunakan pipeline: systemd-analyze blame | head -5.
3. Jalankan systemctl –failed dan dokumentasikan hasilnya. Jika ada layanan yang gagal, cari
tahu penyebabnya dengan journalctl -u nama-layanan -n 30.

```
systemctl list-units --type=service --state=running
```
<img width="308" height="275" alt="image" src="https://github.com/user-attachments/assets/4a821bfd-7cee-4d30-a033-ee4a80e7151c" />

```
systemd-analyze
systemd-analyze blame
```
<img width="206" height="269" alt="image" src="https://github.com/user-attachments/assets/66b3f752-e9c6-42ff-99c6-effcbbf14d35" />

```
systemd-analyze critical-chain
```

<img width="314" height="181" alt="image" src="https://github.com/user-attachments/assets/b6b392cb-d2ce-4233-a196-92bdbd990d7a" />

Kesimpulan: Sistem boot cukup cepat, namun layanan fwupd.service memakan waktu paling lama karena melakukan pengecekan firmware perangkat keras.

#### Latihan 10.2 Layanan Kustom dengan Restart Otomatis
Buat layanan systemd kustom yang mendemonstrasikan fitur restart otomatis.
1. Buat skrip Bash (referensi Bab 7) bernama monitor-disk.sh yang setiap 30 detik menuliskan
penggunaan disk ke berkas log. Gunakan df -h dan date.
2. Buat berkas unit /etc/systemd/system/monitor-disk.service untuk menjalankan skrip
tersebut dengan konfigurasi: Restart=always, RestartSec=5s, dan berjalan sebagai pengguna kamu sendiri.
3. Aktifkan dan jalankan layanan. Verifikasi dengan systemctl status dan pastikan log masuk
ke journal.
4. Simulasikan crash dengan membunuh proses secara paksa (kill -9), tunggu 10 detik, dan
verifikasi bahwa layanan hidup kembali secara otomatis.
5. Bersihkan: nonaktifkan layanan dan hapus berkas unit setelah selesai.

```
mkdir -p ~/lab-os/chapter10-services
nano ~/lab-os/chapter10-services/monitor-disk.sh
#!/bin/bash
LOG_FILE="$HOME/lab-os/chapter10-services/disk-usage.log"

while true; do
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] Memeriksa penggunaan disk..." >> "$LOG_FILE"
    df -h / >> "$LOG_FILE"
    echo "-----------------------------------" >> "$LOG_FILE"
    sleep 30
done
chmod +x ~/lab-os/chapter10-services/monitor-disk.sh
sudo nano /etc/systemd/system/monitor-disk.service
[Unit]
Description=Layanan Pemantau Disk Otomatis
After=network.target

[Service]
Type=simple
User=salsabilawidyadhana
ExecStart=/home/salsabilawidyadhana/lab-os/chapter10-services/monitor-disk.sh
Restart=always
RestartSec=5s

[Install]
WantedBy=multi-user.target
sudo systemctl daemon-reload
sudo systemctl enable monitor-disk
sudo systemctl start monitor-disk

# Verifikasi status
systemctl status monitor-disk

# Verifikasi log di journal
journalctl -u monitor-disk
systemctl show monitor-disk --property=MainPID --value
sudo kill -9 [NOMOR_PID_TADI]
systemctl status monitor-disk
```

<img width="627" height="302" alt="image" src="https://github.com/user-attachments/assets/12c905d7-ec1b-4760-9300-4442a1f59be4" />

```
Pembersihan :
sudo systemctl stop monitor-disk
sudo systemctl disable monitor-disk
sudo rm /etc/systemd/system/monitor-disk.service
sudo systemctl daemon-reload
```

<img width="311" height="74" alt="image" src="https://github.com/user-attachments/assets/d5643cf8-c578-4333-8c6b-135cf8575198" />

#### Latihan 10.3 Investigasi Log dan Keamanan SSH
Analisis log sistem dan tingkatkan keamanan konfigurasi SSH.
1. Gunakan journalctl -b -p err untuk menemukan semua error sejak boot terakhir. Simpan hasilnya ke berkas dan hitung jumlah baris dengan wc -l.
2. Lakukan tiga perubahan keamanan pada /etc/ssh/sshd_config: tambahkan PermitRootLogin no, MaxAuthTries 3, dan LoginGraceTime 30. Ikuti alur aman: backup, edit, validasi sshd -t, reload.
3. Setelah reload, verifikasi tiga hal: layanan masih berjalan (systemctl status ssh), port masih mendengarkan (ss -tlnp | grep ssh), dan konfigurasi baru terbaca (grep -E "PermitRoot|MaxAuth|GraceTime" /etc/ssh/sshd_config).
4. Kembalikan konfigurasi SSH ke kondisi semula menggunakan berkas backup.

```
journalctl -b -p err > error_boot.log
wc -l error_boot.log
```

<img width="304" height="41" alt="image" src="https://github.com/user-attachments/assets/94316383-52f3-46da-a44f-4222c39470dd" />

```
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo nano /etc/ssh/sshd_config
```

<img width="524" height="329" alt="image" src="https://github.com/user-attachments/assets/9765b2fd-e071-4c77-b729-8cf181f2eb51" />

```
sudo sshd -t
sudo systemctl reload ssh
```

<img width="616" height="359" alt="image" src="https://github.com/user-attachments/assets/af50d71a-cdc9-400a-bd4a-d9b010f2877b" />

```
systemctl status ssh
```
<img width="298" height="253" alt="image" src="https://github.com/user-attachments/assets/470c1d46-8050-4f13-805b-f633bc0c7785" />

```
ss -tlnp | grep ssh
grep -E "PermitRoot|MaxAuth|GraceTime" /etc/ssh/sshd_config
```
<img width="483" height="57" alt="image" src="https://github.com/user-attachments/assets/33293bc6-3ab9-4700-98ab-63c73934efb8" />

```
sudo cp /etc/ssh/sshd_config.bak /etc/ssh/sshd_config
sudo systemctl reload ssh
```
<img width="470" height="28" alt="image" src="https://github.com/user-attachments/assets/f2a5c02e-cd3e-46c0-bba8-0c026464b702" />



