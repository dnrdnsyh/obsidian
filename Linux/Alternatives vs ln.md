ln merupakan shortcut murni.
alternatives adalah managed shortcut.

ln harus hapus shortcut secara manual jika sudah tidak digunakan.
Sulit untuk melihat atau mengetahui shortcut apa saja yang sudah dibuat.

Alternatives bisa membuat beberapa shortcut dalam 1 group.
Bisa gonta-ganti versi dengan mudah. Ada 2 mode, yaitu mode manual dan otomatis.
Jika menggunakan mode otomatis, aplikasi akan otomatis berjalan berdasarkan angka prioritasnya.

**Contoh penggunaan:**

```
sudo update-alternatives --install /usr/bin/java java /usr/local/jdk21/bin/java 1
sudo update-alternatives --install /usr/bin/javac javac /usr/local/jdk21/bin/javac 1
```

**Keterangan:**

| **Argumen**  | **Argumen**                 | **Peran**                                            |
| ------------ | --------------------------- | ---------------------------------------------------- |
| **Link**     | `/usr/bin/java`             | Alamat umum yang dipanggil user.                     |
| **Name**     | `java`                      | Nama grup di sistem _alternatives_.                  |
| **Path**     | `/usr/local/jdk21/bin/java` | Lokasi file asli di folder instalasi.                |
| **Priority** | `1`                         | Angka prioritas (memprioritaskan angka paling besar) |

**Cek apakah berhasil didaftarkan**
`update-alternatives --display` atau `update-alternatives --get-selections`

**Cara ganti versi aplikasi**
`sudo update-alternatives --config java`

Akan muncul tampilan seperti ini
```
There are 2 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
* 0            /usr/local/jdk11/bin/java      1111      auto mode
  1            /usr/local/jdk11/bin/java      1111      manual mode
  2            /usr/local/jdk17/bin/java      1711      manual mode

Press <enter> to keep the current choice[*], or type selection number:
```

- Tanda **bintang (`*`)** ada di baris nomor **0**, artinya saat ini sistem menggunakan Java 11.
- Kita ingin menggunakan Java 17, yang berada di **Selection nomor 2**.

Ketik angka 2 lalu tekan Enter.
```
type selection number: 2
update-alternatives: using /usr/lib/jdk17/bin/java to provide /usr/bin/java (java) in manual mode
```

**Cara ubah menjadi mode manual**
`sudo update-alternatives --set <nama_program> <path_ke_binari>`
`sudo update-alternatives --set java /usr/local/bin/java`

**Cara ubah menjadi mode auto**
`sudo update-alternatives --auto <nama_program>`
`sudo update-alternatives --auto java`

**Cara hapus alternatives**
`sudo update-alternatives --remove <nama_program> <path_ke_binari>`
`sudo update-alternatives --remove java /usr/lib/jdk11/bin/java`

**Hapus seluruh grup**
`sudo update-alternatives --remove-all <nama_program>`
`sudo update-alternatives --remove-all java`

