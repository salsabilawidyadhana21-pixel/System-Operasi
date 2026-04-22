# Laporan Praktikum System Operasi Minggu 9 Pemrograman Bash

<h4>Nama        : Salsabila Widyadhana<h4>
<h4>NIM         : 254107020200<h4>
<h4>Kelas       : TI-1H<h4>
<h4>Mata Kuliah : System Operasi<h4>

## Praktikum 7.1 Script Pertama: Laporan Sistem

```
mkdir -p ~/ praktikum - os / week09 /{ scripts , logs , data }
cd ~/ praktikum - os / week09 / scripts
nano laporan - sistem . sh
#!/ bin/ bash
# Script : laporan - sistem .sh
echo " ================================ "
echo " LAPORAN SISTEM "
echo " ================================ "
echo " Tanggal : $( date '+%A, %d %B %Y ')"
echo "Jam : $( date '+%H:%M:%S ')"
echo " Hostname : $( hostname )"
echo " User : $( whoami )"
echo "CPU core : $( nproc )"
echo "RAM bebas : $( free -h | awk '/^ Mem/ { print $4 }')"
echo " Disk / : $(df -h / | awk 'NR ==2 { print $5 }')
terpakai "
echo " ================================ "
chmod + x laporan - sistem . sh
./ laporan - sistem . sh
```

### Latihan 9.1

Modifikasi laporan-sistem.sh agar menyimpan output ke file
laporan-YYYY-MM-DD.txt sekaligus menampilkannya di terminal. Petunjuk:
gunakan tee yang sudah dipelajari di bab sebelumnya.

```
#!/bin/bash

FILE="laporan-$(date +%F).txt"

{
echo "================================"
echo " LAPORAN SISTEM"
echo "================================"
echo "Tanggal : $(date '+%A, %d %B %Y')"
echo "Jam : $(date '+%H:%M:%S')"
echo "Hostname : $(hostname)"
echo "User : $(whoami)"
echo "CPU core : $(nproc)"
echo "RAM bebas: $(free -h | awk '/^Mem/ {print $4}')"
echo "Disk / : $(df -h / | awk 'NR==2 {print $5}') terpakai"
echo "================================"
} | tee "$FILE"
```
<img width="301" height="207" alt="image" src="https://github.com/user-attachments/assets/c2a25945-5f3a-461f-9602-39bbb61d383b" />

## Praktikum 7.2 Script Info Sistem dengan Argumen

```
nano ~/ praktikum - os / week09 / scripts / info - sistem . sh
#!/ bin/ bash
# Penggunaan : ./ info - sistem .sh [nama - admin ] [batas -
disk - persen ]
ADMIN = $ {1: -" Tidak dikenal "}
BATAS = $ {2: -80}
TANGGAL = $ ( date '+%F %T')
DISK_PERSEN = $ ( df / | awk 'NR ==2 { print $5}' | tr -d '%
')
echo " Admin : $ADMIN "
echo " Tanggal : $TANGGAL "
echo " Disk / : ${ DISK_PERSEN }% terpakai "
echo " Batas : ${ BATAS }%"
if [ " $DISK_PERSEN " - gt " $BATAS " ]; then
echo " STATUS : PERINGATAN - disk melebihi batas !
"
else
SISA = $ (( BATAS - DISK_PERSEN ) )
echo " STATUS : Normal ( sisa toleransi ${ SISA }%)"
fi
chmod + x ~/ praktikum - os / week09 / scripts / info - sistem . sh
./ info - sistem . sh
./ info - sistem . sh " Dian " 50
./ info - sistem . sh " Dian " 10
```

### Latihan 9.2

Buat script kalkulator.sh yang menerima tiga argumen: <angka1>
<operator> <angka2> dengan operator +, -, *, atau /. Contoh:
./kalkulator.sh 20 + 5 menghasilkan 25. Gunakan case untuk memilih
operasi, dan validasi jika argumen tidak lengkap.

```
#!/bin/bash

if [ $# -ne 3 ]; then
    echo "Penggunaan: $0 <angka1> <operator> <angka2>"
    exit 1
fi

A=$1
OP=$2
B=$3

case $OP in
    +) HASIL=$((A + B)) ;;
    -) HASIL=$((A - B)) ;;
    x|\*) HASIL=$((A * B)) ;;
    /) 
        if [ "$B" -eq 0 ]; then
            echo "Error: tidak bisa bagi 0"
            exit 1
        fi
        HASIL=$((A / B)) 
        ;;
    *)
        echo "Operator tidak valid (+, -, *, /)"
        exit 1
        ;;
esac

echo "Hasil: $HASIL"
```
<img width="401" height="224" alt="image" src="https://github.com/user-attachments/assets/4715076b-b893-4e01-acdf-9fb9691a6b8f" />

