# Observability
## Overview
Observability is the ability to understand the internal state of a system by analyzing the data it produces. In modern distributed and cloud-native systems, observability is essential for ensuring reliability, performance, and security.

Observability is commonly built on three core pillars:
- Metrics – Numerical data representing system behavior over time

- Logs – Detailed event records describing what happened inside the system

- Traces – End-to-end tracking of requests as they flow through distributed services

This repository documents a complete observability stack using:
- Prometheus for metrics collection and alerting

- Grafana for visualization and dashboards

- ELK / EFK Stack for log aggregation and analysis

- Jaeger for distributed tracing

Observability Architecture

A typical observability architecture in a cloud-native environment looks like this:

- Applications and infrastructure emit metrics, logs, and traces

- Data is collected by specialized agents or exporters

- Centralized backends store and index the data

- Visualization and analysis tools provide insights for engineers and operators

- Each tool in this stack focuses on a specific pillar while integrating with others.


# Metrics: Prometheus. What is Prometheus? 

Prometheus is an open-source monitoring and alerting system designed for reliability and scalability. It is widely used in Kubernetes and cloud-native environments.

## Key Characteristics

- Pull-based metrics collection

- Time-series database (TSDB)

- Powerful query language: PromQL

- Native support for Kubernetes and cloud platforms

- Built-in alerting via Alertmanager

## Core Components

- Prometheus Server – Scrapes and stores metrics

- Exporters – Expose metrics from applications and systems (e.g., Node Exporter)

- Pushgateway – Handles short-lived jobs

- Alertmanager – Manages alerts and notifications

## Use Cases

- CPU, memory, disk, and network monitoring

- Application performance monitoring

- Service-level indicators (SLI) and service-level objectives (SLO)

- Alerting on threshold breaches and anomalies

# Visualization: Grafana. What is Grafana?

Grafana is an open-source visualization platform used to create dashboards and analyze observability data.

## Key Features

- Supports multiple data sources (Prometheus, Elasticsearch, Loki, Jaeger, etc.)

- Interactive dashboards with real-time updates

- Alerting and notification support

- Role-based access control

## Grafana with Prometheus

- Grafana is commonly used as the visualization layer for Prometheus metrics:

Dashboards display system and application health

PromQL queries power panels and charts

Alerts can be defined at the dashboard or data source level

## Use Cases

- Infrastructure dashboards

- Application performance dashboards

- Business and operational metrics visualization

# Logs: (ELK / EFK Stack). What is ELK / EFK?

- The ELK and EFK stacks are popular solutions for centralized log management.

- ELK: Elasticsearch, Logstash, Kibana

- EFK: Elasticsearch, Fluentd / Fluent Bit, Kibana

- The main difference is the log collector used.


# Components of Elasticsearch

- Distributed search and analytics engine

- Stores and indexes log data

- Enables fast querying and aggregation

- ## Logstash (ELK)

- Log processing and transformation pipeline

- Parses, enriches, and forwards logs

- Fluentd / Fluent Bit (EFK)

- Lightweight log collectors

- Commonly used in Kubernetes environments

- Efficient and resource-friendly

- ## Kibana

- Visualization and exploration UI for logs

- Supports search, filtering, and dashboards

## Use Cases

- Application and system log aggregation

- Debugging runtime errors

- Security and audit logging

- Root cause analysis


# Traces: Jaeger. What is Jaeger?

Jaeger is an open-source distributed tracing system originally developed by Uber. It helps track requests as they propagate through microservices.

## Key Concepts

- Trace – A complete journey of a request

- Span – A single unit of work within a trace

- Context Propagation – Passing trace identifiers between services

## Core Components

- Jaeger Client – Instrumentation libraries

- Jaeger Agent – Collects spans locally

- Jaeger Collector – Processes and stores traces

- Jaeger Query UI – Visualization and analysis

# Use Cases

- Identifying performance bottlenecks

- Understanding service dependencies

- Debugging latency issues

- Analyzing request flows in microservices
