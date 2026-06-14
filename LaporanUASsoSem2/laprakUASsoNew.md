# Laporan Remastering Distribusi Linux Berbasis Ubuntu

**Nama**: Salsabila Widyadhana

**NIM**: 254107020200

**Mata Kuliah**: Sistem Operasi

---

## 1. Pendahuluan

Pada proyek ini, dilakukan proses _remastering_ terhadap distribusi Linux berbasis Ubuntu menggunakan tool **Cubic (Custom Ubuntu ISO Creator)**. Proses remastering meliputi penambahan aplikasi kustom, perubahan tampilan antarmuka, serta pembuatan file ISO baru hasil modifikasi.

## 2. Persiapan Sistem Dasar

- **ISO Ubuntu yang digunakan**: Ubuntu 24.04.4 LTS
- **Tool remastering**: Cubic versi [versi]
- **Spesifikasi sistem host**:
  - OS: Windows 10
  - RAM: 12
  - Storage: 512GB
- **Spesifikasi sistem vm**:
  - OS: Ubuntu 24.04.4 LTS
  - RAM: 6 GB
  - Storage: 80 GB

## 3. Kustomisasi dan Instalasi Aplikasi

Berikut aplikasi yang ditambahkan ke dalam sistem hasil remaster:
1. VLC Media Player
2. GIMP
3. Apache2 + PHP
4. Visual Studio Code
5. Aplikasi Kustom (Bash Script)

### 3.1 Aplikasi Kustom Buatan Sendiri (Bash Script)

Aplikasi kustom berupa skrip Bash yang menampilkan informasi dasar perangkat keras komputer, meliputi:

- Informasi CPU
- Kapasitas dan penggunaan memori (RAM)
- Kapasitas ruang penyimpanan (Storage)

**Source code (`sysinfo.sh`):**

