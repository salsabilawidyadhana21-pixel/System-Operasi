# Laporan Praktikum System Operasi Jobsheet 7 Bash Shell dan Shell Basic

<h4>Nama        : Salsabila Widyadhana<h4>
<h4>NIM         : 254107020200<h4>
<h4>Kelas       : TI-1H<h4>
<h4>Mata Kuliah : System Operasi<h4>


## Tugas Praktikum

### Tugas Praktikum 1 — Toolkit Bash Administrator Pribadi
<img width="400" height="250" alt="image" src="https://github.com/user-attachments/assets/142265c8-b4ea-40dc-a0eb-c5c66f18bdbf" />

##### Langkah-langkah Eksekusi:

1. Konfigurasi .bashrc
```
# --- Toolkit Salsabila Widyadhana ---

# 1. Menambahkan direktori bin pribadi ke PATH
export PATH="$HOME/praktikum-os/week07-bash/bin:$PATH"

# 2. Alias produktivitas
alias ll='ls -lah --color=auto'
alias hist10='history | tail -n 10'

# 3. Fungsi shell untuk backup cepat
quick_backup() {
     if [ $# -ne 1 ]; then
     echo "Usage: quick_backup <file>"
     return 1
     fi
     cp -- "$1" "$1.$(date +%F).bak"
     echo "Backup $1 selesai."
}
# --- End Toolkit ---
```

2. Script Ringkasan Sistem
```
mkdir -p ~/praktikum-os/week07-bash/bin
cat <<'EOF' > ~/praktikum-os/week07-bash/bin/sys-summary
#!/usr/bin/env bash
echo "=== SYSTEM SUMMARY ==="
echo "User: $(whoami)"
echo "Hostname: $(hostname)"
echo "Uptime: $(uptime -p)"
EOF
chmod +x ~/praktikum-os/week07-bash/bin/sys-summary
```

### Tugas Praktikum 2: Audit File Konfigurasi

<img width="646" height="58" alt="image" src="https://github.com/user-attachments/assets/3665d0cb-c953-4a93-9425-63a2a6617cb8" />

##### Langkah Eksekusi:

```
# Masuk ke workspace
cd ~/praktikum-os/week07-bash
# Cari file .conf di /etc, simpan hasil ke laporan, dan error ke log terpisah
find /etc -name "*.conf" > audit-konfigurasi-$(date +%F).txt 2> audit-error.log
# Catat jumlah file yang ditemukan ke dalam laporan
echo "Total file ditemukan: $(wc -l < audit-konfigurasi-$(date +%F).txt)" | tee -a audit-konfigurasi-$(date +%F).txt
```

Analisis Singkat: Pemisahan stdout (1) dan stderr (2) penting agar laporan audit bersih dari pesan "Permission Denied", sementara semua error tetap terdokumentasi untuk pengecekan akses nantinya.

### Tugas Praktikum 3: Mini Health Check Harian

Membuat script otomatis untuk memeriksa kondisi server sebelum maintenance.

<img width="528" height="241" alt="image" src="https://github.com/user-attachments/assets/4dad886a-6da5-4fd5-8c1a-9d559ed5c096" />

##### Script daily-healthcheck:
```
cat <<'EOF' > ~/praktikum-os/week07-bash/bin/daily-healthcheck
#!/usr/bin/env bash
{
    echo "=== HEALTH CHECK $(date) ==="
    echo "Hostname: $(hostname)"
    echo "User: $USER | Shell: $0"
    echo "Uptime: $(uptime -p)"
    echo "Memory Usage:"
    free -h
    echo "Root Filesystem:"
    df -h /
    echo "Recent History:"
    history | tail -n 10
} | tee healthcheck-$(date +%F).log
EOF
chmod +x ~/praktikum-os/week07-bash/bin/daily-healthcheck
```

### Tugas Praktikum 4: Penanganan File Kompleks
<img width="645" height="202" alt="image" src="https://github.com/user-attachments/assets/4c948a50-7fa6-4d05-a69e-43990e955d1a" />

##### Langkah Kerja:

1. Buat file contoh:
```
cd ~/praktikum-os/week07-bash/ruang-nama
touch "data survey 2026.csv" "backup[final]server.conf" "laporan-01.log" "laporan-02.log"
```

2. Preview Wildcard:
```
echo *.log
```

3. Proses dengan Quoting yang Benar:
```
tar -czf hasil-praktikum-4.tar.gz ../backup/*

# Untuk melihat hasil riwayat arisp
history | tail -n 10 > riwayat-arsip.txt
cat riwayat-arsip.txt
```















