
# Introduction to AWS Accounts and Global Infrastructure

We will start by introducing the **AWS Global Infrastructure**, which represents the vast collection of data center facilities located across multiple countries throughout the world, which you can access to design, build, and deploy cloud resources. You will also learn about the various tools used to help effectively manage your AWS accounts. Furthermore, you will also complete a series of exercises that will help you to apply the principles related to having a multi-account AWS architecture in real-world use cases.

# Making the Most Out of This Book – Your Certification and Beyond

This book and its accompanying online resources are designed to be a complete preparation tool for your DVA-C02 Exam.

The book is written in a way that you can apply everything you’ve learned here even after your certification. The online practice resources that come with this book (*Figure 1.1*) are designed to improve your test-taking skills. They are loaded with timed mock exams, interactive flashcards, and exam tips to help you work on your exam readiness from now till your test day.

> **Before You Proceed**
>
> To learn how to access these resources, head over to *Chapter 17, Accessing the Online Practice Resources*, at the end of the book.

![](img/34ff9b5b5a734dae3450ab701e223c73_30_0.png)

Figure 1.1 – Dashboard interface of the online practice resources

This study guide will help you understand how the cloud can address TodoPlus Limited's needs and challenges. By delving into security, scalability, high availability, and effective financial operations (FinOps), you will learn how the cloud can provide solutions that meet these needs and offer the potential for growth and innovation. By the end of this journey, you should clearly understand how cloud computing can be leveraged to drive business success in a realistic scenario. You will also be well-equipped to pass the AWS Certified Developer Associate exam. This exam will test your knowledge and ability to develop applications on AWS, learn how to secure application code, and understand the fundamentals of DevOps by applying continuous integration and continuous delivery as your application design workflows.

# Setting the Scene – Background Information

TodoPlus Limited, based in New Jersey, specializes in creating custom productivity apps for small to medium-sized businesses. Their unique approach involves tailoring applications to meet the specific needs of each client. Now, they have a new goal – to develop and sell off-the-shelf (OTS) productivity apps directly to the public.

The company, which previously focused on creating apps for clients to use internally or sell to end users, wants to enter the retail market with its own line of products. The first step is launching a “To-Do List” application for individual users by the end of the year. Later, they plan to expand their offerings to businesses.

The board of directors is excited about this new venture and believes it’s a market where TodoPlus can make a significant impact.

In this study guide, you will see how the AWS cloud platform can facilitate the application’s build, design, hosting, and management for TodoPlus.

# Business Needs

TodoPlus aims to create a task management application that’s dependable, user-friendly, and accessible to anyone at any time or place. The application must handle millions of concurrent users without compromising service quality. Additionally, given the sensitive nature of the information, security is a significant concern. Lastly, TodoPlus must be able to scale its application offering to support users globally and anticipate uptake from end users, backed by the capabilities of its in-house marketing team, all while keeping operational costs under control.

# How the Cloud Meets These Needs

TodoPlus knows that the application will need to be hosted on a cloud platform that can offer the required levels of security, scalability, and fault tolerance and be cost-effective. Let us look at how the AWS cloud can fulfill these core underlying business requirements:

- **Security**: Cloud providers offer a range of built-in security features, including data encryption, two-factor authentication, firewalls, and more. These features enable TodoPlus to protect its users’ sensitive data.
- **Scalability**: The cloud provides elasticity, allowing TodoPlus to adapt to demand. If the number of application users increases, TodoPlus can scale up server capacity to meet this demand. Conversely, during periods of low demand, they can scale down resources to minimize costs.
- **High Availability**: Cloud computing services offer high availability by replicating data and applications across multiple data centers in different regions. This means that even if one data center fails, the TodoPlus application will still be accessible to its users.
- **FinOps**: With the cloud’s pay-as-you-go pricing model, TodoPlus only pays for the resources it uses. This allows the company to control its costs effectively. Moreover, many cloud providers offer cost analysis tools to help TodoPlus monitor and optimize their cloud resource expenses.

In summary, the cloud enables TodoPlus to create a task management application that meets its business needs regarding security, scalability, high availability, and effective financial management.

As we move to the next section, you will delve deeper into the fundamentals of cloud computing and explore the extensive capabilities of the AWS Global Infrastructure, providing a solid foundation to understand how it can be harnessed for applications such as our TodoPlus use case.

# Introduction to Cloud Computing and the AWS Global Infrastructure

As discussed previously, computing refers to on-demand access and delivery of IT services, which customers can consume over the standard public internet or some form of wide-area network. These services will include compute, network, storage, databases, and **Software as a Service (SaaS)** products. Cloud computing has enabled businesses to design and deploy applications without requiring expensive hardware upfront. Instead, they lease/rent required IT infrastructure from such third-party providers.

Of the various providers of cloud computing services, AWS is the largest provider, offering a variety of cloud IT services. These services fall into various categories: **Infrastructure as a Service (IaaS)**, **Platform as a Service (PaaS)**, and **Software as a Service (SaaS)** solutions. Businesses can consume those services rather than create their own dedicated environments to host applications. With established infrastructure across multiple countries and massive economies of scale, AWS can offer infrastructure and software service at a fraction of the cost, with redundancy, scalability, high availability, and security.

In the following section, you will learn about the AWS Global Infrastructure, which enables customers across the globe to access AWS services. Furthermore, customers in one location can consume cloud services worldwide, enabling access to a global customer base, and fulfilling any compliance or regulatory needs.

# AWS Global Infrastructure

The **AWS Global Infrastructure** is a vast collection of data center facilities across multiple countries globally. The geographical locations where AWS hosts its data center services comprising compute, storage, and network, along with its vast array of cloud services, are known as **AWS Regions**. You will find the map at `https://aws.amazon.com/about-aws/global-infrastructure/`.

Within each AWS region are small groups of data centers that are logically and physically separated by a distance that falls within 100 **kilometers (km)** (60 miles) of each other. These logically and physically separated groups of data centers form what we call **Availability Zones (AZs)**. Furthermore, AWS designs its regions with multiple AZs per region. Most AWS regions have at least three AZs, and some have even more. For example, the North Virginia region (us-east-1) has six AZs.

In the next section, you’ll look at the core components of the AWS Global Infrastructure in more detail.

# Regions

As explained earlier, AWS regions are physical locations across the globe where AWS hosts its infrastructure facilities. These comprise data centers designed to enable customers to access a vast collection of infrastructure services with which they can deploy cloud resources, such as compute, network, storage, and database services. Customers can connect to a given region anywhere across the Global Infrastructure.

Choosing a region to provision cloud resources ultimately depends on the use case of the business. Often, this will be based on multiple factors, including the following:

- The requirement to host infrastructure resources closer to your end users, where you can host your applications with reduced network latency.
- The requirement to host infrastructure within political and national borders to adhere to strict data sovereignty and compliance regulations.
- The requirement to isolate groups of resources from each other to facilitate disaster recovery and business continuity use cases.

> **Note**
>
> In the case of our fictitious client, TodoPlus, the initial offering for the application will be based in the US to fulfill compliance and regulatory requirements for storing data within the US borders. Should the product be successful, TodoPlus would be looking to expand into Europe and Asia Pacific once all necessary laws and regulatory requirements have been analyzed, and measures are taken to adhere to them.
