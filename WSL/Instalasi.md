```
#Open Powershell as administrator

#Cek versi linux
wsl --list --online

#Semisal versi ubuntu 24
wsl --install-d Ubuntu-24.04

#Ubah versi wsl 1 ke 2
#WSL 2 lebih optimal untuk container (docker, kuber)
wsl --set-version Ubuntu-24.04 2
```