
When you click on the mobile event viewer, you will see the following screen, which guides us through the event creation process:

![](img/8948f94d0abcdb1ae94d6778da518d16_39_0.png)

Figure 2.12 – The Mobile Event Logger interface

To create the event that we want, follow these steps:

- 1. Select the tracking ID of DEF789 from the drop-down list.
- 2. Click the Next button.
- 3. Select the location of where the cargo had arrived – for example, Dallas.
- 4. Click the Next button.
- 5. Select the UNLOAD event type.
- 6. Select the voyage the cargo is currently on. In our case, this is 0301S.
- 7. Click the Next button.
- 8. Select the date on which the event occurred. In our case, I selected 16-02-2023.
- 9. Click the Next button.

After following these steps, you will be shown a confirmation screen that shows all the information you filled in during the previous steps. Check that they are correct; if they are, you can click the Submit button, which will send the event to the Cargo Tracker application.

When we look at the administrative interface, we will see that the cargo’s status is now IN_PORT and that the last known location is Dallas for the cargo with an ID of DEF789.

## Java EE features used in the Cargo Tracker application

Now that you have an idea of how the Cargo Tracker application works, it is time to look at the technologies involved in making it work. Java EE consists of multiple projects and technologies where each of the projects can leverage the functionalities of the other.

The Cargo Tracker application uses a lot of Java EE functionality. In the next few sections, we will walk through those that are mostly used. We will briefly describe what the functionality is and how it is used with the Cargo Tracker application.

Will be using the Java EE naming of these technologies. Jakarta EE has different names for these technologies. We will discuss the following Java EE 7 technologies:

- Enterprise Java Beans
- Java Persistence
- Java Messaging System
- Context and Dependency Injection
- Java Server Faces
- JAX-RS
- JSON-B
- Java Batch

Note that during the switch from Java EE to Jakarta EE, the names of many technologies had to be changed due to legal requirements. In *Appendix A*, you can find a list that compares the technologies by the original Java EE name and its new Jakarta EE name.

## Enterprise Java Beans

Enterprise Beans have been around since day one. In the old “days of J2EE, there were several different types of enterprise beans: **Session Beans (SB), Entity Beans (EB), and Message Driven Beans (MDB).**

Session beans can be further subdivided into Stateful Session Beans, Stateless Session Beans, and Singleton Session Beans. While the former two have always been around, the latter was added in version 3.x of the specification.

Stateful Session Beans maintain a conversational state with a client. This means that the client has a one-to-one connection with a specific bean and that this bean only serves that client for the duration of the conversation. For example, we can have `ShoppingCartSession` that connects with a Faces shopping cart.

Stateless Session Beans have no knowledge of what clients they had a conversation with during their life cycle. The application server typically pools them. Once a request for Stateless Session Beans arrives at this application, a bean is selected from the pool and the requested method is invoked on it. Stateless Beans provide the container with a great level of flexibility as it can increase or decrease the pool size as it seems fit.

The final kind of session bean is the Singleton Session Bean. This one works just like a stateless session, but there is only ever one instance of this bean for the whole application. Singleton Session Beans are typically used for tasks such as upgrading the database or other batch-related tasks.

Moving on to the next kind of Enterprise Beans, we have the Entity Beans. These beans were introduced in version 1.0 of the specification and came in two flavors: Bean Managed Persistence (BMP) and Container Managed Persistence (CMP). The former left it to the application developer to invoke persistence operations and perform transaction management, while the latter left it up to the EJB container to invoke the appropriate data retrieval/store and transaction control operations. These types of beans became very unpopular since all the operations and parameters needed to be specified in a special deployment descriptor file (ejb-jar.xml), leading to XML files that more frequently than not contained several hundreds of XML statements. They became so unpopular that in version 3 of the Enterprise Beans specification, they were dropped altogether in favor of Java Persistence.

The final type of bean is the Message Driven Bean. This is a specialized form of bean that's registered as a consumer of JMS (or Messaging) messages placed on a queue or topic. Once a message arrives on the queue or topic, this beans' callback method will be invoked to execute some business logic.

## Persistence (JPA)

As mentioned earlier, Java Persistence is the replacement for Entity Beans. JPA should be seen in the widest sense: creating, updating, retrieving, deleting, and querying. Objects that are to be persisted are annotated with the @Entity annotation. Moreover, relationships between entities can be modeled in code.

JPA was heavily influenced by the ideas and concepts of Hibernate, a popular object-relational mapper. It tried to offer a workaround for the fact that, in code, we work with objects that can embed other objects while once persisting or retrieving data, we work with flat tables.

JPA also offers possibilities for querying data via conventional SQL, an object-oriented query DSL, and object-based query definitions called the Criteria API.

## Messaging (JMS)

JMS is the concept by which disjoint systems can integrate through the exchange of messages. Messaging is certainly not something that was invented by Java EE. Even before the rise of Java, there were many implementations of this so-called message-oriented middleware (MoM). Examples include IBM MQ and Tibco Messaging Service. More recently, Apache Kafka attracted enormous

attention inside and outside of the Java community. JMS provided a specification for a standard way of working for messaging.

JMS supports two types of messaging: **point-to-point (PTP)** and **publish and subscribe (pub-sub)**. PTP works with queues, which is a place where the two clients involved in the messaging process can exchange messages between one another. So, this is typically a one-to-one connection. Pub-sub, on the other hand, works in such a way that one client will post messages on a topic and an unbounded number of clients will show their interest in receiving these messages by subscribing to this topic.

One of the main advantages of asynchronous communication via messaging is that a system is not influenced heavily by the unavailability of the intended receiving system. To the sending system, the message still appears to be sent. It will sit idle in the queue until the receiving system comes online to retrieve it. Compare this to a situation in which a direct call is made into another system (for instance, via REST) and this system would not be available. This might lead to all kinds of problems due to the synchronous way of coupling the two systems. Now, increase the number of connected systems and you will start to see why messaging is such a brilliant solution.

### Context and Dependency Injection

CDI is the Context and Dependency Injection API. It is obvious that over the years, Java EE was heavily pressured and influenced by the Spring framework. CDI is the best example of this as the Spring framework popularized **dependency injection (DI)**. DI is the concept that an object should not instantiate itself the objects it requires, but rather have these objects injected into it. DI encourages you to have an **inversion of control (IoC)** for dependency injection, and CDI builds upon that specification.

Next to injecting objects in a class at runtime, CDI also has the concept of producers. These are not the dependent objects themselves but objects that know how to get the dependent object.

CDI is not just about injecting dependent objects, though – it also provides an event mechanism. This allows one bean to fire an event while another bean has registered itself as being interested in these events. So, once the event is fired, a listener class can react to it. As an example, imagine a bean called `CustomerRegistrationBean` that will fire a `CustomerCreatedEvent` event. The `EmailBean` bean will listen for these events and upon receiving the `CustomerCreatedEvent` event, it will send out a welcome email to the new customer.

It is good to point out that CDI is not the only dependency injection mechanism available in Jakarta EE. Since version 3.0, EJB has also supported annotations for dependency injection:

- @EJB
- @Resource

@EJB allows you to inject other EJBs into an EJB, while @Resource allows you to inject program objects that provide connections to other systems, such as a SQL DataSource or a Jakarta Messaging MessageFactory.
