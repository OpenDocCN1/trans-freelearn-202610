In my case, accessing the `http://chapt-farga-65sza285ppo1-1511829492.eu-central-1.elb.amazonaws.com/metrics` URL gave me the following output:

```
...
# HELP flask_exporter_info Information about the Prometheus Flask exporter
# TYPE flask_exporter_info gauge
flask_exporter_info{version="0.22.3"} 1.0
...
# HELP flask_http_request_duration_seconds Flask HTTP request duration in seconds
# TYPE flask_http_request_duration_seconds histogram
flask_http_request_duration_seconds_bucket{le="0.005",method="GET",path="/",status="200"} 309.0
...
```

The lines starting with `# HELP` describe some information about the following lines and how to interpret them, while the ones starting with `# TYPE` show the type of metric (histogram/gauge) and the related values that can then be visualized by tools such as Grafana.

# Visualizing data with Amazon Grafana

So far, we have validated that the application is exposing metrics data on the `/metrics` endpoint and that the OTEL collector is scraping this endpoint and further publishing it in the Prometheus workspace we provisioned in our CDK stack. The final tool in our architecture was Grafana. As described in *Figure 8.7*, you can select certain metrics to be shown on the Grafana dashboard. I selected a few of those and got the following screenshot (*Figure 8.9*), which highlights the network request spikes that lasted for some time while the `curl` command was running in the background: