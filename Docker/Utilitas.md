```
docker node ls --format "{{.Hostname}}" | xargs -I {} docker node inspect {} --format "{{.Description.Hostname}}: {{.Status.Addr}}"
```