# Modul 1 > Sesi 1: systemd dan Service

## Tujuan

Setelah sesi ini:

- Menjelaskan apa itu service dan peran systemd saat boot
- Membedakan `start`, `stop`, `restart`, `reload`, `enable`, `disable`
- Membaca output `systemctl status` dan tahu langkah berikutnya saat service gagal
- Membuat service sendiri dan mengubah service bawaan tanpa merusak file aslinya

## Konsep

### Service

Service (disebut juga daemon) adalah program yang berjalan terus di latar belakang.

- `ls` dijalankan, bekerja, lalu selesai
- `nginx` dijalankan sekali, lalu hidup terus menunggu request sampai dimatikan

### Systemd

Urutan saat server dinyalakan:

```text
Power > BIOS/UEFI > GRUB > Kernel > systemd (PID 1) > semua service lain
```

systemd adalah program pertama yang dijalankan kernel. Tugasnya:

- Menyalakan service sesuai urutan
- Mengawasi service yang berjalan
- Menghidupkan ulang service yang crash
- Mencatat log lewat `journald`

### Unit

| Jenis | Fungsi | Contoh|
| :--- | :--- | :--- |
|`.service` | Program yang dijalankan | `nginx.service` |
|`.timer` | Penjadwal | `apt-daily.timer` |
|`.socket` | Menunggu koneksi | `ssh.socket` |
| `.target` | Kelompok unit / "mode" sistem | `multi-user.target`|
| `.mount` | Titik mount | `home.mount` |

### Unit file dan lokasinya

- `/lib/systemd/system/` berisi file bawaan package. **jangan diedit**, karena akan tertimpa saat update
- `/etc/systemd/system/` berisi file buatanmu dan override. File di sini menang atas file bawaan

Contoh unit file:

```bash
[Unit]
Description=Nginx web server
After=network.target

[Service]
ExecStart=/usr/sbin/nginx
ExecReload=/usr/sbin/nginx -s reload
Restart=on-failure

[Install]
WantedBy=multi-user.target
```


