# 📊 Observability with Prometheus, Alertmanager, EFK & Jaeger

This project demonstrates how to implement **observability** in microservices by combining **metrics, logging, tracing, and alerting**.  
You’ll learn how to instrument applications, visualize metrics, collect logs, set up alerts, and trace requests across services.

---
# The Process of create Costume Metric
- ``Instrument`` the Metric
- ``Set up Prometheus`` or your complete Monitoring
- ``Service Discovery`` It tells you the service that you want to Identify

These 3 Process is the Process of Create Costume Metric. you can additionally set up an alert with ``Gmail``.

## 🎛️ What is Instrumentation?
Instrumentation refers to adding monitoring capabilities to applications, systems, or services.  
This involves embedding code or using tools to collect **metrics, logs, or traces** that provide insights into how the system is performing.

### 🎯 Purpose of Instrumentation
- **Visibility** → Gain insight into application & infrastructure state.  
- **Metrics Collection** → Collect CPU usage, memory, request rates, error rates, etc.  
- **Troubleshooting** → Diagnose failures faster with detailed insights.  

### ⚙️ How It Works
- **Code-Level Instrumentation** → Add code to expose metrics.  
  Example: in Node.js use [`prom-client`](https://github.com/siimon/prom-client) to create custom metrics.

---

## 📈 Metrics in Prometheus

### 🔄 Counter
- Always increases.  
- Example: count HTTP requests, errors, or container restarts.  
- **Metric Example:** `kube_pod_container_status_restarts_total`

### 📏 Gauge
- Goes up and down.  
- Example: memory usage, CPU usage, active users.  
- **Metric Example:** `container_memory_usage_bytes`

### 📊 Histogram
- Measures observations in buckets (e.g., request durations).  
- Provides **count, sum, and bucket breakdowns**.  
- **Metric Example:** `apiserver_request_duration_seconds_bucket`

### 📝 Summary
- Similar to histogram, but with **quantiles (percentiles)**.  
- Example: 95th percentile request duration.  
- **Metric Example:** `apiserver_request_duration_seconds_sum`

---

## 🏠 Architecture

![Project Architecture](images/architecture.gif)

### Components:
- **Service A & B (Node.js apps)** → instrumented with `prom-client`.  
- **Prometheus** → scrapes metrics from services.  
- **Alertmanager** → sends alerts (via email/SMTP).  
- **EFK Stack** → centralized logging (Elasticsearch, FluentBit, Kibana).  
- **Jaeger** → distributed tracing for request flows.  
- **Kubernetes** → orchestration and service discovery.  

---

## 🎯 Project Objectives
- 🛠️ **Implement Custom Metrics in Node.js** with `prom-client`.  
- 🚨 **Set Up Alerts in Alertmanager** (e.g., pod restart > 2 times).  
- 📝 **Centralized Logging with EFK**.  
- 📸 **Distributed Tracing with Jaeger**.  

---

## 1️⃣ Write Custom Metrics

File: `day-4/application/service-a/index.js`

Metrics implemented:
- `http_requests_total` → Counter  
- `http_request_duration_seconds` → Histogram  
- `http_request_duration_summary_seconds` → Summary  
- `node_gauge_example` → Gauge  

### Routes
- `/` → Running status  
- `/healthy` → Health check  
- `/serverError` → Simulate 500  
- `/notFound` → Simulate 404  
- `/logs` → Generate logs  
- `/crash` → Simulate crash  
- `/example` → Async gauge  
- `/metrics` → Prometheus metrics  
- `/call-service-b` → Call service B  

---

## 2️⃣ Dockerize & Push to Registry
```bash
cd day-4

# Service A
docker build -t <REPO>:<TAG> application/service-a/
# Service B
docker build -t <REPO>:<TAG> application/service-b/

```
3️⃣ Deploy to Kubernetes
```bash
kubectl create ns dev
kubectl apply -k kubernetes-manifest/
```

4️⃣ Test Endpoints
Get the LoadBalancer DNS and test:

```bash
Copy code
/
 /healthy
 /serverError
 /notFound
 /logs
 /example
 /metrics
 /call-service-b
Automated test script:
```
```bash
./test.sh <LOAD_BALANCER_DNS_NAME>
```
## 5️⃣ Configure Alertmanager
Files: day-4/alerts-alertmanager-servicemonitor-manifest
```
Steps:

1. Generate Gmail App Password (or AWS SES SMTP).

2. Put it in email-secret.yml.

3. Add your email in alertmanagerconfig.yml.

Alerts
HighCpuUsage → Warning if CPU > 50% for 5m.

PodRestart → Critical if pod restarts > 2 times.
```
Apply:

```bash
kubectl apply -k alerts-alertmanager-servicemonitor-manifest/
```
## 6️⃣ Centralized Logging with EFK
FluentBit (DaemonSet) Collects logs from pods.

Forwards to Elasticsearch.

Example config ``(fluentbit-configmap.yml)``:

```bash
Copy code
[OUTPUT]
    Name es
    Match *
    Host elasticsearch
    Port 9200
    Index kubernetes-logs
```
## Elasticsearch
Deploy with StatefulSet (elasticsearch.yml).

## Kibana
Deploy UI (kibana.yml) and access logs via browser.

## 7️⃣ Distributed Tracing with Jaeger
Install Jaeger in Kubernetes
```bash
kubectl create namespace observability
```
```bash
kubectl apply -f https://github.com/jaegertracing/jaeger-operator/releases/download/v1.49.0/jaeger-operator.yaml
```
## Instrument Node.js (Service A)
```bash
Copy code
const { NodeTracerProvider } = require('@opentelemetry/sdk-trace-node');
const { SimpleSpanProcessor } = require('@opentelemetry/sdk-trace-base');
const { JaegerExporter } = require('@opentelemetry/exporter-jaeger');

const provider = new NodeTracerProvider();
const exporter = new JaegerExporter({
  serviceName: 'service-a',
  endpoint: 'http://jaeger-collector:14268/api/traces',
});
```
provider.addSpanProcessor(new SimpleSpanProcessor(exporter));
provider.register();
## 8️⃣ Test Alerts
Crash the container more than 2 times:

```bash
Copy code
<LOAD_BALANCER_DNS>/crash
👉 Expect email alert when restart count ≥ 3.
```
✅ Summary
You now have a complete observability pipeline:

- Metrics → Prometheus & custom instrumentation.

- Logging → EFK stack.

- Tracing → Jaeger.

- Alerting → Alertmanager.

- Deployment → Kubernetes.

- This setup provides visibility, troubleshooting, and performance monitoring across microservices.