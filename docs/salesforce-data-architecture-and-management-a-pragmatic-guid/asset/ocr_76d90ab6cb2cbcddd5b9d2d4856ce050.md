In EDA, messages or events are published to a central location; in this case, it’s the **Enterprise Messaging Bus (EMS)** and systems that are interested in those events subscribe to it. Whenever an event is published, it is published on the bus and all consumers that subscribed to it can use it. This type of architecture decouples systems that produce these events from systems that consume these events, leading to a scalable architecture. Figure 8.4 shows how EDA works:

![](bbox=[0.14, 0.225, 0.83, 0.685])

Figure 8.4 – Event-Driven Architecture

The enterprise messaging bus is part of the technology on which the Streaming API is based. This is called **CometD** and uses the messaging bus to publish and subscribe to events. The Streaming API has multiple flavors; two of the most commonly used ones are **Platform Events** and **Change Data Capture (CDC)**. The earlier flavors of the Streaming API were **Generic** and **PushTopic**, both of which required code for publishing or subscribing to an event. With Platform Events and CDC, there is more flexibility as they are supported both by code and other declarative features in Salesforce, such as Salesforce Flow or Process Builder.