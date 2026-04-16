# 🚀 Observability Stack Project (Prometheus + Grafana + Alertmanager)

## 📌 Overview

This project demonstrates a **production-style observability stack** built using:

- Prometheus (metrics collection)
- Grafana (visualization)
- Alertmanager (alerting)
- Node Exporter (system metrics)
- Python Flask app (instrumented service)

It simulates a real-world DevSecOps setup where a microservice is monitored, visualized, and protected with alerting.

## 🎯 Objectives

- Build a complete observability pipeline
- Instrument an application with Prometheus metrics
- Visualize system and application data in Grafana
- Configure alerting for reliability issues
- Apply DevSecOps best practices

## 🏗️ Architecture

```txt
                +------------------+
                |   Flask App      |
                |  (/metrics)      |
                +--------+---------+
                         |
                         v
                +------------------+
                |   Prometheus     |
                |  (Scraping)      |
                +--------+---------+
                         |
          +--------------+-------------+
          |                            |
          v                            v
+------------------+         +------------------+
|   Grafana        |         | Alertmanager     |
| (Dashboards)     |         | (Notifications)  |
+------------------+         +------------------+

         ^
         |
+------------------+
| Node Exporter    |
| (Host Metrics)   |
+------------------+
```

## 📁 Project Structure

```txt
observability-project/
├── docker-compose.yml
├── prometheus/
│   ├── prometheus.yml
│   └── alerts.yml
├── alertmanager/
│   └── alertmanager.yml
├── app/
│   └── app.py
```

## ⚙️ Prerequisites

- Docker
- Docker Compose

## 🚀 Getting Started

1. Clone the repository

    ```sh
    git clone https://github.com/your-username/observability-project.git
    
    cd observability-project
    ```

2. Start the stack

    ```sh
    docker-compose up -d 
    ```

3. Access services

    | Service | URL |
    | ------- | --- |
    | Prometheus | http://localhost:9090 |
    | Grafana | http://localhost:3000 |
    | Alertmanager | http://localhost:9093 |
    | App | http://localhost:5000 |

## 📊 Metrics Exposed

The application exposes:

- http_requests_total → request count
- http_request_duration_seconds → latency histogram

Example metric:

http_requests_total{method="GET",endpoint="/",status="200"}

## 📈 Grafana Setup

### Login

```txt
username: admin
password: admin
```

### Add Prometheus Data Source

- URL: http://prometheus:9090

## 📉 Example Queries

### Request Rate

rate(http_requests_total[1m])

### Error Rate

sum(rate(http_requests_total{status="500"}[1m])) 
/
sum(rate(http_requests_total[1m]))

### Latency (95th percentile)

histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[1m]))

## 🚨 Alerting

Defined in:

prometheus/alerts.yml

### Alerts Included

#### High Error Rate

- Trigger: error rate > threshold
- Severity: critical

#### High Latency

- Trigger: 95th percentile latency too high
- Severity: warning

## 🧪 Testing the System

### Generate normal traffic

curl localhost:5000/

### Generate errors

curl localhost:5000/error

### Trigger alerts

Run multiple error requests:

for i in {1..50}; do curl localhost:5000/error; done

## 🔐 Security Considerations

- Do not expose /metrics publicly
- Use authentication for Grafana and Prometheus
- Place services behind a reverse proxy (NGINX)
- Avoid sensitive data in labels
- Use HTTPS in production

## ⚡ Best Practices

### Metrics

- Use consistent naming:
<service>_<metric>_<unit>

- Avoid high-cardinality labels

### Alerting

- Alert on symptoms, not causes
- Use SLO-based thresholds
- Include meaningful descriptions

### Performance

- Use recording rules for expensive queries
- Tune scrape intervals carefully

## 📦 Future Improvements

- Kubernetes deployment (Helm)
- Thanos or Cortex for scaling
- Loki for logs
- Tempo for tracing
- CI/CD integration

## 🧠 What is implemented

- End-to-end observability design
- Prometheus configuration and querying
- Grafana dashboard creation
- Alerting strategies
- DevSecOps monitoring practices
