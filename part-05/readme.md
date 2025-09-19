# Logging 
Logging is crucial for any distribution system, especially for Kubernetes, to monitor application, detect issues, and ensure everything works smothly in Microservices environment. 

# Logging can Help you with
- ``Debugging``: Logs provide crucial information when debugging issues in application.
- ``Security``: Logs help in detecting unauthorized access or malicious activities.
- ``Performance monitoring``: Analyzing can help help to identify the bottlenecks 
- ``Auditing``:  Logs serve as an audit trail, showing what actions were taken and by whom.

# Tools for monitoring Logs
- Elastic FluenD Kibana
- Elastic Fluenbit Logstash Kibana
- Elastic Loggstash Kibana
- Promtail + Loki + Grafana

# EFK Stack (Elasticsearch, Fluentbit, Kibana)
- EFK is a popular logging stack used to collect, store, and analyze logs in Kubernetes.
Elasticsearch: Stores and indexes log data for easy retrieval.
- Fluentbit: A lightweight log forwarder that collects logs from different sources and sends them to Elasticsearch.
- Kibana: A visualization tool that allows users to explore and analyze logs stored in Elasticsearch.
