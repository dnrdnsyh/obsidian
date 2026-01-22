# Monitoring RC
![[Pasted image 20260122230645.png]]

RC00 = Transaksi Sukses
RC0068 = Transaksi Gagal

# Monitoring Transaksi
![[Pasted image 20260122230726.png]]

**Jam Cut Off dimulai dari 23.45 - 00.30**
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
Script:

cek_pre
cek_post12d
cek_non