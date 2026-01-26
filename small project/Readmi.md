
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

- AleartManager
  https://github.com/prometheus/alertmanager/releases/download/v0.30.1/alertmanager-0.30.1.linux-amd64.tar.gz

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

```bash
route:                                # Alertmanager ka main routing section
  group_by: ['alertname']             # Alerts ko alertname ke basis par group karega
  group_wait: 30s                     # Pehla alert bhejne se pehle 30 sec wait karega
  group_interval: 5m                  # Same group ke alerts ke beech 5 min ka gap
  repeat_interval: 1h                 # Same alert dobara 1 ghante baad bhejega
  receiver: email-notifications       # Default receiver ka naam

receivers:                            # Receivers ki list
  - name: email-notifications         # Receiver ka naam
    email_configs:                    # Email configuration section
      - to: jaiswalad246@gmail.com    # Jis email par alert bhejna hai
        from: test@gmail.com          # Sender email address
        smarthost: smtp.gmail.com:587 # Gmail SMTP server aur port
        auth_username: jaiswalad246@gmail.com  # SMTP login username
        auth_identity: jaiswalad246@gmail.com  # SMTP identity
        auth_password: tpqo ntic asso frgb     # Gmail app password
        send_resolved: true           # Alert resolve hone par bhi email bheje

inhibit_rules:                       # Inhibition rules section
  - source_match:                    # Source alert ka match
      severity: critical             # Agar severity critical ho
    target_match:                    # Target alert ka match
      severity: warning              # Aur doosra alert warning ho
    equal:                           # Dono alerts mein yeh labels same hon
      - alertname                    # Alert ka naam same ho
      - dev                          # Dev label same ho
      - instance                     # Instance label same ho
```

```bash
# =========================
# GLOBAL CONFIGURATION
# =========================
global:
  scrape_interval: 15s        # Prometheus har 15 second mein targets scrape karega
  evaluation_interval: 15s    # Alert rules har 15 second mein evaluate hongi

# =========================
# ALERTMANAGER CONFIG
# =========================
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - localhost:9093
            # ↑ Alertmanager ka address
            # ↑ ISI line ki wajah se Prometheus alerts Alertmanager ko bhejta hai
            # ↑ Yehi 3rd STEP hai (Prometheus → Alertmanager)

# =========================
# ALERT RULE FILES
# =========================
rule_files:
  - "alert.rules.yml"
  # ↑ Is file mein node_exporter / blackbox ke alerts defined hote hain
  # ↑ Example: NodeDown, BlackboxDown, HighCPU etc.

# =========================
# SCRAPE CONFIGURATIONS
# =========================
scrape_configs:

  # -------------------------
  # NODE EXPORTER JOB
  # -------------------------
  - job_name: "node_exporter"
    # ↑ Job ka naam (labels mein job="node_exporter" aayega)

    static_configs:
      - targets:
          - "INSTANCE_1_PRIVATE_IP:9100"
          # ↑ Node Exporter ka IP aur port
          # ↑ Agar ye down hua to:
          # ↑ up{job="node_exporter"} = 0 ho jaayega

  # -------------------------
  # BLACKBOX EXPORTER JOB
  # -------------------------
  - job_name: "blackbox_http"
    # ↑ Job ka naam blackbox monitoring ke liye

    metrics_path: /probe
    # ↑ Blackbox exporter ka fixed path

    params:
      module: [http_2xx]
      # ↑ http_2xx module use karega
      # ↑ Matlab HTTP 200 response expect karega

    static_configs:
      - targets:
          - http://INSTANCE_1_PRIVATE_IP:APP_PORT
          # ↑ Ye actual application / URL hai
          # ↑ Isi URL ko blackbox probe karega

    relabel_configs:
      # ↓ Target URL ko blackbox ko pass karta hai
      - source_labels: [__address__]
        target_label: __param_target

      # ↓ Blackbox exporter ka actual address
      - target_label: __address__
        replacement: INSTANCE_2_PRIVATE_IP:9115
        # ↑ Yahan blackbox_exporter run ho raha hota hai
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

