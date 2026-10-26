Logs are particularly helpful in situations where some components stop pushing metrics to an event store due to an unhandled exception. This could be caused by third-party dependencies, network issues, or other unforeseen circumstances. Logs are the ultimate source of truth for the events that are happening in a system. Therefore, it’s important to enforce appropriate security guardrails to prevent any modifications after they have been created. In AWS, it’s a good practice to encrypt these logs at rest using services such as **Amazon Key Management Service (KMS)**.

+   - **Metrics:** These are the *raw data points* that reflect the performance of your systems over time and are particularly useful in alarming, trend analysis, and scenario forecasting. For on-premises deployments, organizations typically leverage third-party tools and **application performance monitoring (APM)** solutions that gather underlying infrastructure and application metrics. However, in AWS, most of the services automatically publish key metrics without any additional configurations or cost to the user. This information, which is available free of charge, can be used to define alarms for threshold breaches or mathematical analysis – for example, maximum IOPS for a disk in any 5-minute interval.

+   - **Traces:** When working with multiple microservices, a holistic view is often necessary to understand the request flow and identify bottlenecks. This allows the software teams to zoom in on how the information *traversed multiple systems* for a particular request, and identify if something didn’t go as expected. These traces are also valuable input for service maps as they can be used to visually depict the dependencies each service has and some problematic areas.

AWS’s Swiss-army knife solution for all observability needs is **Amazon CloudWatch**. CloudWatch can be used for a variety of customer needs to continuously monitor the applications and underlying infrastructure.

Having understood the three main pillars of observability, let’s see the main benefits they help achieve.

# Benefits of observability

There are multiple benefits that teams get when they make their systems observable:

+   - **Enhanced customer experience:** There is no doubt that the better you understand your system behaviors, the easier it is to maximize customer benefits from your software applications. Product managers can leverage this data to understand how a particular software application is used and carve out a future strategy for new features and enhancements that would delight their customers.

+   - **Efficient troubleshooting:** A good observability strategy leads to reduced investigation time around operational issues and outages. This allows the team to have more time for building differentiating features instead of being pulled into long-lasting investigations.

+   - **Visibility:** Observability is not only about fire-fighting operational issues but there are sometimes questions you would like to ask yourself to better understand the consumption patterns of your software application. *Where are the majority of my users coming from? How much time do they*