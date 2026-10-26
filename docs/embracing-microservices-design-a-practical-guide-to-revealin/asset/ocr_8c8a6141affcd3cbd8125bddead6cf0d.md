# Implementing health checks

*Health checks* are used to provide a health status for services. A health check is a REST API that is used to check the health of microservices. When developing microservices, you can expose some web methods to return the health status of the service. Health checks can access these endpoints to see if the service is running and validates all areas that include database connections, services endpoint connections, and other resource availability. The web methods should check all this information and return the response. The following figure demonstrates the implementation of a health check API, which calls multiple APIs to check the health status:

![](bbox=[0.2, 0.32, 0.78, 0.88])

Figure 7.12 – Implementing a health check in microservices architecture