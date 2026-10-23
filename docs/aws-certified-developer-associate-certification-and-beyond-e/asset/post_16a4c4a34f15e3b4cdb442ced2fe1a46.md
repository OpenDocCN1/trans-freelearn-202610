
2. Once you log in, you’ll see a page similar to the one shown in Figure 10.32.

![](img/34ff9b5b5a734dae3450ab701e223c73_442_0.png)

Figure 10.32 – Chapter Review Questions for Chapter 10

3. Once ready, start the following practice drills, re-attempting the quiz multiple times.

## Exam Readiness Drill

For the first three attempts, don’t worry about the time limit.

### ATTEMPT 1

The first time, aim for at least 40%. Look at the answers you got wrong and read the relevant sections in the chapter again to fix your learning gaps.

### ATTEMPT 2

The second time, aim for at least **60%**. Look at the answers you got wrong and read the relevant sections in the chapter again to fix any remaining learning gaps.

### ATTEMPT 3

The third time, aim for at least 75%. Once you score 75% or more, you start working on your timing.

> **Tip**
> You may take more than **three** attempts to reach 75%. That’s okay. Just review the relevant sections in the chapter till you get there.

## Working On Timing

**Target**: Your aim is to keep the score the same while trying to answer these questions as quickly as possible. Here’s an example of how your next attempts should look like:

| Attempt | Score | Time Taken |
|---|---|---|
| Attempt 5 | 77% | 21 mins 30 seconds |
| Attempt 6 | 78% | 18 mins 34 seconds |
| Attempt 7 | 76% | 14 mins 44 seconds |

Table 10.1 – Sample timing practice drills on the online platform

> **Note**
> The time limits shown in the above table are just examples. Set your own time limits with each attempt based on the time limit of the quiz on the website.

With each new attempt, your score should stay above 75% while your “time taken” to complete should “decrease”. Repeat as many attempts as you want till you feel confident dealing with the time pressure.

## 11 Deploying a Multi-Tier Application Stack with Amazon ECS, Fargate, and EKS

Containerization has revolutionized the way developers package and deploy applications. By encapsulating an application and its dependencies into a container, developers can ensure consistency across various environments, from development to production. This transformative approach eliminates the need for developers to tailor their application code for specific host environments.

You no longer need to build application code for a specific host environment, and you can easily transition your application from a desktop computer to a virtual server or from a Linux to a Windows OS.

The traditional Virtual Machine (VM) architecture had its limitations. Each VM hosted a guest operating system, leading to a bloated system that reduced agility and resulted in wastage due to duplication. Containers, on the other hand, share the host machine’s OS kernel. This makes them lightweight and quick to start, allowing a significantly higher number of containers to be hosted on the same physical hardware compared to VMs.

This chapter will equip you with a comprehensive understanding of container orchestration within the AWS ecosystem! Through detailed explanations, hands-on examples, and best practices, you’ll learn how to effectively leverage Amazon Elastic Container Service (Amazon ECS) and Amazon Elastic Kubernetes Service (Amazon EKS) to orchestrate your containerized applications.

Whether you’re looking to deploy a microservices architecture, manage batch processing jobs, or scale your application across multiple environments, ECS and EKS offer the tools and features necessary to achieve your objectives.

The AWS Certified Developer-Associate certification requires you to understand the core concepts of Elastic Container Service to deploy your application using containerization technologies. This topic also aligns with Domain 3: Deployment of the exam guide.

This chapter will also explore how **TodoPlus Limited** can use containerization and orchestration services to enhance the development, deployment, and management of its task management application. Utilizing AWS's robust container services, such as Amazon ECS for Docker-based container management, TodoPlus can significantly streamline and automate its application workflows, achieving scalability, reliability, and efficiency.

The following topics are covered in this chapter:

- Introduction to containers and Docker
- Using Amazon ECS to run and manage containers at scale
- Using Amazon EKS to run and manage containers at scale
- Project task – rearchitect the Todo List Node.js application to use containerization instead of virtual machines

