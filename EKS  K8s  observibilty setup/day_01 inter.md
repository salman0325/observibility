
# 🔎 1. Observability

**Definition:**
Observability means understanding what is happening inside a system by looking at its data (metrics, logs, traces).

**Example:**
If your app is slow, observability helps you find *why* (CPU issue, DB slow, etc.).

---

# 🧱 2. Three Components of Observability

### 1. Metrics

Numbers that show system performance over time.
**Example:** CPU = 70%, Requests = 100/sec

---

### 2. Logs

Text records of events.
**Example:** “Error: database connection failed”

---

### 3. Traces

Show request flow between services.
**Example:**
User request → API → Database → Response

---

# ⚖️ 3. Monitoring vs Observability

### Monitoring

Checks known issues and gives alerts
**Example:** CPU > 80% → alert

### Observability

Explains *why* the issue happened
**Example:** CPU high because of memory leak

---

# ❓ 4. What is Monitoring?

**Definition:**
Monitoring means continuously checking system health using metrics.

**Example:**
Check CPU every 10 seconds and alert if high

---

# 📊 5. What is Metrics?

**Definition:**
Metrics are numeric values measured over time.

**Example:**

* CPU usage
* Memory usage
* Request count

---

# 📡 6. What is Prometheus?

**Prometheus**

**Definition:**
Prometheus is a tool that collects and stores metrics.

**Example:**
It collects:

* CPU usage
* Request count

---

# 🗄️ 7. What is Time Series Database?

**Definition:**
A database that stores data with timestamps.

**Example:**
10:00 → CPU 60%
10:01 → CPU 70%

---

# 🧠 8. What is PromQL?

**Definition:**
Query language used in Prometheus.

**Example:**

```id="p1"
rate(http_requests_total[1m])
```

Shows requests per second in last 1 minute

---

# 🖥️ 9. What is Node Exporter?

**Node Exporter**

**Definition:**
It collects server (node) metrics.

**Example:**

* CPU usage
* Memory usage
* Disk usage

---

# ☸️ 10. What is kube-state-metrics?

**kube-state-metrics**

**Definition:**
It shows Kubernetes object status.

**Example:**

* Pods running
* Pods failed

---

# 🌐 11. What is /metrics?

**Definition:**
An HTTP endpoint that shows metrics.

**Example:**

```id="p2"
/metrics → shows CPU, requests, etc.
```

---

# 🔧 12. What is OpenTelemetry?

**OpenTelemetry**

**Definition:**
A standard tool to collect metrics, logs, and traces.

**Example:**
App → OpenTelemetry → Prometheus / Jaeger

---

# 🏢 13. What is CNCF?

**Cloud Native Computing Foundation**

**Definition:**
An organization that manages cloud-native tools.

**Example:**
Kubernetes, Prometheus, OpenTelemetry

---


👉 “Observability helps us understand system behavior using metrics, logs, and traces.
Monitoring tells us when something is wrong, but observability helps us find why.”

---

