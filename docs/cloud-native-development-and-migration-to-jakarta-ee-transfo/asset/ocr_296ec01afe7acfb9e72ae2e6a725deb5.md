# Showing the complete Docker file

The complete Docker file will look as follows. If you followed all the steps in this chapter, you will have altered the Cargo Tracker application so that it includes metrics. Create a Prometheus and Grafana service that will load their configuration from the cargo-tracker application:

```
version: "3.9"
services:
  cargo-tracker-app:
    build: .
    ports:
      - "4848:4848"
      - "8080:8080"
      - "8181:8181"
      - "9009:9009"
  prometheus:
    image: bitnami/prometheus:latest
    volumes:
      - ./prometheus/prometheus.yml:/opt/bitnami/prometheus/conf/prometheus.yml
    ports:
      - "9090:9090"
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    volumes:
      - ./grafana/provisioning:/etc/grafana/provisioning
      - ./grafana/mydashboards:/var/lib/grafana/dashboards
```

Now, when you run docker compose-up, three services are going to be created: cargo-tracker, Prometheus, and a Grafana service. When you visit your Grafana instance at http://localhost:3000 and log in using admin as the username and admin as the password, you will see the following dashboard. These metrics are gathered from the Prometheus instance, which pulls them from the cargo-tracker application: