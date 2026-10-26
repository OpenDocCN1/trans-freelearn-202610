
Also, API gateways are implemented as a mechanism to orchestrate backend microservices. The practice, well known as API gateway, is aggregator microservice. This concept is explained, in detail, in *Chapter 3, Microservices Architecture Pitfalls*. In the following diagram, a single API gateway is implemented to address the various requirements from different consumers:

![](img/e6276d4d880001c04e81cb4db4898f1c_178_0.png)

Figure 6.3 – A single monolithic API gateway

The API gateway also acts as an aggregator microservice, which brings the orchestration logic to the API gateway and, therefore, making it more complex and difficult to maintain. These API gateways can be a potential bottleneck and a source of single points of failure for the overall system. However, multiple API gateways can help to address the needs of different clients. Further, aggregator microservices can be used to orchestrate different microservices or materialized view patterns, which can help to reduce the communication between microservices. Let's explore them, in detail, next.

## Multiple gateways – backends for the frontends

To keep an API gateway implementation simple, you can have multiple API gateways based on different types of consumers. Sometimes, it makes sense to have separate API gateways for altogether separate purposes; for example, one for external clients, one for internal service calls, and one for partners to consume APIs, where each API gateway contains a different set of policies and subscription model. It also enables developers to provide optimal APIs for each consumer by addressing their data and latency requirements, reducing round-trips, and improving user experience. Another benefit of this approach is that it segregates APIs based on their security and risk profiles. Furthermore, it helps you to address the challenge of a single API gateway becoming the bottleneck and a single point of failure for the system. Many cloud providers offer API gateways as a service to help cloud-native developers use these services to publish their APIs without needing to reinvent the wheel. Authentication, authorization, service discovery, rate limiting, throttling, load balancing, logging, tracing, correlation, IP whitelisting, and response caching are just some of the features supported by API gateways. The following diagram depicts multiple API gateways, each acting as a backend for the frontend:

![](img/e6276d4d880001c04e81cb4db4898f1c_179_0.png)

In the preceding diagram, three different API gateways have been implemented to increase the performance, availability, scalability, security, and maintainability of the overall application. Each API gateway has implemented consumer-specific policies to enable communication between microservices and their consumers through the API gateway. As shown in *Figure 6.4*, the partner API gateway is not available due to deployment failure, but this does not affect other API gateways, and other channels will continue to function without any disruption. This helps us to reduce the blast radius and reduce the number of affected consumers.

We can further simplify the communication between the API gateways and backend microservices by introducing an aggregator microservice. The aggregator microservice is responsible for orchestrating communication between backend microservices and API gateway. This concept is explained, in more detail, in *Chapter 3, Microservices Architecture Pitfalls*.

## Materialized view patterns

The aggregator microservice has played an instrumental role in reducing the communication between the API gateway and backend microservice, but it has only helped us to shift the problem from the API gateway to the aggregator microservice. The materialized view pattern helps you keep a local copy (that is, a read-only model) of the data of other microservices, which is necessary for the primary microservice to function independently. This pattern eliminates calls to other microservices to increase the response time and reliability of microservices. The following diagram shows one attempt to make the overall microservices architecture more resilient by introducing multiple API gateways and materialized view patterns:

![](img/e6276d4d880001c04e81cb4db4898f1c_181_0.png)

Figure 6.5 – Multiple API gateways with materialized view patterns

In the preceding diagram, we have multiple API gateways addressing high availability, scalability, and fault tolerance within the API gateway layer. The search microservice has a read-only model of the relevant data from multiple microservices, which is necessary to perform its operations. It's recommended that you asynchronously update the read-only model using queue or pub/sub patterns. For write operations, you can opt for either synchronous or asynchronous communication; though, asynchronous communication is preferred. The concept is explained, in more detail, in Chapter 3, Microservices Architecture Pitfalls.

Now that you have a good understanding of how to prevent your API gateway from getting overly complicated, let's move on to discuss the factors to bear in mind when enabling service-to-service communication.

### Request timeout

In a microservices architecture, services are hosted as separate processes to communicate with each other over a network via RPCs. These microservices exchange messages using different protocols in order to consume interfaces exposed by these services. In synchronous communication, the consumer needs to establish a connection with a service to consume its capabilities. The ability to connect to a service to invoke its capabilities is defined as the **availability** of the service, while the time a service takes to respond to its consumer is known as its **responsiveness**.

Both availability and responsiveness are important aspects that need to be addressed by the consumer to enable resilient inter-process communication between services. The consumer of the service can choose to wait indefinitely or specify a timeout to wait for the response before it decides the next action. Although specifying a timeout seems like a good choice, it could result in a timeout anti-pattern. The following diagram depicts an inter-process communication between two microservices:

![](img/e6276d4d880001c04e81cb4db4898f1c_182_0.png)

Figure 6.6 – Inter-process communication between two microservices

In the preceding diagram, Microservice A is trying to consume the capabilities that are available in Microservice B. Microservice A has implemented retry logic to re-communicate with the target service in the case of a failure. Additionally, it has implemented the timeout functionality to ensure service responsiveness. The timeout will inform Microservice A about the unavailability of Microservice B in the event of a failure. The series of events between the two microservices are explained as follows:

1. Microservice A tries to invoke a functionality that is available in Microservice B. However, Microservice A is not able to connect to Microservice B. In this case, Microservice B is considered unavailable for Microservice A. Microservice A can respond back to the client with an error message stating the unavailability or it can retry. There are multiple ways in which to handle this failure. To avoid synchronous calls, one can opt for a materialized view pattern or a publish/subscribe pattern, as we will discuss later in the chapter.

2. Microservice A will keep on trying to invoke Microservice B due to the *Retry Logic* implemented as part of Microservice A. If no response is received in the allotted time, this results in a timeout. In this case, Microservice B is considered unresponsive. If the timeout is not specified, Microservice A will continue to wait for Microservice B, resulting in a waste of resources.

3. In an ideal situation, Microservice A invokes the functionality that is available in Microservice B and receives a response in the allotted time.

At a first glance, setting up a timeout value seems to be the obvious choice, but finding the right timeout value that addresses the challenges of high volume, network latency, service unavailability, or overcommitted resources is difficult. For example, you could request to withdraw $5,000 of cash from an ATM. The last thing you want to happen is to get credited from the account and not receive any cash from the ATM due to a service timeout. The service timeout could happen due to network latency or a service load. You can try again, but you will find out that the account has already been credited. One approach to address this problem is to set a high value of timeout to allow these services to respond in times of high volume and load. A common way to address this problem is to set the timeout value to double the response time during peak hours, as specified in the following:

*Average Response Time = 1500 ms*

*Response Time at Peak Time = 4500 ms*

*Timeout Value = 4500 ms x 2 = 9000 ms = 9 s*

You might have wondered why this is an anti-pattern. Now every service must wait for 9 s to let its consumer know about the failure of a request. During this time, the client can either close the window or hit the submit button multiple times. This is not a good experience and should be addressed with a better strategy.

> **Note**
> For a long chain of synchronous calls, it's not a good idea to implement a retry pattern because the consumer will experience a delay if the retry pattern is triggered. We will discuss long chains of synchronous calls in more detail in the *Long chains of synchronous calls – the retry storm section.*
