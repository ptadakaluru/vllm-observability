🚀 Local LLM Observability Stack with vLLM, Prometheus & Grafana

This project demonstrates how to run a local Large Language Model (LLM) using vLLM and monitor its runtime metrics using Prometheus and Grafana.

The setup combines:

vLLM for high-performance LLM inference

Prometheus for metrics scraping

Grafana for visualization and observability

Google Colab notebooks to bootstrap and run the local LLM environment

<img width="272" height="563" alt="image" src="https://github.com/user-attachments/assets/99b023dc-d177-4020-94d0-63f41b845e71" />

| Path                                                            | Description                                                                                                    |
| --------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `VLLM-OBSERVABILITY/`                                           | Root directory containing the local LLM runtime and observability stack.                                       |
| `monitoring/`                                                   | Holds all Prometheus and Grafana configuration required for LLM observability.                                 |
| `monitoring/prometheus.yml`                                     | Prometheus configuration defining scrape jobs and vLLM metrics targets.                                        |
| `monitoring/kubectl.exe`                                        | Optional Kubernetes CLI binary for interacting with Kubernetes clusters (not required for Docker-based setup). |
| `monitoring/grafana/`                                           | Contains Grafana provisioning configuration and dashboards.                                                    |
| `monitoring/grafana/dashboards/`                                | Directory for Grafana dashboard JSON files.                                                                    |
| `monitoring/grafana/dashboards/vllm-observability.json`         | Prebuilt Grafana dashboard visualizing vLLM latency, throughput, and resource metrics.                         |
| `monitoring/grafana/provisioning/`                              | Grafana provisioning configuration loaded automatically at startup.                                            |
| `monitoring/grafana/provisioning/datasources/`                  | Directory for Grafana datasource definitions.                                                                  |
| `monitoring/grafana/provisioning/datasources/prometheus-ds.yml` | Automatically registers Prometheus as the default Grafana datasource.                                          |
| `monitoring/grafana/provisioning/dashboards/`                   | Configuration directory for dashboard auto-discovery.                                                          |
| `monitoring/grafana/provisioning/dashboards/dashboards.yml`     | Instructs Grafana to load dashboard JSON files from disk on startup.                                           |
| `vLLM-gemma/`                                                   | Contains notebooks for running the Gemma model using vLLM.                                                     |
| `vLLM-gemma/VLLM_Gemma2.ipynb`                                  | Google Colab notebook that launches the Gemma LLM with vLLM and exposes inference and metrics endpoints.       |
| `vLLM-qwen/`                                                    | Contains notebooks for running Qwen models using vLLM.                                                         |
| `vLLM-qwen/VLLM_Qwen2.5.ipynb`                                  | Google Colab notebook that launches the Qwen LLM with vLLM and exposes inference and metrics endpoints.        |


Prerequisites

Docker Desktop running

Ports available:

9090 → Prometheus

3000 → Grafana

You are in the repo root (where monitoring/ exists)

Step 1: Create Docker network (one time)
docker network create monitoring


(If it already exists, Docker will just warn — that’s fine.)

Step 2: Start Prometheus using your existing prometheus.yml

From the repo root:

docker run -d --name prometheus \
  --network monitoring \
  -p 9090:9090 \
  -v "$PWD/monitoring/prometheus.yml:/etc/prometheus/prometheus.yml:ro" \
  prom/prometheus:latest

Verify Prometheus

Open: http://localhost:9090

Go to Status → Targets

You should see:

ngrok-metrics-gemma2b

ngrok-metrics-qwen2.5b

Both should be UP

Quick sanity query:

count by (vllm_model) (up)

Step 3: Start Grafana using your existing provisioning + dashboards

⚠️ Do not change any files — we will mount exactly what you already have.

From the repo root:

docker run -d --name grafana \
  --network monitoring \
  -p 3000:3000 \
  -e GF_SECURITY_ADMIN_USER=admin \
  -e GF_SECURITY_ADMIN_PASSWORD=admin \
  -v "$PWD/monitoring/grafana/provisioning:/etc/grafana/provisioning:ro" \
  -v "$PWD/monitoring/grafana/dashboards:/var/lib/grafana/dashboards:ro" \
  grafana/grafana:latest

Step 4: Verify Grafana (no UI configuration required)

Open: http://localhost:3000

Login: admin / admin

Navigate to:

Dashboards → vLLM → vLLM Observability (Model Comparison)

✔ Datasource is already configured
✔ Dashboard auto-loaded
✔ Model comparison works out of the box

What each file is doing (based on your repo)
monitoring/prometheus.yml

Scrapes multiple vLLM endpoints

Adds:

labels:
  vllm_model: gemma2b


This is the key enabler for model comparison

grafana/provisioning/datasources/prometheus-ds.yml

Creates Prometheus datasource automatically

Uses Docker network name:

http://prometheus:9090


Sets a fixed UID so dashboards bind cleanly

grafana/provisioning/dashboards/dashboards.yml

Tells Grafana:

Load dashboards from /var/lib/grafana/dashboards

Place them in Grafana folder: vLLM

grafana/dashboards/vllm-observability.json

The actual dashboard

Uses:

vllm_model → endpoint/model comparison

model_name → internal vLLM model labels

Panels already compare:

HTTP latency p95

Prompt & generation tokens/sec

TTFT p95

KV cache usage

Inter-token latency

Success/failure reasons

Common operations (based on your setup)
Restart after config change
docker restart prometheus
docker restart grafana

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
