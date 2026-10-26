
In the preceding diagram, events are produced by producer microservices as an event stream, which is later consumed by multiple consumer microservices. Event streams are served by the event broker (or event bus), which decouples producers from consumers. One of the challenges of event-driven architecture is its difficulty in figuring out how different microservices are communicating with each other via events. One way to address this challenge is to monitor these systems and to track the flow of events and how different systems are reacting to them. In the next section, we will discuss various pitfalls of design events and how they can be addressed using domain events. Let's move ahead and explore them in more detail.

## Event design pitfalls

An event is a core concept that enables communication between event-driven microservices. Events promote decoupling by allowing systems to be flexible, resilient, and scalable. All these promises of event-driven architecture depend on how well the events are designed. The following is a list of the different pitfalls that you might face while designing events:

- **Event dependency:** An event that can be affected by past or future events is considered to be a dependent event. In an event-driven architecture, events should be atomic, self-contained, and independent. The consumer should be able to process an event without waiting for other events. Independent events allow different consumers to process the same event differently without relying on the publisher or the publisher making assumptions or intentions about its processing.
- **Entities as events:** Another common anti-pattern is to use database entities as events. These entities are part of the internal implementation of the services and might not convey the right intent in the business context.
- **Generic events:** Avoid using generic events, as this will require consumers to analyze a message before deciding whether they want to process it or discard it, which wastes precious CPU cycles.
- **CQRS, event sourcing, and events:** Command and Query Responsibility Segregation (CQRS) and event sourcing are used together; here, the command and event sourcing provide the write model, while the event store projection and query provide the read model for the system. In such architecture, you'll observe that teams disguise commands as events. Commands are usually for a single consumer, while an event is intended for any consumer that's interested in the event. Another observation you can make is around exposing event store events as a method of communication between different business domains. The downside of this approach is that any changes to an event store are now tied to other business domains, which makes it harder to evolve. Both CQRS and event sourcing were discussed, in detail, in *Chapter 5, Data Design Pitfalls.*

## Domain events

In the context of **domain-driven design (DDD)**, domain events are classified as business events. They are considered first-class citizens in DDD, and form the ubiquitous language that's used by stream-aligned teams to understand the business domain. The concept of ubiquitous language was discussed, in detail, in *Chapter 2, Failing to Understand the Role of DDD*. Domain events are specific and contain the necessary information that serves as a contract between the business domains to facilitate communication; for example, the `payment_received` event, which belongs to the payment domain, is generated once a payment has been made by the customer. The invoice generation can subscribe to `payment_received` and perform the necessary action to generate an invoice. Once an invoice has been generated, the system will generate an `invoice_generated` event, which can be processed by the notification system to send notifications.

An event is a critical part of the event-driven architecture. This is because it acts as a data carrier and a contract between microservices. These events should be domain-driven and represent business events. Both events and API interfaces are ways in which to enable communication. As systems evolve, these interfaces go through several changes, resulting in the release of multiple versions of the service. In the next section, we'll explore the role of versioning in the microservices architecture.

## Avoiding service versioning

In Chapter 1, Setting Up Your Mindset for Microservices Endeavor, we talked about the API-first approach as part of the additional factors of building cloud-native applications in addition to the Twelve-Factor App methodology. The API-first approach emphasizes defining a service contract that dictates the behavior of a service. The service API is a contract between a service and its consumers. The service might undergo many changes during its life cycle, which might require changes to its API. A service could have multiple consumers, and it's crucial to ensure backward compatibility during several API changes to allow different consumers to continue their operations without making any changes.

The API changes can be broadly categorized as breaking and non-breaking. Breaking changes require clients to update to a newer version of the API, while non-breaking changes don't affect clients and allow them to continue working with the older version of the API. Any removal or change in the interface, including the message format, method signature, or response type, is considered a breaking change. The addition of new interfaces, including message formats and message types, are considered to be non-breaking changes.

In the light of the Twelve-Factor App methodology, it is essential to have a single code base for your microservice. This strategy works well with non-breaking changes. However, breaking changes introduce a new challenge where you might have to keep different instances of services running to support different consumers. For example, let's imagine you are decomposing a monolithic e-commerce application into microservices, where these microservices call each other to address different business use cases. A better approach would be to ensure that every change is both backward and forward compatible, which can help in a number of ways, such as allowing rollbacks. In the following diagram, we will discuss the effect of deploying new versions of these services with breaking changes:

### Independent deployment

### Breaking changes

### Deploying new client versions

### Lock-step deployment

### Figure 6.9 – Application versioning

In the preceding diagram, we have three microservices, where Microservice A and Microservice B communicate with Microservice C to get the work done. We can say that both Microservice A and Microservice B are dependent on Microservice C and any breaking change of Microservice C can affect both microservices. The deployment of different microservices and their impact at different stages are discussed as follows:

- 1. We start with the independent deployment of microservices. All the microservices are running version v1. This allows us to deploy new versions of Microservice A and Microservice B independently, but can we do the same for Microservice C?
- 2. We have a new version of Microservice C; we call it v2. The newer version of the microservice has updated the interface; hence, it's a breaking change that affects how Microservice A and Microservice B can invoke the capabilities of Microservice C.
- 3. Now, both Microservice A and Microservice B have implemented the changes introduced by Microservice C and have deployed their version, v2.
- 4. The team has realized that Microservice C can't be deployed independently; rather, they also need to deploy dependent services. This is what we call lock-step deployment.

![](img/e6276d4d880001c04e81cb4db4898f1c_191_0.png)

![](img/e6276d4d880001c04e81cb4db4898f1c_191_1.png)

![](img/e6276d4d880001c04e81cb4db4898f1c_191_2.png)

![](img/e6276d4d880001c04e81cb4db4898f1c_191_3.png)