## Praktikum 7.3  Script Grading dan Menu Interaktif
```
nano ~/ praktikum - os / week09 / scripts / grading - batch . sh
#!/ bin/ bash
# Script : grading - batch .sh
# Proses daftar nilai mahasiswa
MAHASISWA =(" Andi :92" " Budi :73" " Citra :55" " Deni :80" "
Eka :45")
echo "=== HASIL GRADING ==="
for ENTRI in "${ MAHASISWA [@]}"; do
NAMA = $ ( echo " $ENTRI " | cut -d : - f1 )
NILAI = $ ( echo " $ENTRI " | cut -d : - f2 )
if [ " $NILAI " - ge 85 ]; then
GRADE ="A"
elif [ " $NILAI " - ge 75 ]; then
GRADE ="B"
elif [ " $NILAI " - ge 65 ]; then
GRADE ="C"
elif [ " $NILAI " - ge 55 ]; then
GRADE ="D"
else
GRADE ="E"
fi
printf "% -10s %3d %s\n" " $NAMA " " $NILAI " " $GRADE "
done
echo " ===================== "
chmod + x ~/ praktikum - os / week09 / scripts / grading - batch .
sh
./ grading - batch . sh
nano ~/ praktikum - os / week09 / scripts / menu - sistem . sh
#!/ bin/ bash
# Menu interaktif pemantauan sistem
while true ; do
echo ""
echo " ===== MENU MONITOR ===== "
echo "1) Info disk "
echo "2) Info memori "
echo "3) Proses teratas "
echo "4) Keluar "
echo -n " Pilih [1 -4]: "
read PILIHAN
case $PILIHAN in
1) df -h ;;
2) free -h ;;
3) ps aux -- sort = -% cpu | head -6 ;;
4) echo " Sampai jumpa !"; exit 0 ;;
*) echo " Pilihan tidak valid ." ;;
esac
done
chmod + x ~/ praktikum - os / week09 / scripts / menu - sistem . sh
./ menu - sistem . sh
```
### Latihan 9.3

Tambahkan ke script grading-batch.sh sebuah ringkasan di bagian bawah
yang menampilkan: jumlah mahasiswa per grade (A, B, C, D, E) menggunakan
perulangan for kedua yang mengiterasi array MAHASISWA.

```
#!/bin/bash
MAHASISWA=("Andi:92" "Budi:73" "Citra:55" "Deni:80" "Eka:45") 
countA=0; countB=0; countC=0; countD=0; countE=0
echo "=== HASIL GRADING ==="
for ENTRI in "${MAHASISWA[@]}"; do 
    NAMA=$(echo "$ENTRI" | cut -d: -f1)
    NILAI=$(echo "$ENTRI" | cut -d: -f2) 
    if [ "$NILAI" -ge 85 ]; then GRADE="A"; ((countA++)) 
    elif [ "$NILAI" -ge 75 ]; then GRADE="B"; ((countB++)) 
    elif [ "$NILAI" -ge 65 ]; then GRADE="C"; ((countC++)) 
    elif [ "$NILAI" -ge 55 ]; then GRADE="D"; ((countD++)) 
    else GRADE="E"; ((countE++)) [cite: 246]
    fi
    printf "%-10s %3d %s\n" "$NAMA" "$NILAI" "$GRADE" 
done
```
<img width="640" height="203" alt="image" src="https://github.com/user-attachments/assets/73ed3aea-cb45-42fd-a0af-43d9de85b483" />

## Praktikum 7.4 Library Fungsi Validasi
```
nano ~/ praktikum - os / week09 / scripts / lib - validasi . sh
#!/ bin/ bash
# lib - validasi .sh - Library fungsi validasi
adalah_angka () {
[[ "$1" =~ ^[0 -9]+ $ ]]
}
file_bisa_dibaca () {
[ -f "$1" ] && [ -r "$1" ]
}
error_exit () {
echo " ERROR : $1" >&2
exit 1
}
info () { echo "[ INFO ] $1"; }
sukses () { echo "[OK] $1"; }
nano ~/ praktikum - os / week09 / scripts / pakai - library . sh
#!/ bin/ bash
# Muat library ( seperti import di Java )
source ~/ praktikum - os / week09 / scripts / lib - validasi . sh
ANGKA = $1
FILE = $2
[ -z " $ANGKA " ] || [ -z " $FILE " ] && \
error_exit " Penggunaan : $0 <angka > <path -file >"
if adalah_angka " $ANGKA "; then
sukses " Input '$ANGKA ' adalah angka valid "
else
error_exit "'$ANGKA ' bukan angka "
fi
if file_bisa_dibaca " $FILE "; then
sukses " File '$FILE ' bisa dibaca "
info " Jumlah baris : $(wc -l < " $FILE ")"
else
error_exit " File '$FILE ' tidak ada atau tidak bisa
dibaca "
fi
chmod + x ~/ praktikum - os / week09 / scripts / pakai - library .
sh
./ pakai - library . sh 42 / etc / hostname
./ pakai - library . sh abc / etc / hostname
./ pakai - library . sh 42 / tidak - ada . txt
./ pakai - library . sh
```
### Latihan 9.4

