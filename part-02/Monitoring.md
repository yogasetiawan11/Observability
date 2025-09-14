# Different Monitoring and Metrics
Monitoring is the process of keeping an eye on these metrics over time to understand what’s normal, identify changes, and detect problems. Monitoring is like watching step count daily to showcase what's working and not or checking heart rate to make sure it in healthy range.

Metrics play an important role in understanding why your application is working in a certain way. Let's assume you are running a web application and feels it is slow. To understand what is happening with your application, you will need some information. For example, when the number of requests is high, the application may become slow.  you can determine the cause and increase the number of servers to handle the load.

# Prometheus
is an open-source systems monitoring and alerting toolkit. Prometheus collects and stores its metrics as time series data (tsdb), i.e. metrics information is stored with the timestamp at which it was recorded, alongside optional key-value pairs called labels.
It can be configure and set up in bare metal-server or container environment like kubernetes.
somebody can send the Metrics to Prometheus or Prometheus can scrapt/pull the Metrics from various resources there is something called as exporter, kubestate metrics, metrics end point of application. so Prometheus pulls various metrics from various resources like AWS, Kubernetes, and represent them in a dashboard format. 
# Prometheus Architecture
<img width="1351" height="811" alt="Image" src="https://github.com/user-attachments/assets/91c4737d-6b0e-4ab5-95da-98337448e8aa" />

source image: prometheus official

# Prometheus server
- At the center is the ``Prometheus server``, it is responsible for scraping matrics from various configured target storing them into Time series database (TSDB) and serving query through HTTP server:
There are 3 componens of Prometheus server:

- Retrieval: The server uses a pull model to get data. It periodically "scrapes" metrics from HTTP endpoints on designated targets. The diagram shows it pulling metrics from Jobs/exporters, which are applications or services configured to expose metrics.

- TSDB (Time-Series Database): This is Prometheus's native, high-performance database. It stores the scraped metrics as time-series data locally on the server's disk (HDD/SSD).

- HTTP server: This component handles queries for the stored data. It's the engine behind the Prometheus web UI and also serves data to external tools like Grafana and other API clients.

# Service Discovery
Prometheus needs to know what to monitor. Service discovery automatically finds targets (services, nodes, pods) instead of hardcoding them.
Kubernetes: Discovers pods and services dynamically.
- file_sd: Reads static files for targets if service discovery isn’t available.

# Jobs / Exporters
Prometheus doesn’t directly know how to get metrics from every app. Exporters act as translators that expose metrics in a format Prometheus understands.
Examples:
- Node Exporter → System metrics (CPU, memory, disk).
- cAdvisor → Container metrics.
- Custom Exporters → App-specific metrics.

# Pushgateway
Normally Prometheus pulls metrics. But for short-lived jobs (batch jobs, CI/CD tasks), metrics might disappear before Prometheus scrapes them.
These jobs push metrics to Pushgateway, and Prometheus pulls them from there.

# Storage

Data is stored locally on HDD/SSD inside Prometheus. You can integrate with long-term storage systems (like Thanos or Cortex) for historical data.

# Alerting

Alertmanager receives alerts from Prometheus when rules are triggered.

It handles:

- De-duplication (avoid sending the same alert multiple times).
- Grouping (combine related alerts).
- Routing (send alerts to the right channel).

Notification channels:
Email, PagerDuty, Slack, Others 

# Visualization
- Prometheus Web UI: Simple, mostly for queries and debugging.
- Grafana: Rich dashboards with graphs, trends, and visualizations.
- API Clients: Other apps can query Prometheus data via HTTP API.

