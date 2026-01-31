```	
192.168.56.23:/nfs/pelayanan  /mnt/pelayanan   nfs   defaults,_netdev   0  0
```

### Kolom 1: Sumber (Device/Filesystem)
**Nilai:** `192.168.56.23:/nfs/pelayanan`
- **Maksudnya:** Ini adalah **lokasi data** yang ingin diambil.
- Karena ini menggunakan protokol NFS (Network File System), formatnya adalah `[IP Server]:[Folder di Server]`.
- Artinya: "Ambil folder `/nfs/pelayanan` yang berada di server dengan IP `192.168.56.23`".
### Kolom 2: Titik Kait (Mount Point)
**Nilai:** `/mnt/pelayanan`
- **Maksudnya:** Ini adalah **pintu masuk** di komputer lokal Anda.
- Artinya: Data dari server tadi akan "ditempel" dan bisa diakses melalui folder `/mnt/pelayanan` di komputer ini. (Folder ini harus sudah dibuat sebelumnya dengan perintah `mkdir`).
### Kolom 3: Tipe File System
**Nilai:** `nfs`
- **Maksudnya:** Ini adalah **jenis format atau protokol** yang digunakan.
- Artinya: Memberitahu sistem operasi (Linux) untuk berkomunikasi menggunakan protokol **NFS**. Jika ini harddisk lokal, biasanya isinya `ext4`, `xfs`, atau `ntfs`.
### Kolom 4: Opsi (Mount Options)
**Nilai:** `defaults,_netdev
- **Maksudnya:** Pengaturan khusus tentang cara folder tersebut dipasang.
- **`defaults`**: Menggunakan kumpulan opsi standar (seperti `rw` untuk baca-tulis, `auto` agar otomatis mount saat boot, dll).
- **`_netdev`**: **(Sangat Penting untuk NFS)** Opsi ini memberitahu sistem bahwa penyimpanan ini berada di jaringan.
    - _Fungsinya:_ Mencegah sistem mencoba me-mount folder ini **sebelum** koneksi internet/jaringan aktif saat booting. Tanpa opsi ini, proses booting bisa macet (hang) karena sistem mencari server NFS padahal Wifi/LAN belum nyambung.
### Kolom 5: Dump (Backup)
**Nilai:** `0`
- **Maksudnya:** Digunakan oleh utilitas backup lawas bernama `dump`.
- **`0`**: Jangan di-backup.
- **`1`**: Perlu di-backup.
- _Catatan:_ Untuk NFS dan penggunaan modern, hampir selalu diisi `0`.
### Kolom 6: Pass (Fsck Order)
**Nilai:** `0`
- **Maksudnya:** Urutan pengecekan error harddisk (filesystem check) saat booting.
- **`0`**: Jangan dicek.
- **`1`**: Cek pertama (biasanya khusus untuk root partition `/`).
- **`2`**: Cek urutan berikutnya.
- _Catatan:_ Untuk NFS, **WAJIB `0`**. Komputer lokal Anda tidak boleh mencoba memperbaiki (fsck) harddisk milik server orang lain. Itu tugas servernya sendiri.