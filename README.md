## Hosting Flask di Ubuntu Server dengan Let's Encrypt (HTTPS)
Tambahkan Let's Encrypt untuk mengamankan server dengan SSL/TLS menggunakan Certbot.
## Installation

#### 1. Install Certbot
Jalankan perintah berikut untuk menginstal Certbot dan plugin Nginx:
```sh
sudo apt update
```
```sh
sudo apt install certbot python3-certbot-nginx -y
```
#### 2. Update Konfigurasi Nginx
Edit konfigurasi Nginx untuk memastikan domain sudah dikonfigurasi dengan benar:
```sh
sudo nano /etc/nginx/sites-available/flaskapp
```
Pastikan ada baris ini:
```sh
server {
    listen 80;
    server_name your_domain_or_ip;

    location / {
        include proxy_params;
        proxy_pass http://unix:/var/www/flaskapp/flaskapp.sock;
    }
}

```
Simpan (CTRL+X, Y, Enter), lalu restart Nginx:
```sh
sudo systemctl restart nginx
```
#### 3. Dapatkan Sertifikat SSL dari Let's Encrypt
Jalankan perintah berikut untuk mendapatkan sertifikat SSL secara otomatis:
```sh
sudo certbot --nginx -d your_domain_or_ip
````
Gantilah your_domain_or_ip dengan domain atau IP server Anda.
Jika muncul opsi, pilih "redirect all HTTP to HTTPS".
#### 4. Verifikasi Sertifikat SSL
Cek apakah SSL berhasil terpasang:
```sh
sudo certbot certificates
````
Uji dengan membuka:
```sh
https://your_domain_or_ip
````
#### 5. Auto-Renewal SSL
Let's Encrypt SSL berlaku selama 90 hari. Untuk memperbaruinya secara otomatis, jalankan:
```sh
sudo certbot renew --dry-run
````
Jika berhasil, tambahkan cron job agar perpanjangan berjalan otomatis:
```sh
ssudo crontab -e
````
Tambahkan baris berikut:
```sh
0 0 * * * certbot renew --quiet && systemctl reload nginx
````
Simpan (CTRL+X, Y, Enter).
