For security and analysis, logs should be sent to a remote centralized syslog server. Ideally, this server will be where the log introspection, analytics, and cataloging will be done, and it should be hosted on a server with appropriate CPU and memory to support these workloads. Log introspection would be done on the content of the log entries as these logs can contain some, or all of the following (example based on the preceding log):

+   - Severity levels (`INFO`)
+   - The server that sent the log (`10.0.3.15`)
+   - The service that sent the log (`nova.osapi_compute.wsgi.server`)
+   - Metadata such as `tenant_id` and `request_id` (`6f0117ddd81b4dc78a8f4c4ed5b04f5` and `req-b5ff3321-19cc-4ce8-af9c-0ed59ae21ac7`)
+   - Error codes from the service, and even performance data such as HTTP response times (involving limited usability for asynchronous connections due to the segmented nature of create requests and API interactions)

Here is what an error would look like:

```
nova-scheduler.log:2016-07-08 07:26:48.687 1159 ERROR
oslo.messaging._drivers.impl_rabbit [req-
c419cec6-9847-4692-8c61-733524097546 -- -
--] AMQP server on 10.0.3.15:5672 is unreachable: [Errno 111] ECONNREFUSED.
Trying
again in 4 seconds
```

As we can see above, this is a structured log file that can be parsed by many different tools to extract the severity, service, timestamp, and other relevant metadata. Furthermore, this data could be searched with open source analysis tools such as **ELK (ElasticSearch, Logstash, and Kibana)**. Basic information about this industry standard data mining platform can be found at `https://www.elastic.co/webinars/introduction-elk-stack`. This page also includes a webinar to get you started.