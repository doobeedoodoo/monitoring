# monitoring

Prometheus is an open-source systems monitoring and alerting toolkit. In this tutorial, we will integrate Prometheus with the following components:

Grafana
Brings data together in a way that is both efficient and organized. It allows users to better understand the metrics of their data through queries, informative visualizations and alerts.

- node exporter
Exports hardware and OS metrics.
https://github.com/prometheus/node_exporter

- cAdvisor
Provides container users an understanding of the resource usage and performance characteristics of their running containers. It is a running daemon that collects, aggregates, processes, and exports information about running containers.
https://github.com/google/cadvisor

- pushgateway
Allows ephemeral and batch jobs to expose their metrics to Prometheus. Since these kinds of jobs may not exist long enough to be scraped, they can instead push their metrics to a Pushgateway. The Pushgateway then exposes these metrics to Prometheus.
https://github.com/prometheus/pushgateway

Setting up Prometheus in the monitoring server.

To set up Prometheus, we need two files: 1) docker-compose.yml, and 2) prometheus.yml.

docker-compose.yml

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
	
prometheus.yml

scrape_configs:

  - job_name: 'prometheus'
    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.
    static_configs:
    - targets: ['__SERVER_IP_ADDRESS__:9090']
	
To spin up Prometheus, simply start the containers via docker-compose:

docker-compose up -d

To check whether it is properly set up, go to:

__SERVER_IP_ADDRESS__:9090


