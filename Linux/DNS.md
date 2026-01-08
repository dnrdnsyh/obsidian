Update sistem dan install bind9
```
sudo apt update
sudo apt install bind9 bind9utils bind9-doc -y
```

Konfigurasi File Options
agar DNS Server bisa forward query ke DNS Publik (Contoh 8.8.8.8) jika domain tidak ditemukan di lokal.
Edit file `sudo /etc/bind/named.conf.options`

cari bagian `forwarders` dan sesuaikan
```
forwarders { 
	8.8.8.8; 8.8.4.4; };
```