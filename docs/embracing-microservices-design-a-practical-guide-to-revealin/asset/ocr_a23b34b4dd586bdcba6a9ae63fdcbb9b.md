The preceding figure illustrates the concept of having Azure Application Insights as a centralized monitoring system, and each microservice is communicating to that service using the Azure Application Insights SDK.

> **Note**
> 
> To learn more about Azure Application Insights, refer to `https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview`.

Azure Application Insights provides a lot of metrics that include, but are not limited to, request rates, response times, failure rates, dependency rates, page views, load performance, and several others.

# Implementing distributed tracing

*Distributed tracing* is also known as *distributed request tracing*. It is a monitoring mechanism that logs traces of the request flow path. Distributed tracing allows you to track how each individual request is handled and helps you locate problems. It is one of the important components when considering monitoring in a microservices architecture. It can be implemented with a centralized logging mechanism where all the traces from different services can be dumped and then monitored. However, with microservices architecture, this is sometimes challenging since the system is based on polyglot technologies, and the monitoring system does not provide the option to integrate with all the platforms or technologies used to build microservices.

Azure Application Insights is one of the cloud services that is best suited in this scenario as well, as it provides integration options with almost all existing popular technologies today. Whether it is .NET, Java, Python, JavaScript, Angular, React, PHP, Node.js, or any other language or platform, it provides an SDK that can be used to log traces.

Some of the practices to consider when implementing distributed tracing are as follows:

+   - Each request should be assigned a unique number. In a microservices architecture, we can assign this number at the API gateway level.
+   - Pass the request ID to all the services part of the request context.
+   - Include the request ID in all the log messages for better traceability.
+   - Capture information such as start time, end time, and the operation performance, along with other important attributes.
+   - Log everything centrally to one monitoring platform.