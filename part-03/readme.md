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

## Example

```container_cpu_usage_seconds_total```
Return all time series with the metric ```container_cpu_usage_seconds_total```
```container_cpu_usage_seconds_total```{namespace="kube-system",pod=~"kube-proxy.*"}
Return all time series with the metric ```container_cpu_usage_seconds_total``` and the given namespace and pod labels.
```container_cpu_usage_seconds_total```{namespace="kube-system",pod=~"kube-proxy.*"}[5m]
Return a whole range of time (in this case 5 minutes up to the query time) for the same vector, making it a range vector.

# Aggregation & Functions in PromQL
Aggregation in PromQL allows you to combine multiple time series into a single one, based on certain labels.

1. Sum Up All CPU Usage:

- ```sum(rate(node_cpu_seconds_total[5m]))```
This query aggregates the CPU usage across all nodes.
Average Memory Usage per Namespace:

- ```avg(container_memory_usage_bytes)``` by ```(namespace)```
This query provides the average memory usage grouped by namespace.

2. rate() Function:

The rate() function calculates the per-second average rate of increase of the time series in a specified range.
- ```rate(container_cpu_usage_seconds_total[5m])```
This calculates the rate of CPU usage over 5 minutes.

3. ```increase() Function:```

The increase() function returns the increase in a counter over a specified time range.
increase(kube_pod_container_status_restarts_total[1h])
This gives the total increase in container restarts over the last hour.

4. ```histogram_quantile() Function:```

The histogram_quantile() function calculates quantiles (e.g., 95th percentile) from histogram data.
- ```histogram_quantile(0.95, sum(rate(apiserver_request_duration_seconds_bucket[5m]))by (le))```
This calculates the 95th percentile of Kubernetes API request durations.