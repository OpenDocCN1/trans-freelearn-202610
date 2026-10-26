# Modifying the Flask application code

There are two main changes that we need. First, we must add logging capabilities to the code base, and second, we must allow the code to publish Prometheus metrics that can be consumed by the OpenTelemetry collector that we will deploy later.

Just like other reusable Python modules, Prometheus offers a variety of ready-to-use integrations for commonly used application frameworks. Flask is no different. The two libraries that are relevant for our use case are prometheus-client and prometheus-flask-exporter. It is very simple to expose Prometheus metrics by simply importing these libraries into our code, as we will see shortly. For logging requirements, we will use the standard logging library.

# Enabling Prometheus metrics in your application

The Flask application code sits in the app.py file. The complete file is available for your reference at https://github.com/PacktPublishing/AWS-DevOps-Simplified/blob/main/chapter-8/chapter-8-flask-app/app.py. Soon after we create an instance of the PrometheusMetrics class, it hooks onto the Flask framework under the hood, thereby directly capturing all the relevant metrics. Whenever a GET or POST request comes in, Prometheus gets to know about it. This avoids any extra effort from the developer’s side unless they want to define and export additional custom metrics based on the application’s business use case:

```
...
from prometheus_flask_exporter import PrometheusMetrics
...
app = Flask(__name__)
metrics = PrometheusMetrics(app)
...
```

To include these modules in our build workflow, we should also add them to the requirements.txt file so that they are picked up when a Docker image is created. As a best practice, we should lock them down with exact version identifiers:

```
...
...
Werkzeug==2.2.3
zipp==3.14.0
prometheus-client==0.16.0
prometheus-flask-exporter==0.22.3
```

Deploying the application at this point will automatically enable the /metrics endpoint, which exports Prometheus metrics extracted from the Flask application.