# grafana-prometheus-loki

Local monitoring stack with Grafana, Prometheus, and Loki using Docker Compose.

## Services

- Grafana: http://localhost:3000
- Prometheus: http://localhost:9090
- Loki: http://localhost:3100

Prometheus is protected with basic auth:

- user: admin
- password: admin

## Quick start

1. Start the stack:

```bash
docker compose up -d
```

2. Open Grafana at http://localhost:3000.

3. Log in with the default credentials (can be overridden by env vars):

- user: admin
- password: admin

4. Stop the stack when done:

```bash
docker compose down
```

## Configuration

- Prometheus config: [prometheus/prometheus.yml](prometheus/prometheus.yml)
	- Scrapes Prometheus and Loki every 5 seconds.
- Prometheus web auth config: [prometheus/web.yml](prometheus/web.yml)
	- Enables HTTP basic auth for Prometheus web endpoints.
- Loki config: [loki/loki-config.yml](loki/loki-config.yml)
	- Stores data on a local filesystem-backed volume.
- Grafana provisioning: [grafana/provisioning](grafana/provisioning)
	- Create datasources and dashboards on startup (optional).

## Environment variables

Grafana admin credentials can be set via:

```bash
GF_SECURITY_ADMIN_USER=admin
GF_SECURITY_ADMIN_PASSWORD=admin
```

## Volumes

- grafana-storage: Grafana data
- prometheus-storage: Prometheus TSDB
- loki-storage: Loki chunks and indexes

## Data retention

- Prometheus retention: 30 days (`--storage.tsdb.retention.time=30d`)
- Loki retention: 30 days (`limits_config.retention_period: 30d` with compactor retention enabled)

## Notes

- All services run on the shared `monitoring` Docker network.
- Prometheus remote write receiver is enabled for testing.