```
#!/bin/bash

# ============================================================
#  SISTEM INFO - Informasi Dasar Perangkat Keras
#  Dibuat untuk tugas Remastering Ubuntu
# ============================================================

# --- Warna ---
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
CYAN='\033[0;36m'
BLUE='\033[0;34m'
BOLD='\033[1m'
RESET='\033[0m'

# --- Fungsi garis pemisah ---
line() {
    echo -e "${CYAN}━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━${RESET}"
}

# --- Header ---
clear
echo ""
line
echo -e "  ${BOLD}${BLUE}       🖥️  INFORMASI PERANGKAT KERAS KOMPUTER${RESET}"
line
echo ""

# ============================================================
# 1. INFORMASI PROSESOR (CPU)
# ============================================================
echo -e "  ${BOLD}${YELLOW}[ CPU - Informasi Prosesor ]${RESET}"
line

CPU_MODEL=$(grep -m1 "model name" /proc/cpuinfo | awk -F': ' '{print $2}')
CPU_CORES=$(nproc --all)
CPU_THREADS=$(grep -c "processor" /proc/cpuinfo)
CPU_SPEED=$(grep -m1 "cpu MHz" /proc/cpuinfo | awk -F': ' '{printf "%.0f MHz", $2}')
CPU_ARCH=$(uname -m)

echo -e "  ${GREEN}Model     :${RESET} $CPU_MODEL"
echo -e "  ${GREEN}Arsitektur:${RESET} $CPU_ARCH"
echo -e "  ${GREEN}Core      :${RESET} $CPU_CORES Core(s)"
echo -e "  ${GREEN}Thread    :${RESET} $CPU_THREADS Thread(s)"
echo -e "  ${GREEN}Kecepatan :${RESET} $CPU_SPEED"
echo ""

# ============================================================
# 2. KAPASITAS DAN PENGGUNAAN MEMORI (RAM)
# ============================================================
echo -e "  ${BOLD}${YELLOW}[ RAM - Memori ]${RESET}"
line

RAM_TOTAL=$(free -h | awk '/^Mem:/ {print $2}')
RAM_USED=$(free -h | awk '/^Mem:/ {print $3}')
RAM_FREE=$(free -h | awk '/^Mem:/ {print $4}')
RAM_PERCENT=$(free | awk '/^Mem:/ {printf "%.1f%%", $3/$2*100}')

SWAP_TOTAL=$(free -h | awk '/^Swap:/ {print $2}')
SWAP_USED=$(free -h | awk '/^Swap:/ {print $3}')

echo -e "  ${GREEN}Total RAM :${RESET} $RAM_TOTAL"
echo -e "  ${GREEN}Digunakan :${RESET} $RAM_USED (${RAM_PERCENT})"
echo -e "  ${GREEN}Tersedia  :${RESET} $RAM_FREE"
echo -e "  ${GREEN}Swap Total:${RESET} $SWAP_TOTAL"
echo -e "  ${GREEN}Swap Used :${RESET} $SWAP_USED"

# Bar penggunaan RAM
RAM_USED_INT=$(free | awk '/^Mem:/ {printf "%d", $3/$2*100}')
BAR_FILLED=$((RAM_USED_INT / 5))
BAR_EMPTY=$((20 - BAR_FILLED))
BAR="${RED}"
for i in $(seq 1 $BAR_FILLED); do BAR="${BAR}█"; done
BAR="${BAR}${RESET}"
for i in $(seq 1 $BAR_EMPTY); do BAR="${BAR}░"; done
echo -e "  ${GREEN}Penggunaan:${RESET} [${BAR}] ${RAM_USED_INT}%"
echo ""

# ============================================================
# 3. KAPASITAS RUANG PENYIMPANAN (STORAGE)
# ============================================================
echo -e "  ${BOLD}${YELLOW}[ Storage - Ruang Penyimpanan ]${RESET}"
line

echo -e "  ${GREEN}Filesystem        Total   Digunakan  Tersedia  Penggunaan  Mount${RESET}"
df -h --output=source,size,used,avail,pcent,target | grep "^/dev" | while read line; do
    echo -e "  $line"
done

echo ""
line
echo -e "  ${BOLD}${BLUE}  Script selesai dijalankan — $(date '+%d %B %Y, %H:%M:%S')${RESET}"
line
echo ""
```

**Cara penggunaan:**

1. Simpan skrip ke `/usr/local/bin/sysinfo.sh`
2. Berikan izin eksekusi: `chmod +x /usr/local/bin/sysinfo.sh`
3. Jalankan dengan perintah: `sysinfo.sh`
4. Hasil Penggunaa:
![Tampilan hasil script](img/hasilscript.png)

## 4. Kustomisasi Tampilan (Antarmuka)

Perubahan visual yang dilakukan pada Desktop Environment bawaan:
* Sebelum:
![Tampilan desktop sebelum kustomisasi](img/mainscreenUbuntu.png)

* Sesudah:
![Tampilan desktop setelah kustomisasi](img/mainscreenRemaster.png)

## 5. Pembuatan File ISO# Laporan Remastering Distribusi Linux Berbasis Ubuntu

Langkah-langkah:
1. Masuk ke dalam Ubuntu Linux
![Tampilan desktop Ubuntu](img/mainscreenUbuntu.png)

2. Buka Cubic
![Tampilan Cubic](img/mainCubic.png)

3. Masukkan iso yang ingin diremaster dan ubah nama hasil iso remaster (jika mau)
![Tampilan Cubic](img/memilihiso.png)

4. Tunggu meng-extract iso
![Tampilan Cubic](img/extracting.png)

5. Install semua aplikasi dan customisasi yang diinginkan
![Tampilan Cubic](img/instalasi.png)

6. Tekan next dan tunggu untuk build iso baru
![Tampilan Cubic](img/buildiso.png)

7. Selesai!!
![Tampilan Cubic](img/hasilakhir.png)
