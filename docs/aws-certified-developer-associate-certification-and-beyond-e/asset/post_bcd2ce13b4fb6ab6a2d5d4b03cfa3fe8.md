
## Exam Readiness Drill – Chapter Review Questions

Apart from a solid understanding of key concepts, being able to think quickly under time pressure is a skill that will help you ace your certification exam. That is why working on these skills early on in your learning journey is key.

Chapter review questions are designed to improve your test-taking skills progressively with each chapter you learn and review your understanding of key concepts in the chapter at the same time. You'll find these at the end of each chapter.

### How to Access These Materials

To learn how to access these resources, head over to the chapter titled *Chapter 17, Accessing the Online Resources.*

To open the Chapter Review Questions for this chapter, perform the following steps:

1. Click the link – https://packt.link/DVAC02_CH11.
   Alternatively, you can scan the following QR code (Figure 11.46):

   ![](img/34ff9b5b5a734dae3450ab701e223c73_495_0.png)

   Figure 11.46 – QR code that opens Chapter Review Questions for logged-in users

2. Once you log in, you’ll see a page similar to the one shown in *Figure 11.47*:

   ![](img/34ff9b5b5a734dae3450ab701e223c73_496_0.png)

   Figure 11.47 – Chapter Review Questions for Chapter 11

3. Once ready, start the following practice drills, re-attempting the quiz multiple times.

### Exam Readiness Drill

For the first three attempts, don’t worry about the time limit.

**ATTEMPT 1**

The first time, aim for at least 40%. Look at the answers you got wrong and read the relevant sections in the chapter again to fix your learning gaps.

**ATTEMPT 2**

The second time, aim for at least 60%. Look at the answers you got wrong and read the relevant sections in the chapter again to fix any remaining learning gaps.

**ATTEMPT 3**

The third time, aim for at least 75%. Once you score 75% or more, you start working on your timing.

> **Tip**
> You may take more than three attempts to reach 75%. That’s okay. Just review the relevant sections in the chapter till you get there.

### Working On Timing

Target: Your aim is to keep the score the same while trying to answer these questions as quickly as possible. Here’s an example of how your next attempts should look like:

| Attempt | Score | Time Taken |
| --- | --- | --- |
| Attempt 5 | 77% | 21 mins 30 seconds |
| Attempt 6 | 78% | 18 mins 34 seconds |
| Attempt 7 | 76% | 14 mins 44 seconds |

Table 11.1 – Sample timing practice drills on the online platform

> **Note**
> The time limits shown in the above table are just examples. Set your own time limits with each attempt based on the time limit of the quiz on the website.

With each new attempt, your score should stay above 75% while your “time taken” to complete should “decrease”. Repeat as many attempts as you want till you feel confident dealing with the time pressure.

## Getting to Grips with DevOps Using AWS CI/CD Tools

DevOps has become an integral part of software development in recent years. It allows teams to deliver high-quality software faster and more efficiently by promoting collaboration and communication between development and operations teams. One key component of DevOps is **Continuous Integration and Continuous Deployment/Continuous Delivery (CI/CD)**, which helps teams automate their software development processes.

AWS provides a range of tools for CI/CD, including AWS CodeCommit, AWS CodeBuild, AWS CodePipeline, and AWS CodeDeploy. These tools help teams automate their software development workflows, from writing code and building applications to testing, deploying, and delivering software to end users.

This chapter examines how to start DevOps using AWS CI/CD tools. You will learn the basics of CI/CD and how it fits into the DevOps landscape. Each CI/CD tool is discussed in detail, exploring its features, benefits, and how they can help teams streamline their software development processes.

Whether you’re new to DevOps or an experienced practitioner, this chapter will provide a comprehensive overview of how to get started with AWS CI/CD tools and how they can help you deliver high-quality software faster and more efficiently. By the end of this chapter, you’ll have a solid understanding of using AWS CI/CD tools to build, test, deploy, and manage your applications in the cloud.

DevOps and CI/CD fall within the scope of *Domain 3: Deployment of the AWS Certified Developer Associate syllabus*. You must understand version control, how to prepare artifacts for deployment, and manual and automated approvals in AWS CodePipeline and CI/CD workflows.

Expanding on the requirements of our fictitious company, TodoPlus Limited, you will gain practical experience of implementing DevOps principles to improve the development and deployment of the **Todo List** task management application using AWS’s suite of CI/CD tools. For instance, whenever a change is committed to the application’s code base in AWS CodeCommit, it can automatically trigger AWS CodeBuild to compile the code and run tests. If the tests pass, AWS CodeDeploy can automatically deploy the changes to production. AWS CodePipeline can orchestrate this workflow, ensuring that every change is automatically, efficiently, and reliably delivered to users.

Implementing these CI/CD practices can significantly increase the speed, efficiency, and reliability of its application’s development and deployment processes. The upcoming chapter will provide a deeper understanding of these principles and tools and show how TodoPlus can integrate them into its operations.

### What Is DevOps?

DevOps is a cultural and technical movement that has revolutionized the software development industry. It represents a shift from the traditional siloed approach to software development, where development and operations teams work in separate departments, to a more integrated and agile approach. The DevOps culture emphasizes collaboration and communication between development and operations teams, leading to faster and more efficient software delivery.

The diagram in Figure 12.1 explains the DevOps lifecycle:

![](img/34ff9b5b5a734dae3450ab701e223c73_500_0.png)

Figure 12.1 – DevOps lifecycle

It is essential to understand this schema when talking about DevOps. The DevOps lifecycle is often depicted in a loop to emphasize the continuous nature of the process. Here’s a breakdown of each phase in the cycle:

- **Plan:** This is the phase where the project’s course of action is outlined. It involves requirements gathering, scheduling, and all the preparation work before starting actual development.
- **Create:** During this phase, the software development takes place. Code is written to create new features, fix bugs, or update the software.
- **Verify:** Once the software is developed, it needs to be tested. This phase involves quality assurance testing, unit testing, integration testing, and other testing to verify that the code meets the quality standards.
- **Package:** This phase involves packaging the code so that it can be deployed. It often involves versioning the software and preparing it in a deployable format, such as a container or a virtual machine image.
- **Release:** This phase is where the software is made ready for deployment. The final checks ensure that the software is prepared and release approvals are obtained. Continuous delivery and continuous deployment practices fall into this phase.
- **Configure:** The software is configured in the production environment. This involves setting up the necessary infrastructure, which can be automated through infrastructure as code practices.
- **Monitor:** After the software is deployed, it needs to be monitored to ensure it operates as expected. This phase includes using logging, performance metrics, and real-time monitoring to identify and resolve issues quickly.

The two halves of the circle, Dev and Ops, represent the development and operations parts of the cycle, highlighting the integration and collaboration between **Software Development (Dev)** and **IT Operations (Ops)**. This integration is the core concept of DevOps, aiming to shorten the development lifecycle and provide continuous delivery with high software quality. The loop indicates that after monitoring, insights and feedback are used to plan new changes, and the cycle continues—hence the terms “continuous integration” and “continuous delivery/deployment.”

### Implementing DevOps through CI/CD

To implement a DevOps culture in your organization, you must develop this principle of continuous integration and continuous delivery/continuous deployment.
