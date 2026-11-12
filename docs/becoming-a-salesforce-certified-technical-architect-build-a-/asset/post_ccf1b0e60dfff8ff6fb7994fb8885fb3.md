
For example, an integration architect might decide to transfer files across the integrated enterprise systems using a pub-sub approach, overlooking the impact of the file size on this approach. This could dramatically impact performance and stability.

Another similar example is when an integration architect decides to build an entire middleware on top of the core Salesforce cloud, introducing dozens of classes, objects, and components to create an orchestration engine linking several systems. In this case, the technology is used in the wrong context and creates a fragile solution that will inevitably collapse one day.

Failing to understand a technology’s details and why it has been created the way it has could lead you to make bad design decisions. Chapter 3, Core Architectural Concepts: Integration and Cryptography, helps you to understand the history behind each integration approach and technology, how they differ, and good use cases for each. This will help you determine the ideal landscape architecture for your integrated solution.

## Determine the Right Enterprise Integration Architecture Technology

Technology is the next topic to consider once you have decided on your integration approach. You need to understand the different tools available in the enterprise landscape. There are several questions you need to consider, such as the following:

- What is an Extract, Transform, Load (ETL) tool?
- What is an Enterprise Service Bus (ESB)?
- How do ETL and ESB differ from each other? And when should you pick one over the other?
- What is a reverse proxy?
- What is an API manager?
- How can you connect a cloud application to an on-premises application?

You covered most of these in Chapter 3, Core Architectural Concepts: Integration and Cryptography.

It is recommended to get some hands-on experience with an ETL tool or an ESB, or both. This will help you understand the different approaches supported by some tools.

For example, to integrate with an on-premises/behind-a-firewall application, some tools offer the ability to install a secure client (also known as a secure agent). The secure client is a small-footprint application (provided by the same company offering the middleware) that can be installed on the secure intranet (behind the firewall). It can connect directly to a local database or API using specific ports. The secure client would then communicate with the cloud-based solution via other ports, such as 80 or 443, which are used for HTTP and HTTPS, respectively, and usually are permitted by the firewall. On some occasions, you might need to ask the firewall administrator to allow communications over specific ports for specific addresses, assuming the target application has a unique URI.

The following diagram illustrates how a secure client/secure agent works for a cloud-based ETL solution (which could slightly differ from one ETL product to another):

![](img/a3b1eecf3c87c814267f2877e8a8be1a_302_0.png)

Figure 9.1 – A cloud-based middleware using a secure agent app

So, Figure 9.1 illustrates the following:

- The secure agent communicates directly with the cloud middleware, typically using ports 443 or 90. These are outbound communications, and therefore, there is no need to adjust or change the firewall rules. The agent downloads the definition of the integration jobs it is supposed to execute and stores them locally (usually encrypted).
- Based on the downloaded job info, the secure agent executes the planned jobs.
- The data is extracted, transformed, and transferred directly to the target system (in Figure 9.1, Salesforce). Data is not staged in the middleware cloud (which is usually an essential requirement for data privacy regulations).
- Based on pre-defined schedules, the secure agent would send logging and monitoring data back to the cloud-based middleware.

Understanding these details will help you present the full end-to-end solution during the review board. Now, you will explore the next objective a CTA should meet in this domain.

## Design Your Integration Interface Using the Right Integration Pattern

Integration patterns describe standard ways to integrate Salesforce with other systems. The principles themselves are not technology-/platform-specific, as they can be applied to any other technologies and platforms. You need to fully understand the purposes of these integration patterns, common use cases, and the Salesforce technologies used to implement them. While creating your end-to-end solution, you need to select which pattern to use based on the use case.

> **Note**
> You can find more details about the patterns at the following link: https://packt.link/bw3nY.

You will cover the following six patterns:

- Remote process invocation (RPI): request and reply
- RPI: fire and forget
- Batch data synchronization
- Remote Call-in
- UI update based on changes in data
- Data virtualization

Next, you will dive deeper into the details of these patterns and their characteristics.

## Remote Process Invocation: Request and Reply

Here is some high-level info about this pattern:

- **Description**: This pattern is used to invoke a remote web service and wait for its response. The system is in a blocking state during this waiting time. In a classic single-threaded desktop application, you will notice that the application is frozen while waiting for a response. This is not the case with web applications (such as Salesforce), but it is common to see a progress indicator spinning while waiting for a response.
- **Timing**: This pattern is synchronous, sometimes referred to as real-time.
- **Operational layer**: This pattern operates on both the data layer and the business logic layer, depending on the implementation. The name of this pattern could be confusing. RPI integrations operate on the business logic layer, such as invoking a remote weather forecasting web service, passing the location and date, and receiving the expected weather forecast for the given date. But in addition, this pattern can also be used for synchronous integrations on the data level, such as invoking a web service to copy a local record to a remote system and waiting until the transaction is committed and a confirmation is returned. The entire data insertion transaction here is considered an RPI.
- **Key use cases**: Invoking a remote web service, executing an operation, and waiting for the result. This could include passing a data structure that gets copied to the remote system.
- **Relevant Salesforce features:** Callouts could be considered a relevant feature, such as callouts invoked from a Visualforce page or a Lightning component. This does not include an asynchronous callout invoked using future methods or similar capabilities. Salesforce External Services called from Flows or Apex is also relevant to this pattern.

> Note
> You can find more details at the following link: https://packt.link/B7jk7.

## Remote Process Invocation: Fire and Forget

Here is some high-level info about this pattern:

- **Description:** This pattern is used to invoke a remote web service and continue with other processes without waiting for a response. The system is not blocked from doing other operations. In a classic single-threaded desktop application, the application would invoke a remote web service and continue with other operations.
- **Timing:** This pattern is asynchronous. In some cases, it can be considered near real-time.
- **Operational layer:** This pattern operates on both the data layer and the business logic layer, similar to the RPI request and reply pattern.
- **Key use cases:** Invoking a remote web service to execute an operation without waiting to know the result. This could include passing a data structure that gets copied to the remote system. This pattern can be modified to include a callback. In that case, Salesforce would expect the remote system to call back once done processing and sending the result. This is a common pattern due to its near real-time nature, agility, scalability, and small overhead.
- **Relevant Salesforce features:** Several Salesforce features are relevant, such as outbound messages, platform events, **change data capture** (CDC), the Salesforce streaming API, callouts using future methods, callouts using queueable classes, email notifications, and push notifications.

> Note
> You can find more details at the following link: https://packt.link/dl4cl.

## Batch Data Synchronization

Here is some high-level info about this pattern:

- **Description:** As indicated by its name, this pattern is mainly used to copy a dataset from one system to another. The dataset usually contains a bulk of records to be more efficient.
- **Timing:** This pattern is asynchronous, usually scheduled to run at a certain time and interval. Execution time is normally subject to the size of the copied dataset.
