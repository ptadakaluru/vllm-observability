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
