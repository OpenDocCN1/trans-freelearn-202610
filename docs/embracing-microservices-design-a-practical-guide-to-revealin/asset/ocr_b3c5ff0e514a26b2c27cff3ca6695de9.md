# # Gateway pattern service discovery

An API gateway is the entry point for clients. Instead of calling services directly, clients make calls to the gateway, which intelligently routes the call to the appropriate backing services.

Advantages of using an API gateway include the following:

+   - It decouples the client from backing services. This allows services to be versioned and refactored without needing to update other services or clients.
+   - Services can use messaging protocols such as the **Advanced Messaging Queuing Protocol (AMQP)** for queues or **publish-subscribe (pub-sub)** scenarios.
+   - An API gateway such as NGINX that operates on **Layer 7 (L7)** can perform other cross-cutting concerns such as authentication caching, logging, **Secure Sockets Layer (SSL)** termination, and load balancing.

Many of the benefits of microservices are the same in DDD. Let's examine a few of them to compare the similarities.

# # Benefits and challenges of microservices as compared to DDD

There are various benefits offered by microservices in comparison to DDD. Let's have a look at them, as follows:

+   - **Highly agile:** Due to the fact that microservices are deployed independently, this makes managing fixing bugs and adding features much easier. You make changes to a microservice without redeploying the entire application, or if something goes wrong, we can roll back an update to a previous version during a deployment. In monolithic applications' deployments, if a bug is discovered during deployment, it can block the entire release, requiring the rolling back of the release. This can cause a delay in the deployment of new features. This trait of microservices being independently deployable is shared with DDD rules around how a bounded context is deployed, and they have their own code base. So, you can clearly see that by understanding DDD, you understand how microservices should be built and deployed.