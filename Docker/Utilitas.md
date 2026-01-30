# Cek IP Node
```
docker node ls --format "{{.Hostname}}" | xargs -I {} docker node inspect {} --format "{{.Description.Hostname}}: {{.Status.Addr}}"
```

# Cek lokasi deployment

`docker ps | grep <port_aplikasi>
`#8089 adalah port aplikasi yang dituju. atau bisa langsung saja`
`docker ps`

`docker service ls | grep <port_service>
![[Pasted image 20260130205852.png]]

`docker service ps | grep urp7oo75qtoh`
![[Pasted image 20260130210010.png]]

`docker ps | grep <id_service>`
![[Pasted image 20260130210426.png]]

`docker exec -t <id_container> /bin/sh`
![[Pasted image 20260130210414.png]]