# Goal
Quickly get a PromQL interface in your hand. 

Allows you to view your local machines metrics

# Components
## Node Exporter - expose metrics
exposes a wide variety of hardware- and kernel-releated metrics

`docker run -p 9100:9100 --rm --name node-exporter bitnami/node-exporter:1.7.0`

List the metrics:
```bash
# nsenter, if you didn't forward port:
nsenter --target <pid-of-node-exporter> --net bash`
curl localhost:9100/metrics
```

## Prometheus - collect metrics
TSDB will scrape the metrics.

`docker run -p 9090:9090 --rm --name prometheus -v prometheus-config:/etc/prometheus/ prom/prometheus:v2.48.1`

Prometheus needs t be configured to scrape the metrics, pass it `--conifg.file./promethues.yml`:
```yaml
# prometheus.yml:
global:
  scrape_interval: 15s

scrape_configs:
- job_name: node
  static_configs:
  - targets: ['localhost:9100']
```

Check if it ingested some metrics:
`curl localhost:9090/`

## Prometheus UI - visualize + query metrics
Prometheus ships with a nice UI after deploying it visit it at:
`localhost:9090/graph`