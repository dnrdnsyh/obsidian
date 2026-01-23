# Monitoring RC
![[Pasted image 20260122230645.png]]

RC00 = Transaksi Sukses
RC0068 = Transaksi Gagal

# Monitoring Transaksi
![[Pasted image 20260122230726.png]]

**Jam Cut Off dimulai dari 23.45 - 00.30**
Jumlah Service Per-Server :
10 Postpaid Service
6 Prepaid Service
1 Nontaglis Service

![[Pasted image 20260122231010.png]]
![[Pasted image 20260122231034.png]]

OLAP = Aplikasi
```
70 truno
10.70.1.48
10.70.1.49
10.70.1.50

71 gandul
10.71.1.51
10.71.1.52
10.71.1.53
```

OLTP = gateway
```
10.70.1.78
10.71.1.78
```

`Kill -15 kalau ada yg stuck`

```
Masuk Ke OLTP lalu jalankan Script:

cek_pre
cek_post12d
cek_non

Jika ada error, cek errornya dimana lalu masuk ke OLAP sesuai dengan service yang error lalu cek.
Pastikan service berjalan, cek port.
lsof -i | grep <port>

Jika tidak ada, cek service melalui systemctl status <postpaid/prepaid/nontgalis_service-berapa>. contoh
systemctl status postpaid_10
Jika terkendala lakukan restart. untuk jaga-jaga, stop dulu, tunggu beberapa menit, start kembali.

Cek service, pastikan service berjalan dan
Cek lsof -i | grep <port> lagi, pastikan service berjalan.