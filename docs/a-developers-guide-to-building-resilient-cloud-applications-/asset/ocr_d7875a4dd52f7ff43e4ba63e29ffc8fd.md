# ## Use case

Let’s start by looking at a problem relating to the direct communication between the client app and microservices. The following figure shows our solution, which contains several frontend and backend services that are directly interacting with the client applications.

In our scenarios, the client apps must send requests directly to the microservices. How does the client know which endpoints to call? What happens if we introduce a new service, for example, or if we decide to make a change and refactor an existing service?

How do services handle authentication or **Secure Sockets Layer (SSL)** termination, data transformations, and dynamic request dispatching?

How can client apps communicate with services that use different protocols? There are some potential problems when we expose services directly to clients:

+   - The client must track multiple endpoints that have been implemented to ensure resilient fault handling, resulting in a complex client code implementation.
+   - Coupling between the client and backend. This makes it more difficult to maintain the client and also more difficult to refactor services.

![](bbox=[0.135, 0.468, 0.846, 0.69])

Figure 2.1 – Scenario using a direct client-to-microservice communication architecture

All these challenges can be resolved by implementing an API Gateway pattern to decouple clients from services because having an intermediate level or tier of indirection (gateway) can be convenient for microservices-based applications. The API gateway sits between the client apps and the microservices.