### Introduction to Containers and Docker

In software development and deployment, containerization has emerged as a revolutionary approach, offering an efficient alternative to traditional VMs. At the heart of this revolution is Docker, a standardized container creation and management platform that makes it simpler for developers to package and deploy applications consistently across various environments.

### Understanding Containerization

Containerization involves encapsulating an application and its dependencies into a lightweight, standalone executable package. Containers include everything needed to run the application: code, runtime, system tools, libraries, and settings. Although containers are isolated from each other and the host system, they share the host OS kernel, libraries, and resources as needed. This isolation ensures that applications work uniformly and securely, regardless of where they are deployed.

![](img/34ff9b5b5a734dae3450ab701e223c73_447_0.png)

Figure 11.1: Containerization versus virtual machine system architecture

The architecture provided in Figure 11.1 illustrates containerization clearly. On the left side of the illustration, you can see multiple containers labeled App A to App F, each representing an isolated application (process) with its dependencies. These containers all rest on a single Docker layer, signifying Docker Engine’s role in managing the container lifecycle. Below the Docker layer is the host operating system, which provides the shared kernel and operating system capabilities. The entire stack is built upon the infrastructure, which could be physical servers or cloud-based virtual machines.

The illustration on the right relates to a typical virtual machine architecture. Each virtual machine must host its guest operating system, which results in duplication and bloat. You will also need more hardware resources to host the same number of apps as you would within a containerized offering. Another key fact is that containers are designed to perform specific tasks, are streamlined, and don’t host unnecessary tools and utilities, which are often bundled with your guest operating system.

Another area where containers are considered a blessing is the shift from a monolith to a microservices architecture in modern application design patterns. Microservices is a software development approach that encourages you to build your application components as separate independent services that communicate with each other using standard protocols such as HTTP or lightweight asynchronous messaging over well-defined APIs. Each component is designed, built, and managed by small independent teams that can focus on their core expertise and ensure scalable applications with fast development and quick iterative enhancements. Using containers to build a microservices architecture means that you can achieve quick scalability, have the ability to transition across environments, and can avoid single points of failure easily.

### The Role of Docker

Docker has played a pivotal role in popularizing container technology. By providing a lightweight platform for container management, Docker enables developers to build, share, and run applications as containers efficiently. This simplicity has dramatically contributed to Docker’s widespread adoption, making it a de facto standard in containerization.

Docker operates on a client-server architecture, where the Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing Docker containers. Docker containers can run on any machine with the Docker daemon installed, ensuring consistency across development, testing, and production environments.

### Advantages of Using Docker

- **Consistency across Environments:** By containerizing applications, developers can eliminate the “it works on my machine” problem. Applications packaged as Docker containers run the same, regardless of where they are deployed.
- **Rapid Deployment and Scaling:** Containers can be started and stopped in seconds, making it easy to scale out applications dynamically.
- **Resource Efficiency:** Containers require fewer system resources than traditional or virtual machine environments because they share the host system’s kernel rather than requiring their OS.

Spearheaded by Docker, containerization represents a significant shift in how applications are developed, shipped, and run. This approach streamlines the development process and enhances operational efficiencies in deployment and scaling. The schema provided visually captures the essence of containerization and contrasts it with the traditional virtual machine approach, underscoring the benefits and efficiencies gained through containerized application deployment.

While containers will host your application stack as an isolated and independent unit, you still need a mechanism to manage your containers’ creation, deployment, and orchestration. That’s where Amazon Elastic Container Service (Amazon ECS) comes to the rescue, and it is discussed in the next section.

### Using Amazon ECS to Run and Manage Containers at Scale

Amazon ECS is a fully managed container orchestration service that enables you to run and manage Docker containers on a cluster of Amazon EC2 instances that you manage or on AWS Fargate, which is the serverless alternative where AWS manages all the underlying infrastructure. With ECS, you can launch and stop container-based applications with simple API calls, query the complete state of your cluster, and access many familiar features such as security groups, Elastic Load Balancing, EBS volumes, and IAM roles.
