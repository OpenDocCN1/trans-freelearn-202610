On the other hand, weak schemas are more adaptive and can be described in JSON or XML format. Weak schemas use mapping techniques to map JSON or XML data to event objects; this is as long as we don’t change the field names and their semantic meaning. A weak schema helps us to keep the latest version of the event, as old events are now convertible to the latest event. A weak schema is the recommended approach for handling event source systems. Other approaches include versioning the event store by replacing the old events with the new events while keeping backups of the old event store for auditing.

With a good understanding of the different versioning techniques that can be applied in a microservices architecture, you can make sure that the system will continue to operate without any disruption even when breaking changes are deployed. In the next section, we will explore the role of service meshes in simplifying and scaling inter-process communication between microservices.

# Service meshes

A service mesh provides an abstraction layer on top of microservices to facilitate inter-process communication. The idea behind a service mesh is to decouple communication concerns from the implementation of microservices and handle them in a consistent manner across the microservices architecture. The following diagram depicts a simple service mesh architecture for microservices:

![](bbox=[0.19, 0.57, 0.77, 0.87])

Figure 6.14 – A service mesh