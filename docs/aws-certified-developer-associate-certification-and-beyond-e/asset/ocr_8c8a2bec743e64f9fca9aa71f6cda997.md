In the case of our fictitious client, **TodoPlus Limited**, Amazon CloudFront is a service that can offer all the features for their new upcoming **Todo List** application. Amazon CloudFront will offer both security features and low latency access with improved performance specifically for any static content that the application hosts. These could include training videos on using the application so that end users do not experience lag or buffering issues.

In addition, with support for protecting against DDoS attacks and application firewalling, TodoPlus Limited can offer higher levels of security for the application, ensuring its reliability and high availability. In the next section, you will learn more about these features in detail.

# Incorporating CloudFront into Your Application Architecture

The following diagram illustrates a simple architecture that uses CloudFront to create distribution endpoints and offer a CDN service.

![](bbox=[0.15, 0.375, 0.82, 0.625])

Figure 9.1: Simple architecture using CloudFront to cache static web pages

The critical components of this architecture are as follows:

+   - **Client browser**: The user's web browser requests to view a website or web application.
+   - **Route 53**: This service is a DNS web service. It translates friendly domain names such as www.example.com into IP addresses that computers use to connect. Route 53 also provides traffic routing services, which can direct users to the nearest CloudFront distribution point for faster content delivery. Amazon Route 53 is discussed in detail later in this chapter.