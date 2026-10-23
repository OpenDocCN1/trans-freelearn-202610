
## DNS Routing

At its core, Route 53 is crucial to translating human-readable domain names (such as `www.example.com`) into IP addresses that computers use to connect. Amazon Route 53 responds to DNS queries using a global network of authoritative DNS servers, which reduces latency by routing each query to the nearest DNS server. This approach ensures that end-user applications running on AWS are highly available and reachable.

To effectively use Route 53, you must create hosted zones. This is a container with information on how to route traffic for a domain (such as `example.com`) and its subdomains (such as `www.example.com`, `shop.example.com`, etc.). There are two types of hosted zones, which are discussed in the following sub-sections.

### Public Hosted Zone

A **public hosted zone** is what you use when you want your domain to be accessible over the internet. When you create a publicly hosted zone, Amazon Route 53 publishes the DNS records you create in that zone to its DNS servers worldwide. This allows users anywhere on the internet to find your domain and connect to it.

### Private Hosted Zone

A **private hosted zone** routes traffic within one or more **Amazon Virtual Private Clouds (VPCs)**. This type of zone is not accessible from the internet. It’s useful for managing the DNS for internal network resources. For example, you might have an internal application within your AWS infrastructure that should only be accessible to other services or users within the same VPC. Using a private hosted zone, you can create DNS records that are only resolvable within that VPC.

Records in a private hosted zone can efficiently point to any IP address, not just addresses for AWS resources. This efficiency allows you to use private DNS names for your internal resources, making it easier to manage network administration and resource access within your AWS environment.

You can also associate a private hosted zone with more than one VPC in the same or different AWS Regions. This setup is beneficial for complex networking scenarios, such as creating a consistent DNS namespace across your development, testing, and production environments.

In summary, public-hosted zones are intended for domains that need to be accessible on the internet, while private-hosted zones are used for domains that need to be resolved within one or more private networks (VPCs). Both hosted zones are managed through the Route 53 console or API and can contain various DNS records, such as Address Record (A), IPv6 Address Record (AAAA), Canonical Name Record (CNAME), Mail Exchange Record (MX), Pointer Record (PTR), etc.

The next section dives deeper into the application of the Route 53 service.

## Amazon Route 53 in Practice

Consider a practical scenario: you are deploying a global e-commerce platform on AWS. The platform will have users, and you need to ensure it is always available, secure, and performs optimally. Route 53 can be instrumental in this scenario, as depicted in Figure 9.10:

![](img/34ff9b5b5a734dae3450ab701e223c73_369_0.png)

Figure 9.10: E-commerce application architecture

In the context of this architecture diagram, Route 53 would be the entry point for user requests, which it would then route to the Amazon CloudFront distribution for edge-cached content or to the Elastic Load Balancer in the appropriate availability zone. The Elastic Load Balancer would then distribute incoming application traffic across multiple targets, such as EC2 instances, containers, and IP addresses, within the VPC.

The Elastic Load Balancer automatically adjusts to the incoming application traffic, distributing it across multiple targets in one or more availability zones, which reduces the risk of overloading a single resource and potentially causing a service disruption.

Furthermore, Route 53’s integration with AWS services, such as CloudFront and the Elastic Load Balancer, allows for a seamless data flow, essential for maintaining operational resilience and providing a smooth user experience. By leveraging AWS’s global network infrastructure, Route 53 helps to ensure that the e-commerce application remains accessible and responsive, no matter where the users are located or how much demand is placed on the system. The next section dives deeper into traffic management with Route 53.

## Managing Global Traffic with Amazon Route 53

Amazon Route 53, with its robust set of features, plays a pivotal role in managing this global traffic, ensuring that user requests are intelligently routed to the most appropriate endpoints. To manage traffic flow across your applications, you can configure the following key routing policies with Route 53:

- **Latency-based routing:** This allows you to route traffic to the region with the lowest latency for your end users. Route 53 automatically responds to DNS queries with the IP address of the AWS Region, which provides the fastest response. This is ideal for global services where user experience depends on response times.
- **Geolocation routing:** Route 53 routes traffic based on the geographic location of your users. This feature is useful for delivering localized content, adhering to legal requirements, or balancing traffic across different regions.
- **Geoproximity routing (with traffic flow):** Geoproximity routing lets you route traffic based on the geographic location of your resources and, optionally, shift traffic from resources in one location to resources in another. Using a bias value, you can adjust the size of the geographic region from which traffic is sent to a resource.
- **Weighted routing:** This allows you to assign weights to different resources and specify the proportion of traffic to be routed to each resource. It is particularly useful for A/B testing and gradually shifting traffic from one endpoint to another.
- **Failover routing:** For applications requiring high availability, failover routing can be configured to route traffic to a secondary site if the primary site fails. This is critical for disaster recovery strategies.

In the next section, you will proceed with the project exercise for our client, Todo Plus Limited. You are now at a stage where we can incorporate the CloudFront distribution point to make the application accessible from the internet via a secure connection.

## Project Tasks – Deploying AWS CloudFront Services and Updating Website Application Code to Integrate with Backend Services

In the next set of project tasks, you will complete the Todo List application deployment, incorporating a CloudFront distribution to enable seamless and fast access to the application. You will update the frontend website, which now incorporates integration with Amazon Cognito, which you set up in Chapter 2, Securing Access with AWS Identity and Access Management.

The following diagram depicts the architectural diagram of the Todo List application as a complete solution. In addition to modifying the application code files, you will also be configuring the AWS CloudFront distribution.

![](img/34ff9b5b5a734dae3450ab701e223c73_371_0.png)

Figure 9.11: Todo List application architecture – incorporating a CloudFront distribution

The following specific tasks will be performed in the upcoming project exercises:

- **Frontend website and application code updates:** You will update the frontend website to integrate with the Amazon Cognito service to enable sign-up and sign-in capabilities. In addition, you will update the backend application code.
- **Rest API authorizer:** You will also update the API resource to use a newly created Cognito authorizer so that signed-in users can access the application.
- **Create a CloudFront distribution:** You must create an Amazon CloudFront distribution to connect to your Amazon S3 static website. Next, you will create an origin and behavior to connect to the API Gateway URL. This will allow all HTTP methods to access and interface with the application data in the backend.

### Technical Requirements

To complete the project exercises in this chapter, you must access the AWS development account you created, ensuring you log in as the IAM user “Alice.”

You must also download the updated frontend website and backend application code, as these have been updated to incorporate the various resources you have been creating on AWS. For example, the frontend application code now has an integration component for Amazon Cognito, allowing users to sign up and sign into the application.
