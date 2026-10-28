# Understanding serverless architecture

**Serverless architecture** is a software design pattern where applications are hosted by a third-party service, allowing developers to build and run services without having to manage the underlying infrastructure.

Applications are divided into separate functions that can be called and scaled individually.

Developers implement and deploy application-only code and can run applications, databases, and storage systems hosted in servers provisioned by cloud providers.

We will understand the role of APIs inside an application and the ecosystem. To start modernizing legacy applications without any modification, we can use APIs to interact with existing code. These APIs will run in the cloud, but if we need to add more features to the application to modernize it, we will encapsulate the data and the functions in services via an API. This encapsulation phase is a technique that consists of reusing legacy software components. The goal is to keep the code in its environment and connect it to the new presentations via encapsulation to access the different layers through an API. APIs permit you to define common protocols (rules) to expose data from an application with the possibility of decoupling the presentation layer.

Technically, the encapsulation works with wrapper technology, providing a new interface for a legacy component. This component will be easily accessible from the rest of the software components.

These minimal modifications reduce the risk of destabilizing the application. Even if encapsulation is a fast, effective, and less expensive solution, it will not solve the present problems related to the difficulties of maintenance or upgrading.

Because we are talking about the role of APIs in digital transformation and their life cycle, we need to define them.

# API definition

APIs form a bridge between different applications to ensure communication and the exchange of information between them. They can even define the behavior of these applications.

APIs are considered connectors because they provide the ability for disparate applications to exchange information.

The information exchanged is generally data; for example, if we make a request in a mobile application, the data will be sent to a server that performs the reading and then sends back a response in a format readable in JSON.

Sometimes, microservices are compared with APIs. We often talk about the relationship between APIs and microservices, but there is also a difference between them. Microservices are a style of architecture that divides an application into a set of services; each service presents a very particular domain, for example, an authentication service or another service for the management of the products. On the other hand, an API is a framework (a structure that you can build software on) used by developers