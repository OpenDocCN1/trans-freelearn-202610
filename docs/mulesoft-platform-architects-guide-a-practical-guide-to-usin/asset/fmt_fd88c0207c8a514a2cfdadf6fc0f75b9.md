
# Integration Architecture Evolution

## The Decline of Traditional ESBs

**Need for Agility**: Modern businesses require agility to quickly adapt to changing market conditions. Traditional `ESBs`, with their heavyweight and centralized nature, could hinder the agility of development and deployment processes.

**Service Mesh and API Gateways**: Newer approaches, like `Service Mesh` and `API Gateways`, emerged as alternatives that were more focused on specific aspects of communication and often provided a more flexible and decentralized architecture.

**Evolution of Integration Patterns**: Event-driven architectures and messaging systems gained popularity as alternative approaches to integration. These architectures often focused on lightweight communication between services and embraced more decentralized and scalable patterns.

**Rise of Cloud-Native Technologies**: The rise of `Cloud-Native` technologies, containerization, and serverless computing shifted the focus toward more modular and scalable solutions. `ESBs` were designed in an era before these technologies and faced challenges in adapting to the new paradigm.

**API-Centric Approaches**: Organizations increasingly adopted `API-Centric` approaches to integration. Technologies like `RESTful APIs` and lightweight messaging became more prevalent for connecting applications and services.

While traditional `ESBs` have lost some of their popularity, the concept of integration remains crucial. Organizations have just transitioned to a more modern and agile integration approach allowing them to align with the evolving landscape of technology and business requirements.

## Service-Oriented Architecture (SOA)

About the same time `ESB` platforms were being introduced to the enterprise IT discussions, `Service-oriented Architecture (SOA)` began to gain popularity. In the late 1990s and early 2000s, enterprise applications were being built as web applications using n-tier designs and leveraging patterns such as `Model-View-Controller`. Browser, `Java`, and Microsoft based front ends were handling thin client `UX` functionality, and business logic was developed and run on `Java` and Microsoft applications running on a web server and using database connectors to run `SQL` queries and stored procedures on a normalized relational database management system (`RDBMS`) running on a different server.

Enterprises had just made it past the `Y2K` crises and businesses began a serious movement away from monolithic mainframe-based systems. `SOA` was a brand-new approach requiring a major paradigm shift which focused on developing applications from multiple distinct services.

Like `ESB`, `SOA` was presented as an architectural pattern and did not come with generally agreed upon protocols or industry standards. `SOA` is often mistakenly exclusively associated with `SOAP Web Services`. However, `SOAP Web Services` is a messaging protocol defining how to describe interacting with an object in another system. `SOA` can be implemented using `SOAP`-based services and well-structured `XML` or using `RESTful` web services or both. `SOAP` uses the `Web Services Definition Language (WSDL)` as the standard for describing the functionality offered by the web service.

`SOA` is generally defined as having 4 main components:

*   Service Provider
*   Service Broker
*   Service Registry
*   Service Consumer

Given that an Enterprise Service Bus (`ESB`) is implemented as a platform enabling and facilitating services across a common middleware layer, `SOA` is often implemented using an `ESB` platform. Also, it is worth mentioning that `Universal Description, Discovery, and Integration (UDDI)`, the Service Registry mentioned above, is in one sense, the grandfather of API Portals. Its adoption however was limited in comparison to other directories which followed.

## Representational State Transfer (REST Services)

`Representational State Transfer (REST)` is an architectural approach to building network-based systems defined by Dr. Roy Fielding primarily in [*Chapter 5*](#B19922_05.xhtml#_idTextAnchor090) of his dissertation “*Architectural Styles and the Design of Network-based Software Architectures”*, published in 2000. It is often abused as simply a protocol based on `JSON` message structure and `HTTP` verbs and in fact, in many cases seems to have taken on a life of its own.

The most common imagery used to describe Fielding’s vision of `RESTful` Web Services is, appropriately enough, a web site. Each page represents a particular state of the system, responding to parameters and requests. And each page contains knowledge of where the consumer may wish to navigate next. For example, an invoice page may be requested for a specific customer. The resulting page may be a list of products on an invoice. Navigation here may be to a specific product on the invoice. From there the consumer may be able to navigate to the quantity of the product in the inventory. And from the quantity, navigation to a backorder page where additional product could be purchased. The message payload should include hyper-links indicating valid navigation the API consumer may wish to take.

This network-based approach provided the foundation for an integration approach which MuleSoft refers to as the `Application Network`. `REST` provides a common mechanism, and indeed a language, with which to interact and integrate with the data and processes of an organization’s systems and applications, and to do so in a reusable way. This `Application Network` is discussed in more detail in [*Chapter 4*](#B19922_04.xhtml#_idTextAnchor074) of this book.

Whereas `SOAP` can be used over any transport layer, `RESTful` Web Services operate over `HTTP` and typically use the verbs defined by the protocol: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`. Like `SOAP`, `RESTful` web services initially used `Swagger` as the standard for describing the functionality of these services. `Swagger` eventually became `Open API Specification (OAS)`; you can think of this as `Swagger 2.0`. `RAML` was also introduced as a standard for describing `REST` functionality as well.

## iPaaS

Whether an organization has point-to-point, or the latest `ESB` technology, or some combination, one thing they all had in common was the need to acquire compute resources, deploy servers to the data center, install and maintain software, and upgrade to newer versions of the software while trying to avoid as much down time as possible. Enter the `iPaaS`. With the `iPaaS`, organizations can fast track the implementation of `RESTful APIs` and create re-usable building blocks. These building blocks can operate on different compute resources running on-premise as well as in the cloud.

In 2002 Jeff Bezos issued his now famous `API Mandate` memo and the race to the cloud was on. As described earlier in the section on `SOA`, enterprises were moving away from the monolithic system. They were also beginning to move away from their data centers and into the cloud.

This move to the cloud was a gradual move for some organizations. One major energy organization, for example, began by moving away from allocating specific, individual servers to individual projects and towards virtual computing to cut down on dock-to-rack time. The virtual computing was still running on servers and hardware, operating inside the company’s data center and still required operations and maintenance teams to manage the compute, memory, and storage allocations. This also meant the operations team had to stay ahead of new hardware requests and production system growth. And when development or testing environments were idle, they couldn’t easily reallocate the resources for other purposes.
