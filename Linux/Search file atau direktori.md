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

```
for file in \
"/data_ail/migrasi/jatim/518031725567/3/808C2D43-C86E-4C9C-8BBB-144C5DAFE77E.pdf" \
"/data_ail/migrasi/jatim/514570295387/4/6564A753-0D96-4EF4-A513-26423E8D8B25.pdf" \
"/data_ail/migrasi/jatim/514500738183/5/6058E82A-E149-4319-A9DF-26B397334CDA.pdf" \
"/data_ail/migrasi/jatim/511201133432/1/04A355FD-15ED-4D1A-9B1E-27084B5286BF.pdf" \
"/data_ail/migrasi/jatim/514570547691/4/7142ADB7-C160-44D1-8DF8-2772047835B3.pdf" \
"/data_ail/migrasi/jatim/516031123025/5/A6A63A0E-5D26-4551-B2A2-000A6247B5D9.pdf" \
"/data_ail/migrasi/jatim/511811944314/5/1FB9207C-AA4A-4D99-9E32-000CE909E3BA.pdf" \
"/data_ail/migrasi/jatim/515421279746/2/F8EE7823-2DF9-40DE-909A-001C76772841.pdf" \
"/data_ail/migrasi/jatim/511811944314/2/DAB24D91-45E2-42C5-9FA4-003A2514803A.pdf" \
"/data_ail/migrasi/jatim/511201099432/3/4082D9A5-6049-4249-873D-04D753E5F379.pdf" \
"/data_ail/migrasi/jatim/515421279180/2/F691B4E1-0D39-4793-B0CA-04DD1DD43E4E.pdf" \
"/data_ail/migrasi/jatim/511062743558/3/B02802EF-B21F-44BD-A47F-121621BF5C8C.pdf" \
"/data_ail/migrasi/jatim/514601096157/5/84A639F6-F362-4661-800B-12483A150AD1.pdf" \
"/data_ail/migrasi/jatim/511820000389/3/BCFED225-B292-43A0-AAD7-124BB27ADFD8.pdf" \
"/data_ail/migrasi/jatim/511201047512/3/AB12E9DE-1E58-45CC-B51B-1250755FD0AB.pdf" \
"/data_ail/migrasi/jatim/514601091491/5/5ECEE692-6D7F-4EAB-9836-1265584AFD8E.pdf" \
"/data_ail/migrasi/jatim/514601087169/5/3DB0C164-9473-455B-8BB4-1268C75185FC.pdf"; \

do
    if [ -f "$file" ]; then
        echo "[ADA] $file"
    else
        echo "[TIDAK ADA] $file"
    fi
done
```

# Copy multiple file
```
# buat file list.txt
nano list.txt

#isi dengan pathnya. contoh :
/migrasi/jatim/516031123025/5/A6A63A0E-5D26-4551-B2A2-000A6247B5D9.pdf
/migrasi/jatim/511811944314/5/1FB9207C-AA4A-4D99-9E32-000CE909E3BA.pdf
/migrasi/jatim/515421279746/2/F8EE7823-2DF9-40DE-909A-001C76772841.pdf


```
