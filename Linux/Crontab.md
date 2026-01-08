Digunakan untuk scheduler.
#### **Cara penggunaan :**
```
* * * * * script/perintah yang dijalankan
```

#### **Keterangan:** 
**Bintang ke-**		**Deskripsi**				**Rentang Nilai**
1						Menit					0 - 59
2						Jam						0 - 23
3						Tanggal				1 - 31
4						Bulan					1 - 12
5						Hari						0 - 6 (0 = Minggu)

#### **Cara konfigurasi crontab:**
`crontab -e` Edit file crontab
`crontab -l` List isi crontab yang sedang aktif
`crontab -r` Remove semua jadwal crontab

#### **Cara penggunaan:**
Menjalankan perintah setiap menit
`* * * * * /usr/bin/php /path/ke/skrip.sh >> /home/dian/cron-log.log 2>&1`

Menjalankan perintah setiap jam 12 malam
`0 0 * * * /usr/bin/mysqldump -u root -p password db_nama > /backup/db.sql`

Menjalankan perintah setiap hari minggu jam 4.30
`30 4 * * 0 /usr/bin/python3 /home/dian/maintenance.py`

**Catatan**
Jangan lupa berikan hak akses/permission eksekusi pada file skrip.