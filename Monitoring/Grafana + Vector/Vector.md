#1. Unduh file instalasi Vector versi terbaru
wget https://packages.timber.io/vector/0.34.1/vector_0.34.1-1_amd64.deb

#2. Instal file tersebut
sudo dpkg -i vector_0.34.1-1_amd64.deb

#3. Jika ada error dependensi (jarang terjadi), perbaiki dengan:
sudo apt-get install -f

Verifikasi versi
vector --version

Edit konfig : sudo nano /etc/vector/vector.yaml

Tambahkan konfig ini:
sources:
  host_metrics:
    type: host_metrics
    collectors: [cpu, mem, disk, network]
    scrape_interval_secs: 15
  docker_metrics:
    type: docker_metrics

sinks:
  prom_exporter:
    type: prometheus_exporter
    inputs: [host_metrics, docker_metrics]
    address: "0.0.0.0:9090"


Cara untuk batasi resource, Jalankan perintah ini untuk membuat _override_ konfigurasi service 
sudo systemctl edit vector

Tambahkan konfig ini:
[Service]
MemoryMax=50M
MemoryHigh=40M

Jalankan service:
sudo systemctl daemon-reload
sudo systemctl restart vector
sudo systemctl enable vector

curl http://localhost:9090/metrics