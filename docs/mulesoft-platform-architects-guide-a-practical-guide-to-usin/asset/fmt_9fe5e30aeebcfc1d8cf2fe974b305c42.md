
# Microservices Architecture

## Advantages

*   `Continuous deployment`: A microservices architecture allows you to perform continuous deployment and continuous integration effectively and allows for the continuous delivery of software applications.
*   `Resource optimization and utilization`: To optimize resource utilization and reduce infrastructure costs, microservices can be run in containerized environments or serverless architectures.
*   `Business agility`: The microservices are well aligned with agile methodologies, enabling organizations to react quickly to changing business requirements.

## Disadvantages

The following are the disadvantages of a microservices architecture:

*   `Deployment and scalability complexity`: A microservices architecture allows you to scale and deploy the application independently, but at the same time, scaling and deploying multiple microservices can be challenging to manage and adds complexity. There are tools such as Kubernetes and Docker that help to reduce deployment and scalability complexity, but they come with their own added complexity.
*   `Complex and distributed application network`: Managing multiple microservices can be complex. With a greater number of services, it becomes harder to manage and monitor those services. Microservices are inherently distributed, which may result in additional network latency that is difficult to solve, as multiple services may need to be called to serve a single client request.
*   `Increased costs`: Microservices can add additional costs because the additional tools used to manage and control microservices, such as Kubernetes, Docker, and API gateways, are often required and come with additional costs.

We have now reviewed the monolithic and microservices architectures and discussed the advantages and disadvantages of both. Next, we will examine a couple of other architectural styles, namely, event-driven architecture and API-led connectivity.

# Saga Pattern

The `Saga pattern` is a failure management technique that assists in coordinating transactions across several microservices to preserve data consistency and aids in establishing consistency in distributed applications. Data consistency is managed and upheld across numerous microservices using this pattern. A single service completes each transaction, and it broadcasts changes in state to the other services participating in the Saga. Through a series of local transactions, the Saga architectural pattern offers transaction management. A Saga participant's work is represented by a local transaction. A compensatory transaction can roll back every Saga activity. Furthermore, the Saga pattern ensures that the relevant compensation transactions are executed to roll back the previously finished work in case of any failure in between to ensure data consistency or that all activities are completed.

## Note

In software, Saga is a design pattern used to manage distributed transactions. It is a series of local transactions where each event updates data in a single service. The term comes from the paper called `SAGAS` written by Hector Garcia-Molina and Kenneth Salem in 1987. The Saga pattern is mainly used in microservices architecture to ensure data consistency across multiple services. Each Saga transaction has a defined recovery process in case of failure. If one transaction fails, rollback processes are performed to maintain data consistency. Saga events are coordinated by an orchestra, which can be either centralized or decentralized.

## Local Transactions in Saga Pattern

A local transaction within the Saga pattern refers to a transaction that occurs within a single microservice or database. Each of these local transactions can be executed independently and, if successful, should transition the system from one consistent state to another. The key distinction between local transactions in Saga and traditional distributed transactions is that partial success is acceptable in local transactions. This contrasts with traditional transactions where every part of the transaction must succeed for it to be considered successful.

In the event of a failure in a local transaction, instead of rolling back like in a traditional transaction, the Saga pattern executes a series of compensating transactions. These compensating transactions are designed to undo the changes made by the preceding local transactions in the Saga, thereby maintaining overall system consistency. It is important to note that these compensating transactions themselves are also local transactions.

The following diagram illustrates the Saga design pattern architecture.

![Figure 7.4 - The Saga design pattern architecture](image/B19922_07_04.jpg)

Figure 7.4 - The Saga design pattern architecture

The Saga pattern can be implemented in two ways:

*   `Saga choreography pattern`
*   `Saga orchestration pattern`

## Saga Choreography Pattern

The choreography approach involves a single microservice consuming an event, carrying out the necessary action, and then passing the event on to the subsequent service to consume.

The `Saga choreography pattern` is used to implement the transaction span across multiple services in a decentralized fashion.

The following figure depicts the Saga choreography pattern.

![Figure 7.5 - Saga choreography pattern](image/B19922_07_05.jpg)

Figure 7.5 - Saga choreography pattern

In the preceding diagram, all the services in the Saga communicate via a message broker for executing the transactions as well as rolling them back. This communication happens asynchronously in a sequenced manner.

### Advantages

The following are the advantages of the Saga choreography pattern:

*   Easily extensible to add new services into the mix, as it works in a decentralized fashion
*   Brings with it the advantages of messaging and event-driven architecture, such as loose coupling, asynchronous communication, and reliability
*   Enables greater flexibility and evolution, improving scalability and availability, and decreasing coupling between microservices

### Disadvantages

The following are the disadvantages of the Saga choreography pattern:

*   There is no centralized control over the transaction processing, so it becomes complex to manage and understand the process if it spans multiple services
*   Transaction rollback is complex as each service in a Saga must have transaction rollback capabilities
*   Monitoring and controlling the interactions between the microservices in a choreographed system can be more challenging in the absence of a central coordinator

## Saga Orchestration Pattern

In the orchestration pattern, the application is completed by connecting each service to the centralized coordinator, which arranges the services in a predetermined sequence. A centralized Saga orchestrator instructs Saga players to carry out local transactions using command messages in an orchestration-based Saga.

The following diagram illustrates the Saga orchestration pattern:

![Figure 7.6 - Saga orchestration pattern](image/B19922_07_06.jpg)

Figure 7.6 - Saga orchestration pattern

Using a centralized controller, a distributed transaction may be implemented using the Saga orchestration microservice design. The calls between the Saga participants are coordinated using this centralized controller to carry out a transaction.

Saga controllers manage communication with the service, perform transactions, and are responsible for notifying services in Saga to perform a rollback in case of any failures.

### Advantages

The following are the advantages of the Saga orchestration pattern:

*   More control of the processing of transactions when they are explicitly executed through the orchestration pattern
*   Monitoring and controlling the interactions between the microservices in an orchestrated system is made simpler with a central coordinator