Tambahkan fungsi konfirmasi() ke lib-validasi.sh. Fungsi ini
menampilkan pertanyaan, membaca input Y/N dari user, mengembalikan
0 jika Y dan 1 jika N. Buat script demo yang memanggil fungsi ini sebelum
menghapus sebuah file.

```
konfirmasi () {
    local PILIHAN [cite: 299]
    read -p "$1 (y/n): " PILIHAN
    case $PILIHAN in
        [Yy]) return 0 ;; [cite: 320]
        [Nn]) return 1 ;; [cite: 320]
        *) echo "Masukkan y atau n!"; konfirmasi "$1" ;;
    esac
}
```
<img width="536" height="401" alt="image" src="https://github.com/user-attachments/assets/be837e43-4eaf-46d3-8361-cab5970c4110" />

## Praktikum 7.5 Script Backup dengan Opsi
```
nano ~/ praktikum - os / week09 / scripts / backup - data . sh
#!/ bin/ bash
# Penggunaan : ./ backup - data .sh [ -v] [ -c] [ -l logfile ]
<sumber > <tujuan >
VERBOSE = false
COMPRESS = false
LOG_FILE =""
while getopts "vcl:" OPSI ; do
case $OPSI in
v ) VERBOSE = true ;;
c ) COMPRESS = true ;;
l ) LOG_FILE =" $OPTARG " ;;
*) echo " Penggunaan : $0 [ -v] [ -c] [ -l logfile ]
<sumber > <tujuan >"
exit 1 ;;
esac
done
shift $ (( OPTIND - 1) )
SUMBER = $1
TUJUAN = $2
log () {
local MSG ="[$( date '+%T ')] $1"
echo " $MSG "
[ -n " $LOG_FILE " ] && echo " $MSG " >> " $LOG_FILE "
}
[ -z " $SUMBER " ] || [ -z " $TUJUAN " ] && {
echo " ERROR : sumber dan tujuan wajib diisi "; exit
1; }
[ ! -d " $SUMBER " ] && { log " ERROR : $SUMBER tidak ada "
; exit 2; }
mkdir -p " $TUJUAN "
TANGGAL = $ ( date '+%F -%H%M%S')
[ " $VERBOSE " = true ] && log " Sumber : $SUMBER | Tujuan
: $TUJUAN "
if [ " $COMPRESS " = true ]; then
ARSIP =" $TUJUAN /backup -$( basename " $SUMBER ")-
$TANGGAL .tar.gz"
tar - czf " $ARSIP " -C "$( dirname " $SUMBER ")" "$(
basename " $SUMBER ")"
log " Arsip : $ARSIP ($(du -sh " $ARSIP " | cut -f1))"
else
cp -r " $SUMBER " " $TUJUAN /backup -$( basename "
$SUMBER ")-$TANGGAL "
log " Backup selesai ."
fi
chmod + x ~/ praktikum - os / week09 / scripts / backup - data . sh
cd ~/ praktikum - os / week09 / scripts
# Tanpa opsi
./ backup - data . sh ~/ praktikum - os / week09 / data ~/
praktikum - os / week09 / logs
# Dengan verbose dan kompresi + log ke file
./ backup - data . sh -v -c -l ../ logs / backup . log \
~/ praktikum - os / week09 / data ~/ praktikum - os / week09 /
logs
cat ../ logs / backup . log
```

