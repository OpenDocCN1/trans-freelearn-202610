
![](img/34ff9b5b5a734dae3450ab701e223c73_269_0.png)

ALBs can direct traffic based on the type of traffic and content, including the following:

- **Host-based routing:** Traffic is forwarded based on the host field in the HTTP header.
- **Path-based routing:** Traffic is forwarded based on the URL, enabling you to split traffic across different services or components of your application.
- **Request attributes** (such as X-Forwarded-For headers): Traffic is forwarded based on the fields in the request header, including query parameters, source IP address, and so on.
- **Redirection:** You can redirect requests from one URL to another.
- **Lambda functions:** Traffic can be directed to Lambda functions.
- **Containerized application:** Traffic can be directed to container tasks and much more.

ALBs also offer application security through encryption over SSL/TLS protocols, ensuring that data is encrypted in transit.

Next, let us look at Network Load Balancers.

## Network Load Balancers (NLBs)

NLBs operate at layer 4 of the OSI model. This load balancer is designed to distribute network traffic over **TCP, UDP, TCP_UDP**, and TLS protocols. NLBs can **handle millions of requests per second**, cope with **traffic volatility**, and **support static IP addresses** for the load balancers. It is also possible to assign an Elastic IP address per subnet for the load balancer.

NLBs use a flow-based routing mechanism, meaning that network packets follow the same route once a connection has been established. This means NLBs efficiently handle TCP, UDP, and TLS traffic. NLBs also support connectivity between VPCs over peering connections and from on-premises connections over AWS Direct Connect. NLBs are better suited to network-based applications, monitoring tools, IoT telemetry data, or media streaming.

As with ALBs, you must configure one or more target groups for your NLBs, which will host targets such as EC2 instances, microservices, and containers. Furthermore, a target can belong to more than one target group. A key differentiator, however, is that your listeners must be configured to listen to network protocols such as TCP, TLS, UDP, and TCP_UDP.

Some additional features include the following:

- **TLS offloading:** The NLB handles all TLS termination tasks and preserves the source IP address for your applications. This frees up your backend web servers to focus on answering client requests rather than using precious compute power to encrypt and decrypt traffic.
- **Sticky sessions:** These ensure that requests are routed from a client and continue to be delivered to a specific target.
- **Preserve source IP address:** This ensures that the backend applications gain visibility of the source client’s IP address.
- **DNS failover:** This ensures that traffic is routed to other nodes in another Availability Zone if there are no healthy targets in a particular zone or if the load balancer node in that zone has failed. This failover and redirection are handled by Amazon Route53, which we will discuss later in this study guide.

Next, let us discuss Gateway Load Balancers.

## Gateway Load Balancers (GWLBs)

Network traffic into and out of your applications must often be inspected and analyzed before being permitted both inbound and outbound. Companies often deploy third-party firewall solutions, **intrusion detection/prevention systems (IDSs/IPSs),** and DDoS protection appliances.

Many of these appliances are offered today on the AWS Marketplace and are easily deployable into your AWS accounts. Customers choose these appliances because of their features and rich network protection capabilities.

Managing traffic into and out of these appliances requires ensuring those services’ high availability and minimizing downtime. You want these network tools to be accessible and available consistently and be capable of handling massive amounts of traffic and performing security processes as traffic enters and leaves your network and resources.

AWS GWLBs address these issues. GWLBs are designed to help you deploy, manage, and scale your third-party virtual appliances. A GWLB is comprised of a gateway service and a load balancer service.

As depicted in the diagram in Figure 7.4, when traffic from a source is sent to a destination, its packets need to be first inspected by your virtual appliance.

The traffic first passes through the layer 3 Gateway component, representing the next hop in the route with no-packet rewrites. Once the GWLB receives the traffic, it acts as a layer 4 load balancer, offering typical features such as scaling and flow-stickiness, which gives the appliances visibility of traffic in both directions and acts on it.

It also performs health checks. Should an appliance fail, traffic can be rerouted to another appliance. The original traffic is also encapsulated in a layer 3 header using the Geneve (Generic Network Virtualization Encapsulation) protocol. This ensures that the layer 3 packet is seen without any changes. The appliance sees the packet as is, can decapsulate and inspect it, and take appropriate action to re-send it to the destination. The key benefit here is that the source and destination application need not be aware of the GWLB, ensuring that no application layer changes need to be made.

![](img/34ff9b5b5a734dae3450ab701e223c73_271_0.png)

Figure 7.4 – GWLB

GWLBs are designed to offer fault tolerance, scalability, and high availability to manage your fleet of third-party appliances on AWS.

Finally, let us briefly discuss the Classic Load Balancer.

## Classic Load Balancer (CLB)

The Classic Load Balancer is a previous-generation load balancer and one that AWS does not recommend for any modern production-level environments of applications. The CLB, as depicted in the following screenshot, can support both layer 7 and layer 4 traffic but lacks many of the advanced features and robustness of the ALB and the NLB.

![](img/34ff9b5b5a734dae3450ab701e223c73_272_0.png)

Figure 7.5 – Setting up a CLB

CLBs support SSL termination, as depicted in the diagram in Figure 7.5, the offloading of encryption tasks to the load balancer, and the management of SSL certificates. There is also support for both IPv4 and IPv6 traffic.

In this section, we examined AWS Elastic Load Balancers in detail and learned how you can build your application architecture to offer high availability by handling and distributing traffic to resources hosted in your AWS account. In the next section, you will learn more about scalability, a vital ingredient in enabling organizations to design elastic solutions, ensuring consistency, reliability, accessibility, and cost control. Auto Scaling for EC2 instances is discussed in the next section.

## Scaling Your Application with Amazon Auto Scaling

While load balancing enables you to distribute traffic across multiple EC2 instances in different Availability Zones, ensuring high availability, defining how many instances you need to support your user base is a crucial component of your overall architecture design.

To ensure that the **Todo List** application you will be deploying on AWS can handle end user demand, you must ensure that your EC2 fleet has sufficient nodes and is of the required capacity. Often, it can be difficult to predict demand, which, in the past, has meant that companies would follow the concept of **peak load provisioning**. This is where you procure the necessary infrastructure and resources to handle your peak load demand. Often, this results in wasted resources when demand is low, and you have resources sitting idle.

The Amazon Auto Scaling service (whether for your EC2 instances, RDS databases, or other services that offer auto-scaling capability) addresses this issue by deploying additional instances and nodes as your demand increases and terminating those resources when demand drops. This not only allows you to control costs but also fulfills the requirement of the Sustainability pillar of the Well-Architected Framework.

### What Is Auto Scaling for EC2?

Amazon EC2 Auto Scaling ensures that your application always has enough EC2 instances to handle the workload. EC2 instances are grouped together into **Auto Scaling groups**, and you can set a minimum number of instances for each group. Amazon EC2 Auto Scaling will never allow the group size to drop below this threshold. You can also limit the maximum number of instances in a group, which is handy to ensure that some rogue processes do not cause the Auto Scaling service to deploy unnecessary instances.

If you specify the required (desired) capacity when establishing the group, the Auto Scaling service will ensure your group has the specified number of instances. The service can automatically launch and terminate instances based on the scaling parameters you define in response to spikes and dips in application usage.

Several AWS services can facilitate the auto-scaling of infrastructure elements and reduce the administrative burden of scaling. Amazon CloudWatch, a monitoring service, acts as a mediator between the two, providing data and insights to monitor your application and infrastructure and adapt to performance or resource usage changes.
