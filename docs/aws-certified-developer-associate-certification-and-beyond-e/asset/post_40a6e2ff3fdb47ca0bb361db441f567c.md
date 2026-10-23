
- 14. From the left-hand menu, click on **Target Groups** under **Load Balancing**. Select the target group you created in *Step 4* in the right-hand pane. In the bottom half of this page, you should find your instances in a healthy state, as per the following screenshot:

![](img/34ff9b5b5a734dae3450ab701e223c73_308_0.png)

| Instance ID | Name | Port | Zone | Health status |
|---|---|---|---|---|
| i-069c9a4881b9896bd |  | 80 | eu-west-2b | Healthy |
| i-07f8476f18d9d3ba9 |  | 80 | eu-west-2b | Healthy |

**Figure 7.31** – Checking to see whether the instances report healthy to the load balancer

Although you cannot access your application with the internal load balancer, the healthy state status means the instances have been successfully deployed. This is because you will still need to deploy the API Gateway and the NLB, which we will discuss in the next chapter.

## Summary

In this chapter, you have discovered how to deploy highly available architectures on the AWS cloud. High-availability systems are trustworthy because they keep running even if some parts break. These systems are also robust, meaning they can absorb shocks to their operation without damaging their functionality and quickly and easily recover after experiencing a setback. The percentage of available time is a standard metric for gauging availability at a high level. The number of nines is often used as an indicator of extreme availability. An uptime of “four nines,” for instance, indicates that the system is available 99.99% of the time and has no more than 1 hour of downtime annually.

While load balancers offer the ability to build highly available systems, Auto Scaling is a service that enables you to design elasticity for your application. As demand for your application increases, AWS Auto Scaling can rapidly accommodate by deploying additional EC2 instances to cope with the load. Using innovative scaling policies, you can configure the same service to terminate unnecessary instances when demand drops, ensuring that costs are controlled.

At this stage, you have built out the primary components of the application. The load balancer will offer high availability, and the Auto Scaling service will ensure elasticity and scalability. The next chapter introduces the API Gateway service and how it allows you to create, manage, and secure APIs that enable access to backend application data, logic, and functionality. In the case of our Todo List application, these backend components will enable interaction with the database, which will host the end user’s to-do list items and allow the user to create and update their to-do items.

## Exam Readiness Drill – Chapter Review Questions

Apart from a solid understanding of key concepts, being able to think quickly under time pressure is a skill that will help you ace your certification exam. That is why working on these skills early on in your learning journey is key.

Chapter review questions are designed to improve your test-taking skills progressively with each chapter you learn and review your understanding of key concepts in the chapter at the same time. You'll find these at the end of each chapter.

> **How to Access These Materials**
> 
> To learn how to access these resources, head over to the chapter titled *Chapter 17, Accessing the Online Resources*.

To open the Chapter Review Questions for this chapter, perform the following steps:

- 1. Click the link – `https://packt.link/DVAC02_CH07`.

Alternatively, you can scan the following **QR code** (*Figure 7.32*):

![](img/34ff9b5b5a734dae3450ab701e223c73_309_0.png)

Figure 7.32 – QR code that opens Chapter Review Questions for logged-in users

- 2. Once you log in, you’ll see a page similar to the one shown in *Figure 7.33*:

![](img/34ff9b5b5a734dae3450ab701e223c73_310_0.png)

*Figure 7.33 – Chapter Review Questions for Chapter 7*

- 3. Once ready, start the following practice drills, re-attempting the quiz multiple times.

### Exam Readiness Drill

For the first three attempts, don’t worry about the time limit.

### *ATTEMPT 1*

The first time, aim for at least **40%**. Look at the answers you got wrong and read the relevant sections in the chapter again to fix your learning gaps.

### *ATTEMPT 2*

The second time, aim for at least **60%**. Look at the answers you got wrong and read the relevant sections in the chapter again to fix any remaining learning gaps.

### ATTEMPT 3

The third time, aim for at least 75%. Once you score 75% or more, you start working on your timing.

