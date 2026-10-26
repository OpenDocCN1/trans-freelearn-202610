# Communication protocol across services

Microservices are independent fine-grained services that communicate with each other across process boundaries, hosted across the network, to address different business use cases. Choosing the right communication protocol is a very important factor to ensure reliable communication. The following aspects should be considered when assessing this factor:

+   - Are you following an API-first approach?
+   - Do you have deep chaining of services over synchronous communication protocols?
+   - Do you have asynchronous communication anywhere in the system?
+   - Which message broker technology are you using?
+   - What is the throughput of messages received or processed by the services?
+   - Do you have direct client-to-microservice communication?
+   - Do you need to persist messages?
+   - Are you using a materialized view pattern to address the chatty behavior of microservices?
+   - Have you implemented a retry pattern, a circuit breaker, exponential back-off, and jittering?
+   - Have you defined domain events to facilitate communication between microservices?

# Exposing services to clients

An API gateway is one of the core components when developing a microservices architecture. Exposing microservices directly to clients is not a good practice due to many factors. You should evaluate the following:

+   - Are the services directly consumed by the frontend application?
+   - Is there a gateway that acts as a facade for all backend services?
+   - Does the gateway provide quota limits, mocking responses and filtering Internet Protocol (IP), and other policies?
+   - Are you using multiple API gateways to address the needs of different clients?