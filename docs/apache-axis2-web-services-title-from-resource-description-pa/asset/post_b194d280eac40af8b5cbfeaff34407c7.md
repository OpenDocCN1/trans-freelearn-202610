
There can also be several `parameter` elements, which are children of the `clustering` element in the `axis2.xml` file. Generally, these parameters will be specific to the `ClusteringAgent` implementation. In the default Apache Tribes-based implementation, there are several parameters that are required to initialize the Tribes Channel properly.

## org.apache.axis2.clustering.state.StateManager

This interface is responsible for handling state replication. This is an optional interface and if state replication is not required, then this interface can be omitted. The word 'state' here means the serializable values stored in the Axis2 context hierarchy which need to be kept in sync across all the members of a cluster group. Property changes (state changes) in the Axis2 context hierarchy in the node that runs this `StateManager` will be propagated to all other nodes in its group. In the default Axis2 clustering implementation, we have only enabled replication of serializable objects stored in the Apache Axis2 `ConfigurationContext`, `ServiceGroupContext`, and `ServiceContext`. Hence, if a user requires state replication, it is this user's responsibility to store these values in the proper Axis2 contexts. Generally, web services authors do not have to handle state replication, as it is handled by the clustering implementation just before request completion. This is done at the Axis2 `MessageReceivers`. However, if the developer writes his own `MessageReceiver`, he will need to call the `org.apache.axis2.clustering.state.Replicator#replicate()` method. Also, note that at any point, `org.apache.axis2.clustering.state.Replicator#replicate()` can be called, if the developer wishes to force state replication.

It is not mandatory to have a `StateManager` in a node. If we are not interested in high availability, we may disable state replication. In such a scenario, in general, the purpose of a clustered deployment is to achieve scalability. In addition, one may also enable clustering without state replication simply to utilize the group communication capabilities of the underlying **Group Communication Framework (GCF)**. In such a case, the purpose of a clustered deployment may also be management of the cluster using the underlying GCF. The implementation of this interface is set by reading the `stateManager` element in the `axis2.xml` clustering section.

## WSO2 ESB

**WSO2 ESB** is an open source Enterprise Service Bus (ESB) released by the open source middleware company, WSO2 Inc. It is shipped under the Apache Software License v2.0. The runtime of WSO2 ESB has been designed to be completely asynchronous, non-blocking, and streaming, based on the Apache Synapse mediation engine. This also means that WSO2 ESB has been built on top of Apache Axis2, as Apache Synapse is based on Apache Axis2. This ESB allows system administrators and SOA architects to simply and easily configure message routing, virtualization, intermediation, transformation, logging, task scheduling, load balancing, failover routing, event brokering, and many enterprise integration patterns.

WSO2 ESB supports many application layer protocols and messaging standards, collectively known as **transports**, including HTTP, HTTPS, e-mail, **Java Message Service (JMS)**, and **Virtual File System (VFS)**. It also supports a range of domain-specific protocols such as **Financial Information eXchange (FIX)**, **Advanced Message Queuing Protocol (AMQP)**, and **Health Level 7 (HL7)**. New transports can be easily plugged into the server due to the extensibility offered by the Axis2 transports framework. The following figure illustrates how the different transports can be plugged into the ESB and how the Apache Synapse runtime fits into the WSO2 ESB:

![](img/e2fb05de168053e6227dab034eff9353_243_0.png)

## OpenESB

**OpenESB** is an open source **Java Business Integration (JBI)** standard-centric Enterprise Service Bus, originally from Sun Microsystems, now Oracle Inc. JBI is built on a web services model and provides a pluggable architecture for a container that hosts service producer and consumer components. Services connect to the container via binding components or can be hosted inside the container as part of a service engine. The services model used is Web Services Description Language (WSDL) 2.0. OpenESB consists of a runtime, a design time, and a management console. The runtime consists of a lightweight JBI core and several components. OpenESB comes with several components for data transformation, orchestration, and connectivity. There is support for HTTP and web services, JMS, databases, MQ Series, SAP, IMS, HL7, and others. Logic can be expressed in BPEL, EJBs, or POJOs.

In the rest of this chapter, we will be looking at the following SOA integration patterns. Please note that Apache Synapse has been shown as the ESB in the diagrams, but it can be replaced by any of the ESBs we have discussed in this chapter:

- Protocol bridging
- External authentication and authorization
- Dynamic routing combined with auditing
- **Event Driven Architecture (EDA)** with **Master Data Management (MDM)**
- Push and pull
- Fault tolerant autoscaling with dynamic load balancing

## Protocol bridging

**Protocol bridging** refers to the request coming on one transport protocol, and being sent in for processing on a different protocol. For example, the corporate policy may dictate that all incoming traffic should be on HTTPS. Some web services may exist that are exposed only via a transport such as JMX. In such a case, there should be a way of bridging from HTTPS to JMX. Furthermore, the backend web services, which may be deployed on Axis2, may be highly secured within a **High Security Zone (HSZ)**, and the policies may dictate that only certain calls from the **Demilitarized Zone (DMZ)** can be made to those web services. We may also implement message validation and request throttling outside the highly secured LAN. This scenario is shown in the following figure. External web service clients talk to the endpoints on the ESB in the DMZ. This ESB will run validations and other Quality of Service (QoS) functionality on these incoming messages, and if those messages meet the requirements, it will switch to a protocol that is compatible with the backend service deployment, and send the message into the LAN.

In the following figure, we have shown another optional ESB deployment, which can further deal with things such as transformations. However, that ESB can be omitted, if required, and the request can be directly routed to Axis2. Once Axis2 receives the request, it will dispatch it to the relevant web service and send the response back. The ESB in the DMZ will again perform a protocol transformation, and send the response back to the client that originated the request.

![](img/e2fb05de168053e6227dab034eff9353_247_0.png)

## External authentication and authorization

In medium to large scale enterprise deployments, authentication and authorization are handled by an external identity management server such as WSO2 Identity Server. The web service client or the server can participate in authentication and authorization.

In these two scenarios, authentication and authorization handling can be done within a pure Axis2 instance as well, but the reason for bringing in an ESB into the picture is to handle all authentication and authorization functionality in one central place. Otherwise, if there are multiple Axis2 instances in an enterprise deployment, each of those Axis2 instances would have to separately talk to the identity management server. In addition, additional mediation steps can be introduced in the ESB, when the system evolves over time.

## Dynamic routing combined with auditing

In some enterprise deployments, there is a requirement to route the request to different Axis2 service implementations or different Axis2 servers, depending on certain criteria or other parameters such as the time of the day. For example, during the peak hours, requests may be routed to a server having more resources in terms of memory and processing power. In some cases, the requests may need more processing power, depending on the input parameter values. Requests are dynamically routed in order to optimize resource usage and maintain throughput. Note that the service contracts will not change and only the service implementation or deployment location will change. Hence this is transparent to the client.

Dynamic routing may be combined with auditing. In this case, each of the requests, along with other relevant metadata, will be asynchronously logged into a file or database for future auditing purposes.

As shown in the following figure, the Synapse ESB is used as a dynamic router and the central point for auditing. The Synapse configuration is loaded from a configuration repository and this configuration can change over the course of the day so that requests are dynamically routed to different Axis2 service implementations or Axis2 servers. A log mediator is used for logging the incoming requests and outgoing responses to a database. In the case where the Synapse configuration is changed at preconfigured times of the day, the server is put into maintenance mode using the JMX management API.
