
# Runtime Fabric architecture on EKS

## Network Subnet Architecture

*   `Firewall subnet`: This subnet will restrict the egress (outbound) traffic from the private subnet. By default, all outbound traffic will be allowed from the private subnet if you have associated the NAT gateway with the private subnet. If you have requirements specifying that you can connect to a few applications available on the internet, in such cases, you can allow those application domains or IP addresses in the firewall policies of the firewall subnet. This means you are not allowing all traffic from the private subnet toward the internet. The firewall subnet is optional and it mostly allows you to define stateful and stateless rules.

*   `Public subnet`: This subnet is associated with the internet gateway and allows ingress (inbound) and egress (outbound) traffic to and from the internet. The NAT gateway will be set up in the public subnet to allow only egress (outbound) traffic from the private subnet toward the internet.

Each subnet is spun up across all the Availability Zones to ensure scalable, highly available, and fault-tolerant architecture. The route table associated with each subnet is used to define the rules where the traffic will be directed from the subnet.

With EKS set up, the Kubernetes control plane (controller nodes) is automatically set up in the AWS-managed VPC. It is managed by AWS and takes care of scaling, high availability, fault tolerance, and more. It also enables EKS-managed ENI across the Availability Zones to establish communication between the control plane and worker nodes in the Kubernetes cluster.

A bastion host can be set up in the public subnet to connect the worker nodes in the private subnet.

Data stored on `etcd` is encrypted using Amazon’s `Key Management Service` (`KMS`). Controller nodes are distributed among the various Availability Zones and traffic is distributed by `Elastic Load Balancer` (`ELB`).

## Runtime Fabric architecture on EKS (self-managed Kubernetes)

Let’s take a look at the Runtime Fabric on self-managed Kubernetes architecture. This architecture and how its many components interact and coordinate are shown in the following figure:

![Figure 11.12 - Runtime Fabric architecture on EKS (self-managed Kubernetes)](image/B19922_11_12.jpg)

Figure 11.12 - Runtime Fabric architecture on EKS (self-managed Kubernetes)

Figure 11.12 shows various components and how they interact with each other:

*   The Persistence Gateway is responsible for storing object store data in a persistent data store. The application must be clustered to share the data, such as in an object store or VM queues across the replicas. The Persistence Gateway will ensure that this data doesn’t vanish if the application restarts or is deployed and recovers from any crashes.

*   The Docker Pod is the worker node that’s responsible for pulling the specific version of the Mule runtime from the Mule Docker registry.

*   The log forwarder agent will forward logs to external systems, such as Splunk, CloudWatch, ELK, or Anypoint Monitoring.

*   Two kinds of clustering can be achieved: at the infrastructure level and application level. Infrastructure clustering involves groups of nodes that work together in the cluster to run the MuleSoft applications and ensure the high availability, scalability, and fault tolerance of the architecture.

*   Application clustering can be enabled when we are deploying more than one replica of the application, and this can be enabled during application deployment. This will improve application performance and allow data to be shared across the replicas of the application.

*   Replicas of applications deployed on Runtime Fabric are fault tolerant. They are recovered automatically after the `JVM` crashes or a worker node is down.

*   Auto-scaling is supported by adding a worker node to the auto-scaling group. It can scale depending on whether the memory and `CPU` usage exceeds the configured value.

## Installing Runtime Fabric on self-managed Kubernetes

Runtime Fabric can be set up on self-managed Kubernetes using the `rtfctl` or `helm` utility.

Here is the official MuleSoft documentation for installing Runtime Fabric on self-managed Kubernetes using `rtfctl` or `helm`:

*   `rtfctl`: [`https://docs.mulesoft.com/runtime-fabric/1.13/install-self-managed`](https://docs.mulesoft.com/runtime-fabric/1.13/install-self-managed)
*   `helm`: [`https://docs.mulesoft.com/runtime-fabric/1.13/install-helm`](https://docs.mulesoft.com/runtime-fabric/1.13/install-helm)

## Shared responsibilities between the customer and MuleSoft

It is critical to understand what the customer’s and MuleSoft’s responsibilities will be in terms of managing Runtime Fabric on self-managed Kubernetes. The following diagram depicts the customer’s and MuleSoft’s responsibilities:

![Figure 11.13 - Shared responsibility model](image/B19922_11_13.jpg)

Figure 11.13 - Shared responsibility model

MuleSoft provides Runtime Fabric services, the Mule runtime, and other components for deploying and running Mule applications on self-managed Kubernetes.

The customer will be responsible for managing the Mule application, infrastructure, Kubernetes, networks, load balancers, monitoring and logging to external platforms, and so on.

## Ingress

Runtime Fabric allows you to specify a custom ingress configuration for ingress using a resource template. In a resource template, you can define the annotation, class name, and HTTP and HTTPS routing rules. Ingress is responsible for TLS termination and tunneling, load balancing, and routing. The ingress controller can be an `NGINX ingress controller`, `AWS load balancer controller`, and so on:

![Figure 11.14 - Ingress-managed load balancer](image/B19922_11_14.jpg)

Figure 11.14 - Ingress-managed load balancer

**Note**

The following documentation explains how you can configure ingress on Runtime Fabric on self-managed Kubernetes: [`https://developer.mulesoft.com/tutorials-and-howtos/runtime-fabric/runtime-fabric-aws-elastic-kubernetes-service/`](https://developer.mulesoft.com/tutorials-and-howtos/runtime-fabric/runtime-fabric-aws-elastic-kubernetes-service/).

## High availability and fault tolerance

Whenever you set up Runtime Fabric on self-managed Kubernetes, the control plane is completely managed by the cloud provider, and it is highly available to ensure business continuity and performance. A minimum of three worker nodes should be set up, as per the quorum, to ensure fault-tolerant and highly available architecture. An application can be highly available by deploying multiple replicas and clusters together.

An application that’s deployed to Runtime Fabric on self-managed Kubernetes is fault tolerant and will automatically recover from any failures or worker nodes that go down.

## Scalability

The control plane scalability of self-managed Kubernetes is managed by the cloud provider. Worker nodes can be scaled horizontally by adding or removing the nodes and can be scaled vertically by adding or removing computing resources such as `CPU` or memory.

A worker node can be added to an auto-scaling group to auto-scale in case memory or `CPU` usage increases or decreases.

An application that’s been deployed to Runtime Fabric on self-managed Kubernetes can be scaled horizontally by adding or removing the replicas and scaled vertically by increasing or decreasing the replica size. Auto-scaling is not supported for the application, but it can be scaled from the `Runtime Manager UI`, the `Mule Maven plugin`, or the `Platform API` with zero downtime.

## How will an application deployed to self-managed Kubernetes communicate with an external service for which IP whitelisting is required?

You may come across a situation where your application must communicate with external services. To establish a successful connection with external services, your application’s IP address must be whitelisted on the external services.

Here, we will consider applications that have been deployed to AWS EKS to communicate with an external service where IP whitelisting is a must.

### Scenario 1 – worker nodes running in the public subnet
