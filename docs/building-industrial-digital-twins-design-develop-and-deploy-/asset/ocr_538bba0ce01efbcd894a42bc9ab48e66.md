HxDR provides Digital Twin capabilities for building structures on top of AWS. Some of
the characteristics of such a Digital Twin solution are outlined here:

+   - High computational complexity to mix spatial data from physical structures to
+   create a Digital Twin. The ability to grow the computational capacity elastically as
+   the scale and precision of the model grows makes the public cloud suitable for it.

+   - A high-fidelity Digital Twin requires a lot of data about its physical structures.
+   Storing and making this data available is demanding due to the amount of data.
+   Growth in storage can be on-demand or on the public cloud. HxDR uses AWS S3
+   for such elastic storage.

+   - The data may be collected globally, depending upon the problem domain of the
+   Digital Twin. Usually, a public cloud platform has points of presence in multiple
+   locations as well as edge locations.

+   - The security and sensitivity of the data collected for Digital Twins may require
+   additional security services from the underlying cloud platform.

Vertex is another provider of capabilities for Digital Twins, on top of the AWS platform
(see `https://aws.amazon.com/iot/solutions/VertexDigitalTwin/`). Bosch
IoT Things on AWS can connect to devices using MQTT and **Advanced Message Queuing**
**Protocol (AMQP)** and help to manage Digital Twins (see `https://aws.amazon.com/`
`marketplace/pp/BoschIO-GmbH-Bosch-IoT-Things/B07DTJK8MV`).

In summary, we see that a public cloud provider such as AWS can be used to build
Digital Twin capabilities from the ground up using a combination of IoT Core and other
services or specialized offerings from ISVs’ partners, which can be leveraged to get started.
However, Amazon lacks a complete Digital Twin solution of its own.

Let’s next explore the capabilities provided by Oracle in this space.

# # Oracle IoT applications

Let’s now look at Oracle’s capabilities. Oracle is known for **enterprise resource planning**
(ERP) and technology solutions that are used by several mid-to-large-sized enterprises
globally. When a company is looking for IoT and Digital Twin solutions, the current
provider of the ERP and manufacturing applications may influence that decision. Oracle
and SAP are the two main global providers of business applications such as ERP, **customer**
**relationship management (CRM)**, or **human capital management (HCM)**. Other
providers in this space are `Salesforce.com`, Workday, Infor, and so on. Some large and
global companies may have a combination of Oracle, SAP, and `Salesforce.com`, either
in different lines of business or due to merger and acquisition activities. Oracle’s business
applications are available via the cloud as SaaS offerings since 2012. Oracle launched its
public cloud platform later, and its current offering is called OCI.