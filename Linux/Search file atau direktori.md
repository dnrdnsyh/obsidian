`find [lokasi] [kriteria] [nama_file]`

Cari file case-sensitive
`find / -name "index.php"`

Cari file case-insensitive
`find /home -iname "config.php"`

Cari direktori
`find /var/www -type d -name "laravel"`

Cari file berdasarkan ekstensi
`find . -type f -name "*.log"`

Cari file berdasarkan size (contoh ukuran lebih dari 100MB)
`find / -size +100M`

===================================================

Cari lokasi binary/Aplikasi
Menampilkan path file eksekusi yang sedang aktif
`which nginx`

Menampilkan lokasi source code, binary, dan manual page
`whereis php`