> **Tip**
> You may take more than **three** attempts to reach 75%. That’s okay. Just review the relevant sections in the chapter till you get there.

### Working On Timing

**Target**: Your aim is to keep the score the same while trying to answer these questions as quickly as possible. Here’s an example of how your next attempts should look like:

| Attempt | Score | Time Taken |
|---|---|---|
| Attempt 5 | 77% | 21 mins 30 seconds |
| Attempt 6 | 78% | 18 mins 34 seconds |
| Attempt 7 | 76% | 14 mins 44 seconds |

Table 7.1 – Sample timing practice drills on the online platform

> **Note**
> The time limits shown in the above table are just examples. Set your own time limits with each attempt based on the time limit of the quiz on the website.

With each new attempt, your score should stay above 75% while your “time taken” to complete should “decrease”. Repeat as many attempts as you want till you feel confident dealing with the time pressure.

## Event-Driven Computing with AWS Lambda and Securing Access to Backend APIs with Amazon API Gateway

In the rapidly evolving world of cloud computing, serverless architecture and event-driven computing have caused a paradigm shift, altering how we think about infrastructure, scalability, and resource utilization. This chapter delves into the core of serverless computing on AWS, a platform that has redefined the application development and deployment landscape. Focusing on AWS Lambda and Amazon API Gateway, this chapter aims to equip you with the knowledge and skills required to use these powerful services to build efficient, scalable, and cost-effective serverless applications.

AWS Lambda is the cornerstone of event-driven serverless architecture. It allows you to run code responding to events without managing servers, fundamentally changing how applications are built and deployed. Lambda’s event-driven nature makes it ideal for many applications, from simple web applications to complex data processing workflows. In this chapter, we will explore the basics of AWS Lambda, diving into its operational model, use cases, and how it seamlessly integrates with other AWS services. We will walk you through creating your first Lambda function, demonstrating how to trigger it with events from other AWS services, such as S3.

However, understanding AWS Lambda in isolation is not enough. To fully harness its power, you need to comprehend the ecosystem in which it operates. This leads us to Amazon API Gateway, a service that enables you to create, publish, maintain, monitor, and secure APIs at any scale. It acts as the front door for applications to access data, business logic, or functionality from your backend services. This chapter will guide you through setting up an API Gateway, showing you how to manage access to your Lambda functions and other backend resources, such as APIs running on EC2 instances, as with our client Todo Plus Limited.

This chapter also covers concepts related to the AWS Certified Developer Associate exam, particularly topics from *Domain 1: Development with AWS Services* and *Domain 3: Deployment*. The exam heavily tests concepts related to AWS Lambda and API Gateway, and understanding these technologies is critical to successfully passing the exam.

In this chapter, you will build an API gateway that will act as the front door to the backend operations of the **Todo List** application. This gateway will enable users who sign up for the service to access their *todo lists* and interact with the application.

We cover the following topics in this chapter:

- Building event-driven solutions with AWS Lambda
- Advanced Lambda concepts
- Managing access to backend services with API Gateway
- **Project Task** – Deploy an API Gateway to enable secure access to the backend services provided by the Todo List application

### Building Event-Driven Solutions with AWS Lambda

Event-driven architecture is a paradigm shift in how software applications are designed, built, and run. **AWS Lambda** is a serverless computing service that allows you to run code in response to events without provisioning or managing servers; it provides high scalability, flexibility, and cost-effectiveness.

### Understanding AWS Lambda in Event-Driven Architectures

AWS Lambda excels in event-driven scenarios because it can react to events in real time. An event source, such as Amazon S3, DynamoDB, or a custom application, triggers a Lambda function, which executes its code in response to the event. Event-driven architecture also enables you to build and design distributed applications and microservices. This ensures that different components of your application can scale independently, removing the level dependencies often associated with a monolithic architecture.

In addition, AWS Lambda can help you build serverless architectures, so you do not need to manage the underlying infrastructure. Provided that the compute requirements for your application fall within the scope and limitations of Lambda, it is possible to build a fully functional application that uses AWS Lambda for all its compute operations.
