# ROV Companion Computer Startup Service

Service ini digunakan untuk menjalankan program **ROV Companion Control** secara otomatis ketika companion computer seperti Raspberry Pi, Jetson, atau perangkat Linux lainnya dinyalakan.

Program ROV Companion Control tersedia di:

https://github.com/aterkia/ROV-Companion-Control

Service menjalankan program menggunakan `systemd` dan `tmux`, sehingga program dapat berjalan secara headless tanpa memerlukan monitor, desktop, RDP, maupun koneksi SSH.

## Setup

Pastikan `tmux` sudah terpasang:

```bash
sudo apt install -y tmux
```

Buat file service:

```bash
sudo nano /etc/systemd/system/rov-control.service
```

Isi file tersebut dengan konfigurasi `rov-control.service` yang tersedia di repository ini.

Pastikan path binary, username, interface Ethernet, device Pixhawk, dan environment yang digunakan di dalam service sudah sesuai dengan konfigurasi companion computer.

Setelah itu, reload konfigurasi `systemd`:

```bash
sudo systemctl daemon-reload
```

Aktifkan service agar otomatis berjalan setiap kali komputer dinyalakan, sekaligus jalankan service sekarang:

```bash
sudo systemctl enable --now rov-control.service
```

## Cek Status

Untuk memastikan service berjalan:

```bash
systemctl status rov-control.service
```

Status yang diharapkan:

```text
Active: active (running)
```

## Melihat ROV Control

Program berjalan di dalam session `tmux` bernama `rov`.

Untuk melihat tampilan program melalui SSH:

```bash
TERM=xterm-256color tmux attach -t rov
```

Keluar dari tampilan `tmux` tanpa menghentikan program dengan:

```text
Ctrl+B
D
```

Program akan tetap berjalan di background setelah session `tmux` dilepas.

## Restart Service

Jika konfigurasi atau binary diperbarui:

```bash
sudo systemctl restart rov-control.service
```

## Stop Service

Untuk menghentikan program:

```bash
sudo systemctl stop rov-control.service
```

## Nonaktifkan Autostart

Untuk mencegah service berjalan otomatis saat boot:

```bash
sudo systemctl disable rov-control.service
```

## Log

Untuk melihat log service:

```bash
journalctl -u rov-control.service -f
```

Dengan konfigurasi ini, alur startup companion computer adalah:

```text
Power On
→ Linux Boot
→ systemd
→ menunggu perangkat dan jaringan siap
→ tmux
→ ROV Companion Control
```

SSH hanya diperlukan untuk monitoring atau maintenance. Program ROV Companion Control tetap berjalan secara otomatis tanpa koneksi SSH.
