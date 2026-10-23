
12. Click **Next**.

13. Recheck all the settings on the **Review** page and then click **Store**.

14. Your secret will now be created. Please note that it will take a few minutes to complete and set up the scheduled rotation configuration. This delay is caused by AWS CloudFormation, which builds a stack consisting of your Lambda function in the background. Refresh the page after a few minutes and your automatic rotation should appear enabled.

15. To test that Secrets Manager can access your RDS database and rotate the credentials, click on **Secrets** from the left-hand menu and then click on the **secret** you created to open up the configuration, as per the following screenshot:

![](img/34ff9b5b5a734dae3450ab701e223c73_259_0.png)

Figure 6.35 – Secrets Manager Todo List secret configuration

16. Next, click on Rotate secret immediately. You will get a prompt asking you if you want to rotate the secret. Click Rotate.

17. You should then get a success message reading **Secret scheduled for rotation**.

18. If you now click on the **Retrieve secret value** button, you should get the updated credentials similar to the following screenshot:

![](img/34ff9b5b5a734dae3450ab701e223c73_260_0.png)

Figure 6.36 – Updated RDS credentials

In this series of project tasks, you deployed an RDS database in your VPC, which will be used for hosting the backend data for your Todo List application. To ensure that you follow best practices, you also created a secret in AWS Secrets Manager to automatically rotate and manage your RDS credentials every seven days in alignment with TodoPlus Limited’s corporate policy.

You also learned about the key prerequisites to ensure that AWS Secrets Manager can connect to your RDS instance in your VPC and deploy a Lambda function that will perform the scheduled rotation.

## Summary

In this chapter, you learned about relational and non-relational databases on AWS. At the heart of every application is data; you need a tool not only to store that data in an efficient and cost-effective manner but also to scan, query, and ultimately manipulate that data to derive critical information from it.

Depending on the application requirements, you may need to deploy either traditional relational database engines such as MySQL or Oracle or non-relational key-value store databases such as Amazon DynamoDB. In addition, your application may also require in-memory caching engines that help to improve latency and reduce costs. Regardless of which database model you opt for, understanding the fundamentals of databases and how they should be configured and deployed is critical for any application.

So far, you have learned about the core infrastructure services offered by AWS, primarily comprising compute, network, storage, and, in this chapter, databases. These four key infrastructure components form the basis of any multi-tier application architecture. In the next chapter, you will examine how you can deploy a multi-tier solution using these four key technologies. You will also explore more about the tools that you can use to ensure that your application solution is highly available and scalable.

## Exam Readiness Drill – Chapter Review Questions

Apart from a solid understanding of key concepts, being able to think quickly under time pressure is a skill that will help you ace your certification exam. That is why working on these skills early on in your learning journey is key.

Chapter review questions are designed to improve your test-taking skills progressively with each chapter you learn and review your understanding of key concepts in the chapter at the same time. You’ll find these at the end of each chapter.

### How to Access These Materials

To learn how to access these resources, head over to the chapter titled *Chapter 17, Accessing the Online Resources.*

To open the Chapter Review Questions for this chapter, perform the following steps:

-   1. Click the link – https://packt.link/DVAC02_CH06.
    -   Alternatively, you can scan the following QR code (Figure 6.37):

![](img/34ff9b5b5a734dae3450ab701e223c73_262_0.png)

Figure 6.37 – QR code that opens Chapter Review Questions for logged-in users

-   2. Once you log in, you’ll see a page similar to the one shown in *Figure 6.38*:

![](img/34ff9b5b5a734dae3450ab701e223c73_263_0.png)

Figure 6.38 – Chapter Review Questions for Chapter 6

-   3. Once ready, start the following practice drills, re-attempting the quiz multiple times.

## Exam Readiness Drill

For the first three attempts, don’t worry about the time limit.

### ATTEMPT 1

The first time, aim for at least 40%. Look at the answers you got wrong and read the relevant sections in the chapter again to fix your learning gaps.

### ATTEMPT 2

The second time, aim for at least 60%. Look at the answers you got wrong and read the relevant sections in the chapter again to fix any remaining learning gaps.

### ATTEMPT 3

The third time, aim for at least **75%**. Once you score 75% or more, you start working on your timing.

> **Tip**
>
> You may take more than **three attempts** to reach 75%. That’s okay. Just review the relevant sections in the chapter till you get there.

### Working On Timing

**Target:** Your aim is to keep the score the same while trying to answer these questions as quickly as possible. Here’s an example of how your next attempts should look like:

| Attempt | Score | Time Taken |
|---|---|---|
| Attempt 5 | 77% | 21 mins 30 seconds |
| Attempt 6 | 78% | 18 mins 34 seconds |
| Attempt 7 | 76% | 14 mins 44 seconds |

Table 6.2 – Sample timing practice drills on the online platform

> **Note**
>
> The time limits shown in the above table are just examples. Set your own time limits with each attempt based on the time limit of the quiz on the website.

With each new attempt, your score should stay above 75% while your “time taken” to complete should “decrease”. Repeat as many attempts as you want till you feel confident dealing with the time pressure.

## Building Application Solutions with High Availability, Elasticity, and Data Security

One of the biggest challenges associated with hosting a website or a web application is ensuring that it is consistently available to your end users and can handle any amount of traffic your business needs to support. You must architect your application to be both **highly available** and **scalable**.

Customers may use terms such as “horizontal scaling” and “microservices” when discussing their plans for site expansion, but they may not fully understand how to begin growing their sites. This chapter discusses various scaling options. For instance, on-premises solutions can utilize the cloud, even if the existing workload is hosted in a traditional data center, to scale up operations while keeping costs low. You can also use preconfigured solutions if your workload is already in the cloud.

This chapter will teach you how to architect your server-based environment to distribute traffic to your application using Amazon Elastic Load Balancers. Depending on the use case, you will learn which type of load balancer to configure, along with health checks and listener rules. Additionally, this chapter introduces automatic horizontal scaling, designed to add elasticity to your application offering and manage application demand using Amazon Auto Scaling.

This chapter is also designed to help you prepare for the AWS Certified Developer Associate exams. It covers concepts related to deployment methodologies for your application, specifically, data security concepts as per *Domain 2: Security, Task Statement 2: Implement encryption using AWS services.*

Expanding on our business scenario, this chapter will also explore how **TodoPlus Limited** can design its task management application, **Todo List**, to be highly available, elastic, and secure using the power of AWS cloud services.

![](img/34ff9b5b5a734dae3450ab701e223c73_267_0.png)

Figure 7.1 – Vertical scaling has limitations

An important point to note is that vertical scaling does not provide support when the instance fails or if there is an outage in the Availability Zone hosting the instance, resulting in poor customer experience.

AWS offers another scaling configuration known as **horizontal scaling**. Here, multiple instances act as *nodes* in a fleet that hosts a copy of your application. Furthermore, you can spread the nodes across multiple Availability Zones in each region. This means that users can be redirected to other nodes in the fleet if one node fails or a specific Availability Zone has an outage.

With horizontal scaling, traffic can be distributed across a fleet of instances (nodes) using the **Elastic Load Balancing** service. AWS Elastic Load Balancing is a service designed to distribute traffic across a fleet of resources (in this case, EC2 instances), which helps to avoid overloading some instances while others remain idle, and improves response times. By distributing your client/user requests across instances that are deployed in multiple Availability Zones as per the diagram in Figure 7.2, you can achieve **high availability** within a region – a key benefit of using cloud computing.

![](img/34ff9b5b5a734dae3450ab701e223c73_267_1.png)

Figure 7.2 – Horizontal scaling access Availability Zones can help achieve high availability
