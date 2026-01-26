<<<<<<< HEAD
# 📁 1️⃣ `prometheus.yml` (PRODUCTION – EACH LINE COMMENTED)

```yaml
# ==================================================
# PROMETHEUS GLOBAL SETTINGS
# ==================================================
global:
  scrape_interval: 15s          # Har 15 second metrics scrape karega
  evaluation_interval: 15s      # Har 15 second alert rules evaluate hongi
  scrape_timeout: 10s           # Agar target slow ho to 10s baad timeout

# ==================================================
# ALERTMANAGER CONNECTION
# ==================================================
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - ALERTMANAGER_IP:9093   # Alertmanager ka address
            # 🔔 Isi line se Prometheus alerts bhejta hai

# ==================================================
# ALERT RULE FILES LOCATION
# ==================================================
rule_files:
  - "/etc/prometheus/rules/*.yml"  # Saare alert rules yahan se load honge

# ==================================================
# SCRAPE TARGETS
# ==================================================
scrape_configs:

  # ---------------- PROMETHEUS SELF ----------------
  - job_name: "prometheus"
    static_configs:
      - targets:
          - "localhost:9090"     # Prometheus khud monitor ho raha hai

  # ---------------- NODE EXPORTER ------------------
  - job_name: "node_exporter"
    static_configs:
      - targets:
          - "NODE1_IP:9100"      # Server 1
          - "NODE2_IP:9100"      # Server 2
          # CPU / RAM / Disk / Load metrics

  # ---------------- JENKINS METRICS ----------------
  - job_name: "jenkins"
    metrics_path: /prometheus   # Jenkins Prometheus plugin endpoint
    scheme: http                # https use karo agar TLS ho

    static_configs:
      - targets:
          - "JENKINS_IP:8080"

    basic_auth:
      username: prometheus      # Jenkins user
      password: STRONG_PASSWORD # Jenkins password

  # ---------------- BLACKBOX -----------------------
  - job_name: "blackbox_http"
    metrics_path: /probe        # Blackbox ka probe path
    params:
      module: [http_2xx]        # HTTP 200 expect karega

    static_configs:
      - targets:
          - https://jenkins.company.com/login
          - https://app.company.com/health

    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target   # Actual URL pass karta hai

      - target_label: __address__
        replacement: BLACKBOX_IP:9115  # Blackbox exporter ka address
```

---

# 🚨 2️⃣ `alert.rules.yml` (EXACT – COPY PASTE)

```yaml
groups:
- name: production-alerts
  rules:

  # -------- NODE DOWN --------
  - alert: NodeDown
    expr: up{job="node_exporter"} == 0
    for: 2m
    labels:
      severity: critical
    annotations:
      summary: "Node is DOWN"
      description: "Server {{ $labels.instance }} unreachable"

  # -------- JENKINS DOWN --------
  - alert: JenkinsDown
    expr: up{job="jenkins"} == 0
    for: 2m
    labels:
      severity: critical
    annotations:
      summary: "Jenkins is DOWN"

  # -------- JENKINS QUEUE HIGH --------
  - alert: JenkinsQueueHigh
    expr: jenkins_queue_size_value > 10
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "Jenkins queue too high"

  # -------- DISK LOW --------
  - alert: DiskSpaceLow
    expr: node_filesystem_avail_bytes{mountpoint="/"} < 10*1024*1024*1024
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "Disk space low"
```

---

# 📊 3️⃣ Grafana Dashboards (PRODUCTION STANDARD)

Use these **official dashboards** (Grafana.com):

* ✅ **Node Exporter Full** → ID `1860`
* ✅ **Jenkins Performance & Health**
* ✅ **Blackbox Exporter Overview**

👉 Steps:

1. Grafana → Dashboards → Import
2. Paste Dashboard ID
3. Select Prometheus datasource

---

# 🔐 4️⃣ TLS + AUTH HARDENING (PRODUCTION MUST)

### 🔒 Prometheus TLS

```yaml
web:
  tls_server_config:
    cert_file: /etc/prometheus/prometheus.crt
    key_file: /etc/prometheus/prometheus.key
```

### 🔐 Basic Auth (NGINX example)

```nginx
auth_basic "Restricted";
auth_basic_user_file /etc/nginx/.htpasswd;
```

### 🔥 Best practices

* Prometheus NOT public
* Jenkins metrics auth enabled
* Alertmanager internal only

---

# ☸️ 5️⃣ Kubernetes Version (Prometheus SCRAPE)

```yaml
- job_name: "kubernetes-pods"
  kubernetes_sd_configs:
    - role: pod

  relabel_configs:
    - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
      action: keep
      regex: true
```

Used with:

* kube-state-metrics
* node-exporter (DaemonSet)
* Prometheus Operator (production standard)

---

=======
# Task Master Pro

Welcome to Task Master Pro, a comprehensive Java application designed to manage tasks efficiently. This project is developed and maintained by DevOps Shack, a YouTube channel dedicated to DevOps tutorials and best practices.

## Table of Contents
- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Introduction

Task Master Pro is a task management application built using Java. It provides a robust set of features to help users create, manage, and track tasks. This project aims to demonstrate best practices in Java development, including project structure, coding standards, and documentation.

## Features

- Create, update, and delete tasks
- Mark tasks as complete or incomplete
- User authentication and authorization

## Installation

### Prerequisites

- Java Development Kit (JDK) 17 or later
- Apache Maven 3.6.0 or later
- A database (H2)

### Steps

1. Clone the repository:

    ```sh
    git clone https://github.com/jaiswaladi246/Task-Master-Pro.git
    cd Task-Master-Pro
    ```

2. Configure the database:

    Update the `application.properties` file with your database configuration.

3. Build the project:

    ```sh
    mvn clean install
    ```

4. Run the application:

    ```sh
    mvn spring-boot:run
    ```

## Usage

Once the application is running, you can access it at `http://localhost:8080`. You can use the web interface to manage your tasks.

### Endpoints

- `/tasks` - View and manage tasks
- `/tasks/{id}` - View, update, or delete a specific task
- `/login` - User login
- `/register` - User registration

## Contributing

We welcome contributions to improve Task Master Pro. If you have a feature request, bug report, or improvement suggestion, please open an issue or submit a pull request.

### Steps to Contribute

1. Fork the repository
2. Create a new branch (`git checkout -b feature-branch`)
3. Make your changes
4. Commit your changes (`git commit -m 'Add some feature'`)
5. Push to the branch (`git push origin feature-branch`)
6. Open a pull request

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## Contact

For any questions or inquiries, please reach out to us at [DevOps Shack](https://www.youtube.com/@devopsshack/videos)

Happy coding!
>>>>>>> master
