# Metrics
- Metrics in Prometheus are the core data objects that represent measurements collected from monitored systems.

Metric basically give you historical data of event this event can be CPU, Memory, Disk, Http request.

# Label
Labels are key-value pairs that allow you to differentiate between dimensions of a metric, such as different services, instances, or endpoints.

## Example
- ``container_cpu_usage_seconds_total{namespace="kube-system", endpoint="https-metrics"}``
- ``container_cpu_usage_seconds_total`` is the metric.
- ``{namespace="kube-system", endpoint="https-metrics"}`` are the labels.

# PromQL
- PromQL (Prometheus Query Language) is a powerful and flexible query language used to query data from Prometheus.

It allows you to retrieve and manipulate time series data, perform mathematical operations, aggregate data, and much more.