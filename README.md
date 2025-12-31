🚀 Local LLM Observability Stack with vLLM, Prometheus & Grafana

This project demonstrates how to run a local Large Language Model (LLM) using vLLM and monitor its runtime metrics using Prometheus and Grafana.

The setup combines:

vLLM for high-performance LLM inference

Prometheus for metrics scraping

Grafana for visualization and observability

Google Colab notebooks to bootstrap and run the local LLM environment

🧩 Architecture Overview
┌──────────────┐
│  vLLM Server │
│  (LLM API)   │
│  /metrics    │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Prometheus   │
│  :9090       │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Grafana      │
│  :3000       │
└──────────────┘

📁 Repository Structure
monitoring/
├── prometheus.yml
└── grafana/
    └── provisioning/
        └── datasources/
            └── prometheus-ds.yml


Additional files:

Google Colab notebooks to start and expose the local LLM using vLLM

🧠 Local LLM Setup (vLLM)

This repository includes Google Colab notebooks that:

Install vllm

Load an open-source LLM

Expose an HTTP API compatible with OpenAI-style requests

Enable a /metrics endpoint for Prometheus scraping

📌 These notebooks are intended to run in Google Colab (GPU-backed) and act as the LLM backend for this observability stack.

📊 Prometheus Setup
1️⃣ Prometheus Configuration

Create monitoring/prometheus.yml:

global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "vllm"
    static_configs:
      - targets:
          - "<vllm-host>:<metrics-port>"


Replace <vllm-host> and <metrics-port> with your vLLM metrics endpoint.

2️⃣ Run Prometheus via Docker
docker run -d \
  --name prometheus \
  --network monitoring \
  -p 9090:9090 \
  -v "$(pwd)/monitoring/prometheus.yml:/etc/prometheus/prometheus.yml:ro" \
  prom/prometheus:latest


Verify:

Prometheus UI: http://localhost:9090

Status → Targets

📈 Grafana Setup
1️⃣ Grafana Datasource Provisioning

Create:

monitoring/grafana/provisioning/datasources/prometheus-ds.yml


Contents:

apiVersion: 1

datasources:
  - name: Prometheus
    type: prometheus
    access: proxy
    url: http://prometheus:9090
    isDefault: true
    editable: false

2️⃣ Run Grafana via Docker
docker run -d \
  --name grafana \
  --network monitoring \
  -p 3000:3000 \
  -e GF_SECURITY_ADMIN_USER=admin \
  -e GF_SECURITY_ADMIN_PASSWORD=admin \
  -v "$(pwd)/monitoring/grafana/provisioning:/etc/grafana/provisioning:ro" \
  -v "$(pwd)/monitoring/grafana/dashboards:/var/lib/grafana/dashboards:ro" \
  grafana/grafana:latest


Access Grafana:

URL: http://localhost:3000

Username: admin

Password: admin

Prometheus is automatically configured as the default datasource.

📊 What You Can Monitor

Typical metrics exposed by vLLM include:

Request latency

Tokens generated per second

Active requests

Queue depth

CPU / GPU utilization

Memory usage

These metrics can be visualized using Grafana dashboards or used for alerting.

🛠️ Prerequisites

Docker

Docker network named monitoring

docker network create monitoring


Google Colab (for LLM runtime)

Basic familiarity with Prometheus and Grafana

🔮 Future Enhancements

Prebuilt Grafana dashboards for vLLM

Alerting rules (Alertmanager)

Kubernetes / Helm deployment

Multi-model observability

Secure metrics endpoints (TLS / auth)

Integration with AI agents or MCP servers

📜 License

This project is intended for learning and experimentation.
You are free to adapt and extend it for your own use.