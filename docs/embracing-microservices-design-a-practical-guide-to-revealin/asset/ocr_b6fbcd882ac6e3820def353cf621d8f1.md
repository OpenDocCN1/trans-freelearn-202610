The Dapr runtime can be operated in self-hosted mode or Kubernetes mode. In Kubernetes mode, Dapr injects a sidecar into the pod to enable the sidecar to make service calls on behalf of the application. This makes the application logic simple and lightweight, where Dapr takes care of the overhead associated with the distributed application architecture. Applications are written in different languages, which means they can use the available SDKs to consume the Dapr runtime via HTTP or gRPC APIs. Dapr offers its capabilities in the form of building blocks, each addressing a set of common challenges related to distributed systems, as illustrated in the following Dapr architecture:

![](bbox=[0.149, 0.269, 0.821, 0.597])

Figure 3.10 – Dapr architecture

Each building block provides a specification (API) which can have multiple implementations, implemented as Dapr components. This helps you choose the right implementation that's tailored to your application. We will discuss each building block in more detail in the following sections.

# Service-to-service invocation

In a microservice architecture, different microservices communicate with each other to complete a business task. Service-to-service invocation helps in addressing the common communication challenges regarding service discovery, distributed tracing, error handling, and retries.