
### 1️⃣ What is Observability?

**Answer:**
Observability ka matlab hai system ke andar kya ho raha hai usko **samajhna using data**.
Ye 3 cheezon se hoti hai:

* **Logs**
* **Metrics**
* **Traces**

---

### 2️⃣ Difference between Monitoring and Observability?

**Answer:**

* **Monitoring**: Pehle se pata hota hai kya check karna hai
* **Observability**: Unknown problems bhi samajh sakte hain

👉 Observability zyada powerful hai.

---

### 3️⃣ What are the 3 pillars of Observability?

**Answer:**

1. **Logs** – detailed events
2. **Metrics** – numbers (CPU, memory)
3. **Traces** – request ka complete path

---

## 📊 PROMETHEUS

### 4️⃣ What is Prometheus?

**Answer:**
Prometheus ek **monitoring tool** hai jo:

* Metrics collect karta hai
* Time-series data store karta hai
* Alerting support karta hai

---

### 5️⃣ Prometheus push karta hai ya pull?

**Answer:**
👉 **Pull model** use karta hai
Prometheus khud ja kar `/metrics` endpoint se data uthata hai.

---

### 6️⃣ What is PromQL?

**Answer:**
PromQL Prometheus ki **query language** hai
Is se metrics ko search, filter aur calculate karte hain.

Example:

```
rate(http_requests_total[5m])
```

---

### 7️⃣ What is an Exporter in Prometheus?

**Answer:**
Exporter ek tool hota hai jo metrics ko Prometheus format mein expose karta hai.

Examples:

* Node Exporter (CPU, RAM)
* Kubernetes metrics

---

### 8️⃣ What is Alertmanager?

**Answer:**
Alertmanager alerts ko manage karta hai:

* Email
* Slack
* PagerDuty

Alert spam ko control karta hai.

---

## 📈 GRAFANA

### 9️⃣ What is Grafana?

**Answer:**
Grafana ek **visualization tool** hai
Is se dashboards bante hain (graphs, charts).

---

### 🔟 Grafana and Prometheus relationship?

**Answer:**

* Prometheus → Data store
* Grafana → Data show karta hai

👉 Grafana Prometheus se data read karta hai.

---

### 1️⃣1️⃣ What is a Grafana Dashboard?

**Answer:**
Dashboard multiple panels ka group hota hai
Jaise:

* CPU usage
* Memory usage
* Pod status

---

## 🔗 OPENTELEMETRY

### 1️⃣2️⃣ What is OpenTelemetry?

**Answer:**
OpenTelemetry ek **standard framework** hai jo:

* Logs
* Metrics
* Traces collect karta hai

Vendor-neutral hai.

---

### 1️⃣3️⃣ Why OpenTelemetry is used?

**Answer:**

* One standard for observability
* Tool-independent
* Cloud native friendly

---

### 1️⃣4️⃣ What is OpenTelemetry Collector?

**Answer:**
Collector data:

* Receive karta hai
* Process karta hai
* Export karta hai (Prometheus, Jaeger, etc.)

---

## 🧭 JAEGER (Tracing)

### 1️⃣5️⃣ What is Jaeger?

**Answer:**
Jaeger ek **distributed tracing tool** hai
Ye request ka poora flow dikhata hai.

---

### 1️⃣6️⃣ What is a Trace?

**Answer:**
Trace ek request ka **full journey** hota hai
Jaise:
User → API → DB → Response

---

### 1️⃣7️⃣ Difference between Trace and Span?

**Answer:**

* **Trace** = complete request
* **Span** = request ka ek step

Trace multiple spans ka collection hota hai.

---

## 📄 FLUENT BIT (Logs)

### 1️⃣8️⃣ What is Fluent Bit?

**Answer:**
Fluent Bit ek **lightweight log collector** hai
Mostly Kubernetes mein use hota hai.

---

### 1️⃣9️⃣ Fluent Bit vs Fluentd?

**Answer:**

| Fluent Bit  | Fluentd         |
| ----------- | --------------- |
| Lightweight | Heavy           |
| Fast        | Slower          |
| Edge nodes  | Central logging |

---

### 2️⃣0️⃣ How logs flow using Fluent Bit?

**Answer:**

1. Pod logs collect
2. Parse logs
3. Send to:

   * Elasticsearch
   * Loki
   * Cloud logging

---

## 🧠 INTERVIEW TIP (VERY IMPORTANT)

👉 Ye sentence yaad rakhna:

> **“Prometheus for metrics, Grafana for visualization, Jaeger for tracing, Fluent Bit for logs, and OpenTelemetry to unify everything.”**

---
