# OpenESB

**OpenESB** is an open source **Java Business Integration (JBI)** standard-centric Enterprise Service Bus, originally from Sun Microsystems, now Oracle Inc. JBI is built on a web services model and provides a pluggable architecture for a container that hosts service producer and consumer components. Services connect to the container via binding components or can be hosted inside the container as part of a service engine. The services model used is Web Services Description Language (WSDL) 2.0. Open ESB consists of a runtime, a design time, and a management console. The runtime consists of a lightweight JBI core and several components. OpenESB comes with several components for data transformation, orchestration, and connectivity. There is support for HTTP and web services, JMS, databases, MQ Series, SAP, IMS, HL7, and others. Logic can be expressed in BPEL, EJBs, or POJOs.

In the rest of this chapter, we will be looking at the following SOA integration patterns. Please note that Apache Synapse has been shown as the ESB in the diagrams, but it can be replaced by any of the ESBs we have discussed in this chapter:

+   - Protocol bridging
+   - External authentication and authorization
+   - Dynamic routing combined with auditing
+   - **Event Driven Architecture (EDA)** with **Master Data Management (MDM)**
+   - Push and pull
+   - Fault tolerant autoscaling with dynamic load balancing

# Protocol bridging

**Protocol bridging** refers to the request coming on one transport protocol, and being sent in for processing on a different protocol. For example, the corporate policy may dictate that all incoming traffic should be on HTTPS. Some web services may exist that are exposed only via a transport such as JMX. In such a case, there should be a way of bridging from HTTPS to JMX. Furthermore, the backend web services, which may be deployed on Axis2, may be highly secured within a **High Security Zone (HSZ)**, and the policies may dictate that only certain calls from the **Demilitarized Zone (DMZ)** can be made to those web services. We may also implement message validation and request throttling outside the highly secured LAN. This scenario is shown in the following figure. External web service clients talk to the endpoints on the ESB in the DMZ. This ESB will run validations and other Qualities of Service (QoS) functionality on these incoming messages, and if those messages meet the requirements, it will switch to a protocol that is compatible with the backend service deployment, and send the message into the LAN.