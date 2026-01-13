#1. Tambahkan GPG Key
`curl -L https://apt.vector.dev/gpg.key | gpg --dearmor | sudo tee /usr/share/keyrings/vector-archive-keyring.gpg > /dev/null`

#2. Tambahkan Repository ke Sources List
`echo "deb [signed-by=/usr/share/keyrings/vector-archive-keyring.gpg] https://apt.vector.dev stable main" | sudo tee /etc/apt/sources.list.d/vector.list`

#3. Update dan Install
```
sudo apt-get update
sudo apt-get install vector -y

vector --version
```

Tambahkan konfig ini di `sudo nano /etc/vector/vector.yaml`:
```
data_dir: "/var/lib/vector"

sources:
  my_host_metrics:
    type: "host_metrics"
    namespace: "node"
    collectors:
      - cpu
      - memory
      - disk
      - network
      - load
      - cgroups
      - filesystem
      - host
    scrape_interval_secs: 15

sinks:
  prom_exporter:
    type: "prometheus_exporter"
    inputs: ["my_host_metrics"]
    address: "0.0.0.0:9090"
```



Cara untuk batasi resource, Jalankan perintah ini untuk membuat _override_ konfigurasi service 
`sudo systemctl edit vector`

Tambahkan konfig ini:
```
[Service]
MemoryMax=50M
MemoryHigh=40M
```
Jalankan service:
```
sudo systemctl daemon-reload
sudo systemctl restart vector
sudo systemctl enable vector

curl http://localhost:9090/metrics
```

Catatan:
Sesuaikan Json node exporter agar bisa terbaca.