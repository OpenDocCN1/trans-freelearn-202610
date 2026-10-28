
![](img/66aceec30d3e7dead2f71bb68a887688_60_0.png)

Figure 2.29 – Find and replace string in body

We will be modifying the result sent back to the client as follows:

```
find-and-replace from="price_billing" to="price" />
```

In Outbound Processing, we are able to modify the response before it is sent to the client.

## Summary

In this chapter, we learned about the different components of the API Management service and their functions. After that, we focused on the role of API gateways in managing calls to our APIs. Then, we learned how to secure access to APIs using subscriptions and certificates. We created a backend API and used Azure API Management to proxy a public API.

In the next chapter, we will learn how to develop event-based and message-based solutions. We will explore Event Grid, Azure Event Hubs, and Azure message queues.

## Developing Event-Based and Message-Based Solutions

In this chapter, we will explore the use of event-based and message-based solutions. We will publish and subscribe from a web application, see how we can send and receive messages from a Service Bus queue by using the same application, discover the operating mode of Event Grid and how it connects to services and event handlers, and explore Azure Event Hubs.

In this chapter, we’re going to cover the following main topics:

- Exploring Event Grid and Azure Event Hubs
- Exercise 1 – publishing and subscribing from a .NET app to Event Grid events
- Exploring Azure message queues
- Exercise 2 – creating an Azure Service Bus namespace and a queue
- Exercise 3 – publishing messages to a Service Bus queue using a .NET Core application
- Exercise 4 – reading messages from a Service Bus queue using a .NET Core application
- Exercise 5 – sending and receiving messages to and from a topic

### Introduction

An event is an action that causes a change of state. This state change can be simple or complex, but when a state changes, this is considered an event. In the event-driven architecture approach, consumers *subscribe* to events and can receive notifications when they occur. This contrasts with the traditional server-client model, in which a client actively requests updates on a set of information. This model solves a simple problem – how can we notify consumers of status changes? The message-based approach solves this same problem but in a different way than the traditional server-client model. The simple idea behind the message-based approach is that instead of consumers subscribing to event updates, individual elements or services can have message queues with many calls. These calls are ordered and sent to all parties.

To summarize, instead of having an event creating a service through a notification, a message queue takes an event or output that requires additional processing and then adds it to a long queue.

Business organizations need to become event driven and able to respond in real time. In an event-driven architecture, the events are used to trigger and communicate between decoupled services and delivered in nearly real time, and the consumers are able to respond immediately when events are produced.

An event can be a change of status or an update or deletion of an item placed in a basket on an e-commerce site. A sequence of related events represents a behavior or a status. For example, for an item purchased, its price and a delivery address, in this case, the events will present the identifiers, such as sending a notification that an order has been shipped.

Let's examine the following diagram. The event router links the different services and is used as a route for sending and receiving messages.

When an event producer is generated, the event router will execute a response to the first event after it sends it as input to the appropriate consumers. We can observe the flow of events that are handled asynchronously between managed services whose outcomes are determined as a result of a service's reaction to an event.

![](img/66aceec30d3e7dead2f71bb68a887688_62_0.png)

Figure 3.1 – Example of event-driven architecture

We have examined the basic elements of event-based architecture. We will now move on to look at **Azure Event Grid**, which allows us to easily build applications with event-based architecture, and **Azure Event Hub**, a scalable event-processing service that allows us to store and manage events.

## Exploring Event Grid and Azure Event Hubs

In this section, we will explore the basic elements of Event Grid and Event Hub.

### Event Grid

Azure Event Grid is used to easily build applications with event-based architectures.

It’s simple and interactive. To start, we select the Azure resource that we want to subscribe to, and then we indicate the event handler or the endpoint of the webhook to which we will send the event.

Event Grid supports all events from Azure services, such as storage blobs and resource groups. Event Grid also supports other external events using custom topics.

The filters provide the ability to route specific events to different endpoints. We can also multicast to multiple endpoints to ensure events are reliably streamed.

![](img/66aceec30d3e7dead2f71bb68a887688_63_0.png)

Figure 3.2 – Sources and handlers

An event source is where the event happens. Several Azure services, including Blob Storage, Media Services, IoT Hubs, and Service Bus, are automatically configured to send events. We can also use custom applications to send events. Note that custom applications don’t need to be hosted in Azure or in the cloud to use Event Grid for event distribution.

An event handler is the destination the event is sent to. The handler takes some further action to process the event. Several Azure services, such as **Azure Functions**, **Logic Apps**, **Queue Storage**, or any event hubs, are automatically configured to handle events. We can also use any webhook to handle events. The webhook doesn’t need to be hosted in Azure or in the cloud to handle events.

![](img/66aceec30d3e7dead2f71bb68a887688_64_0.png)

Figure 3.3 – Event Grid basic concepts

We defined Azure Event Grid as routing all the events from Azure services. An Event Grid is composed of two basic elements: the event sources and the event handlers. In the next section, we define Event Hubs before moving on to some exercises.

### Event Hubs

Azure Event Hubs is a scalable event processing service that stores and processes large volumes of events, data, and telemetry produced by distributed software and devices. It provides a distributed stream-processing platform with low latency and high reliability.

## Exercise 1 – publishing and subscribing from a .NET app to Event Grid events

In this exercise, we can use either Visual Studio Code or Visual Studio 2022 to create a console .NET application. We will work through the following steps:

- 1. Creating an Event Grid.
- 2. Creating a .NET Console project.
- 3. Making some modifications to the program class to be able to connect to Event Grid.
- 4. Publishing new events to the Event Grid.

We will start by creating an Event Grid topic in the next section.

### Creating an Event Grid topic

An Event Grid topic is a channel for related events (for example, *storage events* or *inventory events*).

An Event Grid topic provides an endpoint to which sources send events. A publisher creates an Event Grid topic and decides whether the event source requires one topic or multiple topics. Topics are used for collections of related events. Subscribers decide which topics to subscribe to so they can respond to particular types of events.

We will use the Azure portal to create an Event Grid topic as follows:

- 1. Browse to the Azure portal (`https://portal.azure.com`), and then sign in with your account.
- 2. In the **Search services and marketplace** text box, type `Event Grid Topic` and press *Enter*.
- 3. Select the **Event Grid Topic** result, then select **Create**. The following screenshot displays the configured settings in the **Basics** tab. We enter all the information needed to create our Event Grid topic, then select **Review + create**, and then **Create**.

![](img/66aceec30d3e7dead2f71bb68a887688_65_0.png)

Figure 3.4 – Create Topic in Event Grid

- 4. We will now deploy the Azure Event Grid Viewer to a web application.

We have created an Event Grid topic. In the next section, we will create a web application in order to deploy the Azure Event Grid viewer.
