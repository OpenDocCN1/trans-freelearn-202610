# Definition

Azure Service Fabric allows you to create modern, resilient, and scalable software solutions, with features required to build a robust system, such as autoscaling, health monitoring, and fault tolerance.

The Service Fabric platform offers a programming model that you can use to create reliable services or components based on the actor model. For containerized applications, Service Fabric also supports Docker containers so that you can use the platform as an orchestrator. Azure Service Fabric is able to run on any operating system (Windows or Linux). Additionally, it can run in any environment, on-premises as a private cloud or on a public cloud such as Azure or any other cloud provider, such as AWS or Google Cloud Platform, or in the local development environment of a developer. It offers a complete set of development tools and software development kits (SDKs) to start building solutions on the platform very quickly. These tools can be used in a development environment or in a production environment. The cloud has allowed the evolution of software architectures and development platforms for distributed systems. Azure Service Fabric is not new. It is an open source project that is used in many services in the Azure infrastructure as well as other Microsoft services. In fact, it has existed since the beginning of Azure, when it was still called Windows Azure. Within Microsoft, this platform is strategic and fundamental since many cloud services use it, such as Azure SQL Database, Azure Cosmos DB, Azure DevOps, Dynamics 365, Power BI, and many more. The following diagram depicts the different Microsoft services and tools that use Service Fabric.

![](bbox=[0.115, 0.475, 0.885, 0.735])

Figure 5.2 – Powering Azure and Microsoft services with Service Fabric

But how does Azure Service Fabric work? In the next section, we will present the core functionality of Azure Service Fabric, which is based on two essential concepts: clusters and nodes.