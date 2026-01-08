Digunakan untuk menerjemahkan IP Address menjadi Domain dan sebaliknya.
Contoh:
**IP Server** 192.168.56.24
**Domain** iconpln.co.id

Ketika ping atau akses *iconpln.co.id* akan diarahkan ke 192.168.56.24

#### **Update sistem dan install bind9**
```
sudo apt update
sudo apt install bind9 bind9utils bind9-doc -y
```

#### **Konfigurasi File Options**
agar DNS Server bisa forward query ke DNS Publik (Contoh 8.8.8.8) jika domain tidak ditemukan di lokal.
Edit file `sudo /etc/bind/named.conf.options`

cari bagian `forwarders` dan sesuaikan
```
forwarders { 
	8.8.8.8; 
	8.8.4.4; 
};
```

#### **Konfigurasi Zone Lokal**
Perlu mendefinisikan zona untuk domain baru. 
edit file `sudo /etc/bind/named.conf.local`

```
// Forward Zone (Domain ke IP)
zone "iconpln.co.id" {
    type master;
    file "/etc/bind/db.iconpln.co.id";
};

// Reverse Zone (IP ke Domain)
zone "56.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.192";
};
```

#### **Membuat File Database Forward**
Copy template file db.
```
sudo cp /etc/bind/db.local /etc/bind/db.iconpln.co.id
sudo nano /etc/bind/db.iconpln.co.id
```

Ubah isinya menjadi seperti ini:
```
;
; BIND data file for iconpln.co.id
;
$TTL    604800
@       IN      SOA     ns1.iconpln.co.id. admin.iconpln.co.id. (
                              3         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.iconpln.co.id.
@       IN      A       192.168.56.24
ns1     IN      A       192.168.56.24
www     IN      A       192.168.56.24
```

#### **Membuat File Database Reverse**
Copy template file db reverse.
```
sudo cp /etc/bind/db.127 /etc/bind/db.192
sudo nano /etc/bind/db.192
```

Ubah isinya menjadi seperti ini:
```
;
; BIND reverse data file for 192.168.56.xx
;
$TTL    604800
@       IN      SOA     ns1.iconpln.co.id. admin.iconpln.co.id. (
                              3         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.iconpln.co.id.
24      IN      PTR     iconpln.co.id.
```

#### **Cek konfigurasi**
Cek apakah ada typo atau salah konfig
```
sudo named-checkconf
sudo named-checkzone iconpln.co.id /etc/bind/db.iconpln.co.id
```

Jika tidak ada error, restart
`sudo systemctl restart bind9`

#### **Pengujian**
```
sudo nano /etc/resolv.conf
# Tambahkan baris paling atas:
nameserver 127.0.0.1
```

```
nslookup iconpln.co.id
nslookup 192.168.56.24
```

Pastikan firewall sudah dibuka. `sudo ufw allow 53/tcp` dan `sudo ufw allow 53/udp`