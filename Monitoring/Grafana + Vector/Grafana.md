`sudo mkdir -p /etc/apt/keyrings`
`wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null`

`echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list`

```
sudo apt-get update
sudo apt-get install grafana -y
```


Edit konfig: `sudo nano /etc/prometheus/prometheus.yml`
```
- job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']
        labels:
          instance: 'monitoring-pusat'
      - targets: ['192.168.56.48:9090']
        labels:
          instance: 'harbor-server'
```

```
sudo systemctl daemon-reload
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```

========================================
Prometheus

```
sudo apt-get update
sudo apt-get install prometheus -y
sudo systemctl status prometheus
```