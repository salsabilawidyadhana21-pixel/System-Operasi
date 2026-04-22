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
