
# AWS Monitoring Setup (Prometheus + Grafana)

## Architecture Overview

Humare paas **2 AWS Ubuntu EC2 instances** hain:

### Instance 1 (App Server)
- Application running
- Node Exporter installed (system metrics)

### Instance 2 (Monitoring Server)
- Prometheus
- Blackbox Exporter
- Grafana

Prometheus metrics scrape karega:
- Node Exporter (Instance 1)
- Blackbox Exporter (HTTP/TCP checks)

Grafana dashboards ke through visualization hogi.

---

## Instance Details (Example)

| Service | IP |
|-------|----|
| App + Node Exporter | `INSTANCE_1_PRIVATE_IP:9100` |
| Prometheus | `INSTANCE_2_PRIVATE_IP:9090` |
| Blackbox Exporter | `INSTANCE_2_PRIVATE_IP:9115` |
| Grafana | `INSTANCE_2_PRIVATE_IP:3000` |

---

## Download Links (Official)

- Prometheus  
  👉 https://prometheus.io/download/

- Node Exporter  
  👉 https://prometheus.io/download/

- Blackbox Exporter  
  👉 https://prometheus.io/download/

- Grafana  
  👉 https://grafana.com/grafana/download

---

## Instance 1: Node Exporter Setup

```bash
sudo useradd --no-create-home --shell /bin/false node_exporter

wget https://github.com/prometheus/node_exporter/releases/latest/download/node_exporter-*.linux-amd64.tar.gz
tar xvf node_exporter-*.tar.gz
cd node_exporter-*/
sudo cp node_exporter /usr/local/bin/
````

### Node Exporter Service

```bash
sudo nano /etc/systemd/system/node_exporter.service
```

```ini
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=default.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
```

Verify:

```
http://INSTANCE_1_IP:9100/metrics
```

---

## Instance 2: Prometheus Setup

```bash
wget https://github.com/prometheus/prometheus/releases/latest/download/prometheus-*.linux-amd64.tar.gz
tar xvf prometheus-*.tar.gz
cd prometheus-*/
```

---

## Blackbox Exporter Setup (Instance 2)

```bash
wget https://github.com/prometheus/blackbox_exporter/releases/latest/download/blackbox_exporter-*.linux-amd64.tar.gz
tar xvf blackbox_exporter-*.tar.gz
cd blackbox_exporter-*/
```

Run:

```bash
./blackbox_exporter
```

Verify:

```
http://INSTANCE_2_IP:9115
```

---

## Prometheus Configuration (`prometheus.yml`)

```yaml
global:
  scrape_interval: 15s

scrape_configs:

  - job_name: "node_exporter"
    static_configs:
      - targets:
          - "INSTANCE_1_PRIVATE_IP:9100"

  - job_name: "blackbox_http"
    metrics_path: /probe
    params:
      module: [http_2xx]
    static_configs:
      - targets:
          - http://INSTANCE_1_PRIVATE_IP:APP_PORT
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - target_label: __address__
        replacement: INSTANCE_2_PRIVATE_IP:9115
```

Restart Prometheus:

```bash
./prometheus --config.file=prometheus.yml
```

Verify:

```
http://INSTANCE_2_IP:9090/targets
```

---

## Grafana Setup (Instance 2)

```bash
sudo apt-get install -y apt-transport-https software-properties-common
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
sudo apt update
sudo apt install grafana
```

```bash
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

Access:

```
http://INSTANCE_2_IP:3000
```

Default login:

* user: `admin`
* password: `admin`

---

## Grafana Dashboards

### Node Exporter Dashboard

* **ID:** `1860`

### Blackbox Exporter Dashboard

* **ID:** `7587`

Import Steps:

1. Grafana → Dashboards → Import
2. Enter Dashboard ID
3. Select Prometheus datasource
4. Import 🎉

---

## Final Flow

```
Node Exporter → Prometheus → Grafana
Blackbox Exporter → Prometheus → Grafana
```

---

## Done ✅

Ab tum:

* Server metrics dekh sakte ho
* App uptime monitor kar sakte ho
* Alerts future me add kar sakte ho

Happy Monitoring 🚀

```

