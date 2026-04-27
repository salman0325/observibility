

## 🔍 1️⃣ What is Observability?

**Answer:**
Observability means understanding what is happening inside a system **using data**.
It is achieved through three main components:

* **Logs**
* **Metrics**
* **Traces**

---

## 🔄 2️⃣ Difference between Monitoring and Observability?

**Answer:**

* **Monitoring**: You already know what to check (predefined checks).
* **Observability**: You can also discover unknown issues.

👉 Observability is more powerful because it helps in debugging unexpected problems.

---

## 🧱 3️⃣ What are the 3 pillars of Observability?

**Answer:**

1. **Logs** – Detailed event records
2. **Metrics** – Numerical data (CPU, memory, etc.)
3. **Traces** – End-to-end request flow

---

## 📊 PROMETHEUS

### 4️⃣ What is Prometheus?

**Answer:**
Prometheus is a **monitoring tool** that:

* Collects metrics
* Stores time-series data
* Supports alerting

---

### 5️⃣ Does Prometheus use push or pull?

**Answer:**
👉 Prometheus uses a **pull model**
It fetches data from the `/metrics` endpoint itself.

---

### 6️⃣ What is PromQL?

**Answer:**
PromQL is the **query language of Prometheus**
It is used to search, filter, and calculate metrics.

**Example:**

```
rate(http_requests_total[5m])
```

---

### 7️⃣ What is an Exporter in Prometheus?

**Answer:**
An exporter is a tool that exposes metrics in a format Prometheus understands.

**Examples:**

* Node Exporter (CPU, RAM)
* Kubernetes exporters

---

### 8️⃣ What is Alertmanager?

**Answer:**
Alertmanager manages alerts and sends notifications via:

* Email
* Slack
* PagerDuty

It also helps reduce alert spam.

---

## 📈 GRAFANA

### 9️⃣ What is Grafana?

**Answer:**
Grafana is a **visualization tool** used to create dashboards (graphs, charts).

---

### 🔟 Relationship between Grafana and Prometheus?

**Answer:**

* Prometheus → Stores data
* Grafana → Visualizes data

👉 Grafana reads data from Prometheus.

---

### 1️⃣1️⃣ What is a Grafana Dashboard?

**Answer:**
A dashboard is a collection of panels showing different metrics like:

* CPU usage
* Memory usage
* Pod status

---

## 🔗 OPENTELEMETRY

### 1️⃣2️⃣ What is OpenTelemetry?

**Answer:**
OpenTelemetry is a **standard framework** that collects:

* Logs
* Metrics
* Traces

It is vendor-neutral.

---

### 1️⃣3️⃣ Why is OpenTelemetry used?

**Answer:**

* Provides a single standard for observability
* Tool-independent
* Works well with cloud-native systems

---

### 1️⃣4️⃣ What is OpenTelemetry Collector?

**Answer:**
The collector:

* Receives data
* Processes it
* Exports it (to Prometheus, Jaeger, etc.)

---

## 🧭 JAEGER (Tracing)

### 1️⃣5️⃣ What is Jaeger?

**Answer:**
Jaeger is a **distributed tracing tool**
It shows the complete request flow.

---

### 1️⃣6️⃣ What is a Trace?

**Answer:**
A trace represents the **full journey of a request**, for example:
User → API → Database → Response

---

### 1️⃣7️⃣ Difference between Trace and Span?

**Answer:**

* **Trace** = Complete request
* **Span** = A single step within the request

👉 A trace is made up of multiple spans.

---

## 📄 FLUENT BIT (Logs)

### 1️⃣8️⃣ What is Fluent Bit?

**Answer:**
Fluent Bit is a **lightweight log collector**, commonly used in Kubernetes.

---

### 1️⃣9️⃣ Fluent Bit vs Fluentd?

| Fluent Bit  | Fluentd         |
| ----------- | --------------- |
| Lightweight | Heavy           |
| Fast        | Slower          |
| Edge nodes  | Central logging |

---

### 2️⃣0️⃣ How do logs flow using Fluent Bit?

**Answer:**

1. Collect logs from pods
2. Parse logs
3. Send to destinations like:

   * Elasticsearch
   * Loki
   * Cloud logging

---

## 🧠 INTERVIEW TIP (VERY IMPORTANT)

👉 Remember this line:

> **“Prometheus for metrics, Grafana for visualization, Jaeger for tracing, Fluent Bit for logs, and OpenTelemetry to unify everything.”**

---

