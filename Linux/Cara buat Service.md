```
#buat service di /etc/systemd/system	
nano /etc/systemd/system/nginx.service

[Unit]
Description=The NGINX HTTP and reverse proxy server
After=syslog.target network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
#tambahkan ini jika ingin diakses oleh user tertentu saja.
#User=root 
#Group=root
Type=forking
PIDFile=/opt/nginx/logs/nginx.pid

# Path ke file binary di /opt/nginx/sbin/nginx
ExecStartPre=/opt/nginx/sbin/nginx -t
ExecStart=/opt/nginx/sbin/nginx
ExecReload=/opt/nginx/sbin/nginx -s reload
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Keterangan

Ini adalah bagian terpenting yang mengatur bagaimana file di `/opt/nginx` dieksekusi.

- **`Type=forking`**: Nginx bekerja dengan cara _master process_ memanggil beberapa _worker process_. Systemd perlu tahu bahwa setelah menjalankan perintah, proses utama akan membelah diri (_fork_).
- **`PIDFile`**: Lokasi di mana Nginx menyimpan nomor ID prosesnya. Systemd memantau file ini untuk mengetahui apakah Nginx masih hidup.
- **`ExecStartPre`**: Perintah yang dijalankan **sebelum** Nginx benar-benar start. Di sini digunakan `nginx -t` untuk mengecek apakah ada kesalahan ketik di konfigurasi. Jika error, Nginx tidak akan dipaksa jalan.
- **`ExecStart`**: Perintah utama untuk menjalankan Nginx dari folder `/opt` Anda.
- **`ExecReload`**: Perintah saat Anda mengetik `systemctl reload nginx`. Ini mengirim sinyal ke Nginx untuk membaca ulang konfigurasi tanpa mematikan koneksi yang sedang berjalan.
- **`ExecStop`**: Cara mematikan Nginx secara halus (_graceful shutdown_) menggunakan sinyal `QUIT`.
- **`Restart=always` & `RestartSec=5`**: Fitur _self-healing_. Jika Nginx mati mendadak, Systemd akan mencoba menyalakannya lagi secara otomatis setiap 5 detik.
- **`PrivateTmp=true`**: Fitur keamanan yang memberikan folder `/tmp` terisolasi khusus untuk Nginx agar tidak bisa diintip proses lain.
- **`WantedBy=multi-user.target`**: Ini artinya servis ini akan aktif pada mode "Multi-User" (kondisi normal Linux saat siap digunakan). Tanpa baris ini, perintah `systemctl enable` tidak akan berfungsi.

Update sistem daemon
`sudo systemctl daemon-reload`

Jalankan service
`sudo systemctl start nginx`

Enable on boot
`sudo systemctl enable nginx`