## Praktikum 7.6 Debugging Script
```
nano ~/ praktikum - os / week09 / scripts / debug - latihan . sh
#!/ bin/ bash
# Script : debug - latihan .sh
# Penggunaan : ./ debug - latihan .sh <direktori > <batas -MB
>
DIREKTORI = $1
BATAS = $2
if [ $# -ne 2 ]; then
echo " Penggunaan : $0 <direktori > <batas -MB >"
exit 1
fi
UKURAN = $ ( du - sm " $DIREKTORI " | cut - f1 )
echo " Direktori : $DIREKTORI "
echo " Ukuran : ${ UKURAN } MB"
echo " Batas : ${ BATAS } MB"
if [ " $UKURAN " - gt " $BATAS " ]; then
echo " PERINGATAN : Ukuran melebihi batas !"
echo " Kelebihan : $(( UKURAN - BATAS )) MB"
else
echo " Status : Normal ( sisa : $(( BATAS - UKURAN )) MB
)"
fi
chmod + x ~/ praktikum - os / week09 / scripts / debug - latihan .
sh
bash -n debug - latihan . sh && echo " Sintaks OK"
bash -x debug - latihan . sh / etc 10
./ debug - latihan . sh / var 50
./ debug - latihan . sh
```

### Latihan 9.5

Script debug-latihan.sh tidak menangani direktori yang tidak ada. Perbaiki
dengan menambahkan:
• set -e di baris kedua
• Pengecekan -d "$DIREKTORI" sebelum memanggil du
• Pesan error yang informatif jika direktori tidak ditemukan
Uji dengan direktori yang tidak ada.
```
#!/bin/bash
set -e 
DIREKTORI=$1
BATAS=$2
if [ $# -ne 2 ]; then 
    echo "Penggunaan: $0 <direktori> <batas-MB>"
    exit 1
fi
if [ ! -d "$DIREKTORI" ]; then 
    echo "ERROR: Direktori '$DIREKTORI' tidak ditemukan!" >&2 
    exit 1
fi
UKURAN=$(du -sm "$DIREKTORI" | cut -f1) 
```
<img width="401" height="280" alt="image" src="https://github.com/user-attachments/assets/de82dfd1-b815-4b72-8503-e1194c4ab4fe" />

## Tugas 1 Script Absensi Kelas
```
#!/bin/bash
# Penggunaan: ./absensi.sh <nama> <status> atau ./absensi.sh -r
FILE_ABSEN="../logs/absensi-$(date +%Y-%m-%d).txt" 
show_recap() { 
    if [ ! -f "$FILE_ABSEN" ]; then echo "Belum ada data."; exit 0; fi
    echo "--- REKAPITULASI ABSENSI ---"
    for status in hadir izin alpha; do
        count=$(grep -c -i "$status" "$FILE_ABSEN")
        echo "$status: $count"
    done
    exit 0
}
while getopts "rh" OPSI; do 
    case $OPSI in
        r) show_recap ;;
        h) echo "Gunakan: $0 <nama> <status(hadir/izin/alpha)>"; exit 0 ;;
    esac
done
shift $((OPTIND - 1)) 
# Validasi input
if [ $# -lt 2 ]; then
    echo "Error: Nama dan status wajib diisi."
    exit 1
fi
# Simpan entri 
echo "[$(date +%H:%M)] $1 - $2" >> "$FILE_ABSEN"
echo "Data berhasil dicatat."
```
<img width="416" height="332" alt="image" src="https://github.com/user-attachments/assets/6fdd66a0-133c-4ef4-af51-220a2b1ea2cf" />

## Tugas 2 Script Health Check Sistem
```
#!/bin/bash
set -euo pipefail 
# Konfigurasi 
BATAS_DISK=80 
LOG_FILE="../logs/healthcheck-$(date +%Y-%m-%d).log" 
cleanup() { 
    echo "[$(date +%T)] Pemeriksaan selesai."
}
trap cleanup EXIT 
while getopts "t:" OPSI; do 
    case $OPSI in
        t) BATAS_DISK=$OPTARG ;;
    esac
done
do_check() {
    echo "=== SYSTEM HEALTH CHECK ==="
    echo "Waktu    : $(date '+%F %T')" 
    echo "Hostname : $(hostname)" 
    echo "Uptime   : $(uptime -p)" 
    echo "--- Penggunaan Disk ---"
    df -h | grep '^/dev/' | while read -r line; do 
        usage=$(echo "$line" | awk '{print $5}' | tr -d '%')
        p_mount=$(echo "$line" | awk '{print $6}')
        echo "$p_mount: $usage% terpakai"
        if [ "$usage" -gt "$BATAS_DISK" ]; then 
            echo ">> PERINGATAN: Penggunaan disk pada $p_mount melebihi batas!"
        fi
    done
}
# Jalankan dan simpan hasil 
do_check | tee -a "$LOG_FILE"
```
<img width="289" height="404" alt="image" src="https://github.com/user-attachments/assets/de71ad94-7fad-462c-a66a-fb48ad095b17" />
