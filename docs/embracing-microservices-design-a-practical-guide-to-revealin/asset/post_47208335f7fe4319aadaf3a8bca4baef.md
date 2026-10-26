
The following figure depicts the use case of the gatekeeper pattern that acts as a piece of security middleware between frontend and backend applications to protect them:

![](img/e6276d4d880001c04e81cb4db4898f1c_218_0.png)

Figure 7.8 – Using the gatekeeper pattern

The preceding figure shows the use of the gatekeeper pattern that validates and sanitizes requests before sending them out to the backend services. Gatekeeper patterns can be implemented by adding the API gateway as a proxy to backend services. The **Trusted Host** is the system or platform used to host the microservices. Kubernetes could be an example of a **Trusted Host** in microservices architecture.

A few considerations to understand regarding gatekeeper patterns are as follows:

- Gatekeepers should not hold any tokens or access key information.
- Gatekeepers should not contain any logic to perform operations related to services or data.
- Gatekeepers should be designed as a single point of failure and there should not be any room to access the services directly.
- You should consider implementing monitoring and logging at the gateway level.
- Always use secure connections when communicating to or from frontend or backend services and the gatekeeper. Use secure connections such as HTTPS, SSL, and TLS.

The gatekeeper pattern is one of the most common patterns used when building enterprise-grade applications. In Azure, **API Gateway**, **Azure Front Door**, and **Azure API Management** can be used to implement a gatekeeper pattern.

## Keeping configuration inside microservices

Every business application needs to connect with various systems to perform business operations. To communicate with systems that could be databases, message brokers, services, or other platforms, you need to provide a key or a connection string to access them. Keeping all of these configuration values in the microservice itself is an easy option, but it introduces tight coupling when you want to connect to other environments. The solution to this pitfall is to externalize your configuration values, so instead of changing the values at the application level, when deploying to that environment, you can switch these values from the external system to the target respective environment.

## Externalizing configuration

Externalizing configuration is the practice of decoupling the configuration values from the application. One approach is to set the configuration values as parameterized values when running the application and then building the logic inside the application to read those values at runtime. On the other hand, you can create a data store that holds all of the property values, and the application can read them at runtime depending on the context in which it is functioning. The application does not need to be recompiled with this approach, and the values can be conveniently maintained.

When using DevOps for automating build and release cycles, configuration values can be defined in the pool of variables collection, also known as *variables groups* in the context of Azure DevOps. The values can easily be changed and used with respect to the environment you are targeting.

## Using Azure Key Vault

**Azure Key Vault** is a cloud-based service for storing secrets. You can keep all the configuration values or properties inside Azure Key Vault and use it as an external configuration storage provider. All the keys/values stored in this resource are highly secured and it enforces the TLS protocol to protect them between clients and Azure Key Vault.

You can store all your application's secrets at one central location that is closely regulated and secured, and you can access them safely by using their special URIs. Furthermore, proper authentication and authorization are required for any access. Authentication can be accomplished in a variety of ways, including **Azure Active Directory** authentication, **Role-Based Access Control (RBAC)**, or through a Key Vault access policy.

> **Note**
> 
> To learn more about Azure Key Vault, refer to `https://docs.microsoft.com/en-us/azure/key-vault/general/basic-concepts`.

Some of the best practices when using Azure Key Vault are as follows:

- Create access policies for Azure Key Vault.
- Enable least privilege access to Azure Key Vault.
- Turn on firewall.
- Enable VNET service endpoints if the other cloud resources are accessible over VNET.
- Enable soft delete to protect your keys from accidental deletion.
- Set up automation to automatically respond when your secrets rotate.
- Store all security certificates in your vault.
- Ensure that the deleted key vaults are recoverable by enabling the built-in protection features of Azure Key Vault.
- Enable monitoring and set up some metrics to monitor the vault's availability, saturation, latency, and error codes.
- Set up alerts to send out notifications when any of the metric conditions suffice.

## Undiscoverable microservices

In a microservices architecture, services are hosted separately on a different IP and port. Unlike a monolithic application, where you can add a direct reference to the module or component you want to use, with microservices you need to make an HTTP/REST call to invoke the operation of the target service. Since the system with microservices is divided into sets of various services, you need to build some mechanism to make your services discoverable.

Services are either consumed by the client or frontend applications or by other services to perform service-to-service invocation. To do so, implementing service discovery patterns at both client and server levels is important. To clarify our understanding, in the next section, we will discuss the client-side service discovery and server-side discovery patterns, with examples.

## Implementing client-side service discovery patterns

Microservices usually run inside a containerized environment, where multiple instances of a single service can run in parallel and each instance is assigned a unique IP address or port. The problem is – how does the client know if the IP changes or is dynamically assigned?

The solution to this is by implementing the *service registry* pattern. A service registry registers all the services running across the cluster and lets clients request the service endpoint by providing the service name or key. A service registry can be considered a database of services, their instances, and other information, such as IP addresses and ports. The client application searches the service registry to obtain the list of available running instances and selects the one that best serves the load balancing criteria. The `heartbeat` function is used by the service registry to check the availability of services.

The client must be able to access the service registry and you must write some code to connect to the service registry. After getting the details of the active instances, the client must decide to use the correct endpoint to connect to that microservice directly. This kind of client is also known as a *registry-aware client.*

The following diagram depicts the use case of a service registry in a microservices architecture:

![](img/e6276d4d880001c04e81cb4db4898f1c_222_0.png)

Figure 7.9 – Client-side service discovery pattern

In the previous figure, the frontend application is a registry-aware client application that communicates to the service registry over HTTP and gets the list of running instances details to access them directly. On the other hand, microservices should be capable of self-registering to the service registry when they start and unregistering when they shut down. Moreover, any third-party registration service can also be used to register/unregister services to the service registry.

## Implementing server-side service discovery patterns

With a server-side discovery pattern, the client can make a request to the load balancer that acts as a front door to the backend services and route the traffic based on the load balancing algorithm. In this scenario, there is no service registry where the client can request the available endpoints. Instead, it contains a load balancer endpoint that can be used to make a request.
