
# MuleSoft Government Cloud Overview

## Anypoint VPC and VPN

**Anypoint VPC**: This lets you create a virtual network that is under your control and is like a private, isolated area inside MuleSoft’s Government Cloud. A VPC allows you to create a virtual private network, with resources such as apps and APIs that are completely isolated from all other resources in Government Cloud.

**Anypoint VPN**: VPNs extend a private network and allow resources to send and receive data as if they were directly connected to the private network. Anypoint VPN guarantees the security and encryption of all data transferred over the public internet between your Anypoint VPC and your on-premises network.

An additional degree of security is added by isolating your application data from public view using Anypoint VPC. Anypoint VPN connects securely your on-premises network to your VPC, allowing you to access your information from your private network without worrying about data getting intercepted during transit. By offering secure, private data ingress and egress points, Anypoint VPC and VPN work together to increase system security and privacy.

They offer advantages, including improved network performance, data security, and more network customization options.

These tools are particularly significant in the context of Government Cloud because they address the higher security and compliance requirements that are inherent to government organizations.

## Dedicated Load Balancer in MuleSoft Government Cloud

A Government Cloud environment includes a dedicated load balancer, which is a cloud-based service that distributes incoming network traffic to multiple MuleSoft application workers in Government Cloud. This ensures that the load is distributed evenly across the environment, thereby promoting effective resource use, optimum performance, and greater availability of applications.

The following are the key benefits of the presence of a dedicated load balancer in MuleSoft Government Cloud:

*   **Improved performance**: By evenly distributing network traffic, load balancers help prevent any single application worker from becoming a bottleneck of performance.
*   **Increased reliability and availability**: The load balancer reroutes traffic to other operational servers so that your application remains available when there is a problem with an operation worker.
*   **Secure Sockets Layer (SSL) termination**: Support for SSL termination is provided by a separate load balancer. This implies that the load balancer handles SSL handshakes with a user session, which frees up resources for backend application workers and improves system performance.
*   **Supports scaling**: Dedicated load balancers also work effectively in scalable environments, dynamically distributing network loads across numerous instances as they go in and out of service.
*   **Custom domains**: You can also configure routing rules that allow traffic to be directed to specific domains or multiple domains using a dedicated load balancer.

The use of a dedicated load-balancing server is especially important to maintain high availability and performance, where mission-critical applications are frequently housed in Government Cloud.

The dedicated load balancer in Government Cloud exposes two A records to accept traffic from local private networks and public networks. One of these records is internal (`internal-<lb-name>.lb-gprod-rt.anypointdns.net`) and the other is external (`<lb-name>.lb-gprod-rt.anypointdns.net`).

## MuleSoft Government Cloud Control Plane

MuleSoft Government Cloud supports Anypoint Platform as a control plane that is FedRAMP-compliant. The control plane is where you design, deploy, share, and manage APIs and applications.

The following components are included in the control plane:

*   Anypoint Design Center to design API specifications
*   Anypoint Management Center to manage users, APIs, and integrations
*   Anypoint Exchange is a marketplace to publish and share API specifications, APIs, and integration
*   Anypoint Runtime Manager for the deployment of API and integration into the cloud or data centers

## MuleSoft Government Cloud Runtime Plane

APIs and Mule applications are deployed and made available to users on the runtime plane component of Anypoint Platform. The Mule runtime server, Anypoint connectors, and support services are part of the running plane.

## Standalone Mule Support in MuleSoft Government Cloud

MuleSoft Government Cloud deployment permits the deployment of Mule runtime engine instances into an on-premises environment (standalone Mule instances). Standalone Mule instances, and MuleSoft support for them, are outside the scope of FedRAMP. This means the customer is responsible for ensuring that all data exchanged with Salesforce complies with any applicable data protection principles, requirements, and standards. Review the MuleSoft System Security Plan for the FedRAMP scope.

Mule runtime version `4.1.x` or later and the runtime agent version `2.4.9` or later are required for FIPS compliance. Standalone Mule support enables federal, state, and local government customers to deploy standalone Mule instances inside their private clouds or on-premises environment and manage them, using the MuleSoft Government Cloud control plane.

The following components are not supported by MuleSoft Government Cloud:

*   Anypoint Analytics
*   Anypoint Code Builder
*   API Data Gateway
*   API Governance
*   Anypoint Flex Gateway
*   Anypoint Runtime Fabric
*   Anypoint Security
*   Anypoint Service Mesh

## Deployment Models for MuleSoft Government Cloud

There are two options when deploying Government Cloud. The first option is the fully managed model, where the control and runtime planes are in the cloud. The second option is to have the control plane in the cloud and the runtime plane in the customer’s data center. If the control plane is in the cloud and the runtime plane is hosted in the MuleSoft cloud or the customer’s data center, you can also use a combination of both options.

### Fully Managed Deployment Model

In this deployment model, the control and runtime planes will be managed by MuleSoft. Customers don’t have to worry about the underlying infrastructure, and it is completely FedRAMP-compliant.

`Figure 13.1` shows the architecture of the fully managed deployment model.

![Figure 13.1: The fully managed deployment model](`image/B19922_13_1.jpg`)

Figure 13.1: The fully managed deployment model

The following are the benefits of the fully managed deployment model:

*   It supports high availability, scalability, and an intelligent healing mechanism
*   99.99% uptime
*   It’s able to manage multiple Mule runtime versions
*   FedRAMP-compliant
*   Fault-tolerant
*   MuleSoft is responsible for managing the underlying infrastructure to run MuleSoft applications
*   Application isolation
*   Government Cloud can connect to on-premises data centers or private clouds using Anypoint VPN and VPC peering

The following is the primary limitation of the fully managed deployment model:

*   Latency can be experienced while connecting to on-premises data sources, services, and resources

### Hybrid Deployment Model

With the hybrid option, users can host the Mule runtime server and associated APIs in a self-managed environment and can use the MuleSoft control plane to manage the APIs. The Mule runtime server can be installed in a third-party cloud such as AWS, Microsoft Azure, GCP, OCI, or another cloud infrastructure provider, or it can run on a physical server or virtual machine.

The customer is also in charge of providing the infrastructure and the framework for high availability, failover, clustering, and load balancing when using customer-hosted runtimes.

`Figure 13.2` shows the architecture of the hybrid managed deployment model.
