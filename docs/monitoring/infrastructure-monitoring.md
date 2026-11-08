cd ~/infrastructure-lab

cat > docs/monitoring/infrastructure-monitoring.md <<'EOF'
# Infrastructure Monitoring Lab

## 1. Project Overview

Built a containerized infrastructure monitoring environment using Prometheus, Grafana, and Node Exporter.

The purpose of this lab is to collect system metrics, query time-series data, and visualize infrastructure performance through a Grafana dashboard.

## 2. Technology Stack

- Docker Desktop
- Docker Compose
- Prometheus 3.5.0
- Grafana 12.1.0
- Node Exporter 1.9.1
- Ubuntu 24.04 on WSL2

## 3. Architecture

Node Exporter exposes system metrics on port 9100.

Prometheus scrapes Node Exporter every 15 seconds and stores time-series metrics.

Grafana connects to Prometheus as its data source and visualizes the collected metrics.

| Service | Port | Purpose |
|---|---:|---|
| Prometheus | 9090 | Metrics collection and querying |
| Node Exporter | 9100 | System metrics |
| Grafana | 3000 | Monitoring dashboards |

All published ports are bound to 127.0.0.1 for local access.

## 4. Monitoring Dashboard

Created an Infrastructure Monitoring Dashboard with five panels:

1. CPU Utilization (%)
2. Memory Utilization (%)
3. Disk Utilization (%)
4. Network Receive Traffic (bytes/sec)
5. Network Transmit Traffic (bytes/sec)

## 5. Prometheus Configuration

- Scrape interval: 15 seconds
- Prometheus retention: 7 days
- Targets:
  - localhost:9090
  - node-exporter:9100

Both targets were verified as UP through the Prometheus targets API.

## 6. Example PromQL Queries

### CPU Utilization

    100 * (1 - avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])))

### Memory Utilization

    100 * (1 - node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)

### Disk Utilization

    100 * (
      1 -
      node_filesystem_avail_bytes{mountpoint="/",fstype!="rootfs"}
      /
      node_filesystem_size_bytes{mountpoint="/",fstype!="rootfs"}
    )

### Network Receive Traffic

    sum by (device) (
      rate(node_network_receive_bytes_total{device!="lo"}[5m])
    )

### Network Transmit Traffic

    sum by (device) (
      rate(node_network_transmit_bytes_total{device!="lo"}[5m])
    )

## 7. Validation and Troubleshooting

Verified the monitoring services using:

- `docker compose ps`
- `docker compose config --quiet`
- Prometheus readiness endpoint
- Grafana health API
- Node Exporter metrics endpoint
- Prometheus targets API

Resolved a Node Exporter filesystem mount issue by adjusting the Docker volume mount configuration for the WSL2 environment.

## 8. Security

- Service ports are bound to localhost.
- Grafana admin password is stored in a local `.env` file.
- `.env` is excluded from Git.
- `.env.example` provides a safe configuration template.
- Grafana user self-registration is disabled.

## 9. Environment Limitations

This lab runs on Windows using Docker Desktop and WSL2.

Node Exporter runs inside the Docker Linux environment. Its metrics should not be interpreted as direct measurements of the physical Windows host's CPU, memory, or disks.

This is a local learning environment, not a production deployment.

## 10. Skills Demonstrated

- Containerized service deployment
- Docker Compose configuration
- Infrastructure monitoring concepts
- Prometheus metric collection
- PromQL queries
- Grafana dashboard creation
- Linux troubleshooting
- Environment variable and credential management
EOF
