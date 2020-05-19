# Monitoring with Prometheus and Grafana

Prometheus is an open-source systems monitoring and alerting toolkit. It allows us to scrape metrics from and endpoint and publish it to a visualization tool like Grafana.

In this tutorial, we will use Prometheus with the following components:

- **[Grafana](https://grafana.com/grafana/)**: brings data together in a way that is both efficient and organized. It allows users to better understand the metrics of their data through queries, informative visualizations and alerts.

- **[node exporter](https://github.com/prometheus/node_exporter)**: Exports hardware and OS metrics.

- **[cAdvisor](https://github.com/google/cadvisor)**: provides container users an understanding of the resource usage and performance characteristics of their running containers.

- **[pushgateway](https://github.com/prometheus/pushgateway)**: allows ephemeral and batch jobs to expose their metrics to Prometheus. 

## Setting up Prometheus in the monitoring server

To set up Prometheus, we need to define two files: `1) 01-docker-compose.yml`, and 2) `01-prometheus.yml`.

01-docker-compose.yml:
```
services:

  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - 9090:9090
    volumes:
      - "./prometheus.yml:/etc/prometheus/prometheus.yml:ro"
    healthcheck:
      test: ["CMD", "ls", "/bin/prometheus"]
      interval: 35s
      timeout: 10s
      retries: 6
    restart: unless-stopped
```
prometheus.yml:
```
global:
  scrape_interval:     15s
  evaluation_interval: 15s
  
scrape_configs:

  - job_name: 'prometheus'
    static_configs:
    - targets: ['__SERVER_IP_ADDRESS__:9090']
```
To spin up Prometheus, simply start the containers via docker-compose: `docker-compose up -d`

To check whether it is properly set up, fire up a web browser and go to: `__SERVER_IP_ADDRESS__:9090`

Finally, try scraping some metrics by typing the following query in the query box: `promhttp_metric_handler_requests_total{code="200"}`
