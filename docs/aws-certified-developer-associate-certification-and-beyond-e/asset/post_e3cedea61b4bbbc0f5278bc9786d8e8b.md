
## Configuring HTTPS for Traffic between CloudFront and Custom Origin

A custom origin can be an Elastic Load Balancer, an EC2 instance, or some other origin. The configuration requires you to change the **Origin Protocol Policy** setting for the origin to either **HTTPS Only** or **Match Viewer**. With Match Viewer, the communication depends on the protocol the end user/viewer uses. So, for example, if the end users use HTTPS to request content, CloudFront will use HTTPS in its request to the origin.

In addition, you will need to install a **Secure Sockets Layer (SSL)**/**Transport Layer Security (TLS)** certificate to your custom origin.

## Configuring HTTPS for Traffic between CloudFront and an Amazon S3 Origin

With Amazon S3, if you configure your bucket as a static website, you can only use the HTTP protocol to communicate between CloudFront and the S3 bucket. Note that the **HTTPS** and **Match Viewer** options are grayed out in this configuration, as shown in the following screenshot:

![](img/34ff9b5b5a734dae3450ab701e223c73_363_0.png)

Figure 9.7: Configuring CloudFront distribution for an Amazon S3 origin

If you configure your Amazon S3 bucket with a standard API endpoint (you do not configure with static website hosting), then you can serve content over HTTPS. In this case, CloudFront will forward the request to S3 using the **Match Value** settings so that the protocol used by the viewer is the same one that is forwarded to the origin. To ensure traffic is sent over HTTPS, you must set the Viewer Protocol Policy to **Redirect HTTP to HTTPS** or **HTTPS Only.**

Next, you will learn about another encryption solution known as **field-level encryption**.

## *Field-Level Encryption*

CloudFront’s field-level encryption adds a layer of security by enabling encryption at the field level within an HTTP form posted to CloudFront. It encrypts POST data with specific encryption keys before forwarding it to the origin, ensuring sensitive information, such as credit card details, is securely transmitted.

In the next section, you will learn how to use the same distribution to serve different types of content from different origins and services. This can help organizations build complex solutions that comprise differing content types.

## Multiple Origins and Origin Groups

CloudFront allows you to configure multiple origins within the same distribution. This is useful for serving different types of content from different locations or services. For example, static content could be served from an S3 bucket, while dynamic content could be served from an EC2 instance or an Elastic Load Balancer.

Origin groups allow you to set multiple origins for your distribution. You can define static content to be served from an Amazon S3 bucket and any dynamic content from an Application Load Balancer serving backend applications processed from a fleet of EC2 instances.

In addition, you can also configure failover features. For example, you can create an origin group with a primary origin and a secondary origin. If the primary origin fails (becomes unavailable), CloudFront will automatically switch to the secondary origin in the group. This provides high availability and fault tolerance for your applications. This can be applicable in several use cases, such as if the primary site is down or because of timeout issues due to latency.

Consider an e-commerce site with an S3 bucket that stores product images (primary origin) and a secondary origin configured to serve images from an EC2 instance. If the S3 service experiences an outage, CloudFront will automatically route requests to the EC2 instance, ensuring that users can still view product images.

Monitoring your CloudFront distribution is a key operational task that you must perform to ensure the reliability of your application. The next section looks at Amazon CloudFront’s logging capabilities.

## Real-Time Logs

CloudFront provides real-time logs that give you visibility into user requests made to your distribution. The logs can be delivered to Amazon Kinesis Data Streams, where you can analyze and visualize the data in real time. This feature allows you to respond quickly to changes in viewer traffic, troubleshoot issues, and improve the performance and security of your content delivery.

When using these features, consider their compatibility with your use case and the impact on pricing. The real-time logs, for example, can generate large amounts of data that might require additional processing and storage, potentially incurring further costs. Field-level encryption provides enhanced security, but it also requires careful key management and might add complexity to your application logic. Origin groups improve availability and fault tolerance, which can be critical for maintaining a high-quality user experience.

By using CloudFront in conjunction with S3 and Route 53, the architecture ensures a robust, secure, and efficient way to serve static content to end users with the benefits of a CDN.

Having explored how Amazon CloudFront builds powerful CDNs, the next section discusses the enhancement of these networks with Lambda@Edge services, where you can execute serverless functions to tailor and optimize your content delivery.

## Incorporating Lambda@Edge Services for Amazon CloudFront

**Lambda@Edge** is a feature within AWS that allows you to run serverless functions in response to CloudFront events. It brings compute power closer to the user by executing functions in AWS locations globally, reducing latency and improving performance for dynamic web applications. The following section outlines how it enhances the capabilities of Amazon CloudFront.

## Customization at the Edge

With **Lambda@Edge**, you can execute functions that customize the content delivered through CloudFront. You can run code that modifies HTTP responses, customize the user experience based on device, location, or language, or even implement specific security protocols. *Figure 9.8* depicts an example function:

![](img/34ff9b5b5a734dae3450ab701e223c73_366_0.png)

Lambda@Edge functions are serverless, so you don’t have to manage servers, provision compute resources, or scale them. This allows easy scaling and maintenance because AWS handles the infrastructure.

You trigger Lambda@Edge functions from CloudFront events, such as when a viewer request is made or when CloudFront receives a response from the origin server. This allows for real-time content modification and decision-making at the network’s edge. Lambda@Edge reduces the need for requests to travel back to the origin server, thus decreasing latency and improving the speed of content delivery.

## Simplified Development and Deployment

Lambda@Edge integrates seamlessly with AWS CloudFormation and the AWS **Serverless Application Model (SAM)**, simplifying your serverless application’s creation, deployment, and management. You can develop and test your Lambda functions in a standard AWS Lambda environment and associate them with a CloudFront distribution, as illustrated in the following diagram.

![](img/34ff9b5b5a734dae3450ab701e223c73_367_0.png)

Figure 9.9: Architecture using Lambda@Edge

The use case for Lambda@Edge depicted here involves a user making a request via CloudFront. If a function needs to be executed to modify the request or generate a response, Lambda@Edge is triggered. The request can also be routed through an Application Load Balancer before being processed by a Lambda function in the AWS Cloud. The response is then delivered back to the user, potentially after being modified by another Lambda@Edge function.

This setup is typically used for applications that deliver content with low latency to a global audience and require dynamic content generation or real-time processing at the edge, closest to the end user. Lambda@Edge functions can be used for various purposes, such as website customization, security and privacy controls, intelligent routing, and image transformation. This setup helps reduce latency since the computation happens closer to the user location than the origin server, which could be geographically distant from the user.

The next section covers AWS's DNS offering with Amazon Route 53

## Deploying Custom Domains for Your Application with Amazon Route 53

In cloud computing and web applications, the role of a DNS service such as Amazon Route 53 is pivotal. It's not just about linking a domain name to an IP address; it's about orchestrating a symphony of network traffic, ensuring secure, fast, and reliable access to your applications hosted on AWS. Route 53 offers a suite of functionalities that extend far beyond typical DNS services, making it an indispensable tool in the AWS architect's toolkit.
