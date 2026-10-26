The following diagram depicts the use case of a service registry in a microservices architecture:

![](bbox=[0.13, 0.21, 0.88, 0.56])

Figure 7.9 – Client-side service discovery pattern

In the previous figure, the frontend application is a registry-aware client application that communicates to the service registry over HTTP and gets the list of running instances details to access them directly. On the other hand, microservices should be capable of self-registering to the service registry when they start and unregistering when they shut down. Moreover, any third-party registration service can also be used to register/unregister services to the service registry.

# Implementing server-side service discovery patterns

With a server-side discovery pattern, the client can make a request to the load balancer that acts as a front door to the backend services and route the traffic based on the load balancing algorithm. In this scenario, there is no service registry where the client can request the available endpoints. Instead, it contains a load balancer endpoint that can be used to make a request.