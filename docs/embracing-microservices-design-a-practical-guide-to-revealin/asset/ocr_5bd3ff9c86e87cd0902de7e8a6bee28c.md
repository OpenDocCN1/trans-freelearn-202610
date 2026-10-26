The following diagram shows the difference between the monolithic architecture and a microservice-based architecture. The monolithic architecture shows two kinds of n-tier architectures, where the left-hand side shows a direct reference to the business layer inside a presentation layer, which enables direct dependencies between layers. On the other hand, the right-hand side of the monolithic architecture shows loose coupling by having a service layer exposing APIs to allow the presentation layer to communicate with the service layer over HTTP/HTTPS, HTTP/2, or gRPC protocols:

![](bbox=[0.15, 0.28, 0.92, 0.58])

Figure 4.1 – Comparing the monolithic and microservices architectures

In a microservices architecture, the system is decomposed into a set of various fine-grained services, where each service is a RESTful API exposing an endpoint over HTTP. The presentation layer contains various frontend components and communicates with the backend services over standard protocols, as mentioned previously.

# Simple deployment but expensive scaling

Monolithic applications are simple to deploy since the application is packaged into a single assembly or artifact that can be deployed. Unlike microservices, where the application is decomposed into several services, manual deployment is not an option. The following diagram shows the difference between the monolith architecture and the microservices architecture for deployment: