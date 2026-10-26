# Synchronous versus asynchronous communication

To enable communication between clients and services, we will explore the fundamental differences between various alternatives to help us choose the right communication type for our microservices. In **synchronous communication**, the client sends a request and waits for a response from the service. Synchronous communication can either be blocking or non-blocking, but the client can only continue further processing once a response has been received from the service. This pattern is also known as the synchronous **request/response pattern**. In service-to-service communication, request/response patterns can either be synchronous or asynchronous. In async API calls, the call is made, but the calling service is free to do other processing while the other service is working on a response. This pattern is useful in improving the throughput of the service.

In **asynchronous communication**, the client sends a message to the message broker, which acts as a middleware between client and service. Once the message has been published to the message broker, the client will continue processing without waiting for a response. The service can subscribe for specific messages to the message broker and place responses back in the message broker after processing. Both client and service are unaware of each other and can continue to evolve without any dependency.

In most cases, a microservices application uses both synchronous and asynchronous communication to address different use cases. However, if possible, asynchronous communication is the preferred approach, as it offers loose coupling and independence between microservices via messages. In the following section, let's explore messages and their types.

# Messages and message types

A message is defined as data that is sent between the client and its receiver, which serves as a means of communication by exchanging information. There are different types of messages that are categorized based on their intent. These message types are described as follows:

+   - A **command** is defined as a message sent by the client with an expectation that the receiver performs an action.
+   - A **query** is defined as a message sent by the client with the intent of expecting a response from its receiver. The response could be a success or a failure. If no response is received, the client can decide to retry the request.
+   - An **event** is defined as a message that indicates an event that happened in the past in the system without any expectation from the receiver. The receivers of the event can ignore or perform necessary actions based on the information provided in the event.