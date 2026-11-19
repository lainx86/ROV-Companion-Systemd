# ROV Companion Computer Startup Service

Service ini digunakan untuk memulai program kontrol ROV yang berada di Companion Computer (raspi, jetson, etc.) agar otomatis memulai process ketika computer startup.
Binary file dari program untuk ROV Companion Computer ada di [Link](https://github.com/aterkia/ROV-Companion-Control)

## Setup 
Jalankan ini di terminal:
bash```
sudo nano /etc/systemd/system/rov-control.service
```
isi rov-control.service dengan file yang sama yang ada pada repo ini.

lalu:
bash```
sudo systemctl daemon-reload
sudo systemctl restart rov-control.service
```
