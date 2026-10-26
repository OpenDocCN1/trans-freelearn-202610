# # Chapter 6: Communication Pitfalls and Prevention

+   1. In synchronous communication, the client sends a request and waits for a response from the service. Synchronous communication can be blocking or non-blocking, but the client can only continue further processing once the response is received from the service. In asynchronous communication, the client sends a message to the message broker that acts as middleware between the client and service. Once a message is published to the message broker, the client will continue processing without waiting for the response. The service can subscribe for specific messages to the message broker and place responses back in the message broker after processing. Both the client and service are unaware of each other and can continue to evolve without any dependency.
+   
+   2. Direct client-to-microservice communication introduces tight coupling between the client and microservice, where clients need to be aware of how different microservices can work together to achieve a business use case, which results in implementing service orchestration by the client application. Furthermore, calling each microservice from the client adds latency and affects the user experience and mobility.
+   
+   3. When multiple microservices call each other repeatedly in a long chain, there is potential that a microservice may take more time to respond than expected, which can result in timeouts. These timeouts will initiate additional retry requests with an expectation that the operation may succeed and the flood of these retries will eventually make the system unusable. The situation is known as a retry storm.
+   
+   4. There are four different approaches to HTTP/REST API versioning, as mentioned next:
+      a) URI path versioning
+      b) Query parameter versioning
+      c) Content-type versioning
+      d) Custom header versioning
+   
+   5. A service mesh provides an abstraction layer on top of microservices to provide capabilities such as traffic management, security, policy, identity, and observability for your microservices. The idea behind the service mesh is to decouple these concerns from the implementation of microservices and handle them in a consistent manner across the microservices architecture.