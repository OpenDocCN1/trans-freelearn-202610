
As your analytics develop, you will have a list of actions that is either transforming the data, running it through a model or algorithm, or reacting to the result. It can get complicated quickly. By encapsulating the analytics, it is easier to know where to make changes when needed down the road. You will also be able reconfigure parts of the process without affecting the other components.

The encapsulation process follows these steps:

- 1. Make a list of the steps.
- 2. Organize them into groups.
- 3. Think which groups are likely to change together.
- 4. Separate the groups that are independent into their own processes.

It is a good idea to have the data transformation steps separate from the analytical steps if possible. Sometimes, the analysis is tightly tied to the data transformation, and it does not make sense to separate, but in most cases, it can be separated. The action steps based on the results of the analysis should almost always be separate.

Each group of steps will also have its own resource needs. By encapsulating them and separating the processes, you can assign resources independently and scale more efficiently where you need it. You can do more with less.

## Decoupling with message queues

Decoupling encapsulated analytics processes with message queues has several advantages. It allows for change in any process without requiring the other ones to adjust. This is because there is no direct link between them.

It also builds in some robustness in case one process has a failure. The queue can continue to expand without losing data while the down process restarts, and nothing will be lost after things get going again.

## What is a message queue?

![](img/9f2a55db1236f995dcd9392acbc82910_87_0.png)

New data comes into a queue as a message, it goes into line for delivery, and then it is delivered to the end server when it gets its turn. The process adding a message is called the **publisher**, and the process receiving the message is called the **subscriber**.

The message queue exists regardless of whether the publisher or subscriber is connected and online. This makes it robust against intermittent connections (intentional or unintentional). The subscriber does not have to wait until the publisher is willing to chat and vice versa.

The size of the queue can also grow and shrink as needed. If the subscriber gets behind, the queue just grows to compensate until it can catch up. This can be useful if there is a sudden burst in messages by the publisher. The queue will act as a buffer and expand to capture the messages while the subscriber is working through the sudden influx.

There is a limit, of course. If the queue reaches some set threshold, it will reject (and you will most likely lose) any incoming messages until the queue gets back under control.

Here is a contrived but real-world example of how this can happen:

> Joe Cut-rate (the developer): *Hey, when do you want this doo-hickey device to wake up and report?*
> 
> Jim Unawares (the engineer): *Every 4 hours*
> 
> Joe Cut-rate: *No sweat. I’ll program it to start at 12 a.m. UTC, then every 4 hours after. How many of these you gonna sell again?*

## Distributed computing

Also called cluster computing, distributed computing refers to spreading processes across multiple servers using frameworks that abstract the coordination of each individual server. The frameworks make it appear as if you are using one unified system. Under the covers, it could be a few servers (called nodes) to thousands. The framework handles the orchestration for you.

## Avoid containing analytics to one server

The advantage to this for IoT analytics is in scale. You can add resources by adding nodes to the cluster; no change to the analytics code is required. The most common framework in use today is Hadoop, which we will talk about in *Chapter 4, Creating an AWS Cloud Analytics Environment*.

Try and avoid containing analytics to one server (with a few exceptions). This puts a ceiling on scale.

## When to use distributed and when to use one server

There is a complexity cost to distributed computing though. It is not as simple as single server analytics. Even though the frameworks handle a lot of the complexity for you, you still have to think and design your analytics to work across multiple nodes.

Here are some guidelines on when to keep it simple and on one server:

- **There is not much need for scale:** Your analytics process needs little change even if the number of IoT devices and data explodes. For example, the analytics process runs a forecast on data already summarized by month. The volume of devices makes little difference in that case.
- **Small data instead of big data:** The analytics run on a small subset of data without much impact from data size. Analytics on random samples is an example.
- **Resource needs are minimal:** Even at orders of magnitude more data, you are unlikely to need more than what is available with a standard server. In this case, keep it simple.

## Assuming that change is constant

The world of IoT analytics moves quickly. The analytics you create today will change many times over as you get feedback on results and adapt to the changing business conditions. Your analytics processes will need to change. Assume this will happen continuously and design for change. This brings us to the concept of **continuous delivery**.

Continuous delivery is a concept from software development. It automates the release of code into production. The idea is to make change a regular process. Bring this concept into your analytics by keeping a set of simultaneous copies that you use to progress through three stages:

- 1. **Development:** Keep a copy of your analytics for improving and trying out new things.
- 2. **Test:** When ready, merge your improvements into this copy where the functionality stays the same, but it is repeatedly tested. The testing ensures it is working as intended. Keeping a separate copy for test allows development to continue on other functionality.
- 3. **Master:** This is the copy that goes into production. When you merge things from test to the master copy, it is the same as putting it into live use. Cloud providers often have a continuous delivery service that can make this process simpler.

For any software developer readers out there, this is a simplification of the **git flow** method, which is a little outside the scope of this book. If the author can drop a suggestion, it is worth some additional research to learn git flow and apply it to your analytics development in the cloud.

## Leverage managed services

Cloud infrastructure providers, such as AWS and Microsoft Azure, offer services for things such as message queues, big data storage, and machine learning processing. The services handle the underlying resource needs such as server, storage provisioning, and also network requirements. You do not have to worry about how this happens under the hood, and it scales as big as you need it.

Cloud providers also manage global distribution of services to ensure low latency. The following image shows the AWS regional data center locations combined with underwater internet cabling:

![](img/9f2a55db1236f995dcd9392acbc82910_91_0.png)

This reduces the amount of things you have to worry about for analytics. It allows you to focus more on the business application and less on the technology. This is a good thing and you should take advantage of it.

An example of a managed service is Amazon **Simple Queue Service (SQS)**. SQS is a message queue where the underlying server, storage, and compute needs are managed automatically by AWS systems. You only need to set it up and configure it, which takes just a few minutes.

## Use Application Programming Interfaces (API)

APIs are a way for other processes, software, or services to access analytics code that you have created. It allows you to easily reuse your code in other applications. You can also allow your customers to directly access this functionality through web-based APIs.

An API builds on of encapsulation principle. It is a defined list of supported actions and information that another system can call and retrieve. The calling system does not need to know how the action is performed or the information created and retrieved. The complexity is hidden.

The API defines how to interact with an encapsulated set of analytic processes. It abstracts away the details. It also supports low friction change as the analytics processes can be improved without requiring other systems to alter what they are doing. As long as the API definition is held constant, the other systems will not know the difference.

APIs are a great way to create building blocks for more complex analytics. You can use multiple APIs to build rich, fully-functional analytic applications in a short period of time. It is also far easier to adapt to changing business conditions by reconfiguring the assembled applications to use a different mix of APIs.

A Web API uses the internet to handle the communication between systems. Cloud providers offer this as a managed service. It can help a great deal in handling security and scale. Using this service, you can quickly implement new analytics functionality securely and at scale. The API can be either public or private. Private is only accessible to your internal applications. Public opens it up to systems outside your company.

![](img/9f2a55db1236f995dcd9392acbc82910_92_0.png)

Example architecture using Web API gateway. Source: Amazon Web Services
