
## Create a new ASP.NET Core web application

ASP.NET Core 6.0

- **ASP.NET Core Empty**
  An empty project template for creating an ASP.NET Core application. This template does not have any content in it.
- **ASP.NET Core Web API**
  A project template for creating an ASP.NET Core application with an example Controller for a RESTful HTTP service. This template can also be used for ASP.NET Core MVC Views and Controllers.
- **ASP.NET Core Web App**
  A project template for creating an ASP.NET Core application with example ASP.NET Razor Pages content.
- **ASP.NET Core Web App (Model-View-Controller)**
  A project template for creating an ASP.NET Core application with example ASP.NET Core MVC Views and Controllers. This template can also be used for RESTful HTTP services.
- **ASP.NET Core with Angular**
  A project template for creating an ASP.NET Core application with Angular
- **ASP.NET Core with React.js**
  A project template for creating an ASP.NET Core application with React.js

Authentication
None

Advanced
Configure for HTTPS
Do not use top-level statements

Get additional project templates

Author: Microsoft

Back Create

Figure 5.21 – Create a new ASP.NET Core web application with Service Fabric

We will explore the solution tree. We have two projects. The first project is the application itself and the second one is the service.

![](img/66aceec30d3e7dead2f71bb68a887688_147_0.png)

Figure 5.22 – The hospital management solution structure

In the application, you will find .xml files that represent metadata about the application. For example, inside ApplicationPackageRoot, we will find the ApplicationManifest.xml file, which represents the entire application because it includes the reference to all the services that constitute the application. In the PackageRoot folder inside the HospitalService project, you will find the ServiceManifest.xml file. Each service has its own manifest. The ServiceManifest.xml file contains a ConfigPackage element. Find the Resources element and the endpoints that this service will listen with. In this case, we only have a single endpoint.

![](img/66aceec30d3e7dead2f71bb68a887688_148_0.png)

If we check our application in ASP.NET Core, in Program.cs, the service is registering itself to Service Fabric using this line of code:

```
ServiceRuntime.RegisterServiceAsync("HospitalServiceType",
    context => new HospitalService(context)).
GetAwaiter().GetResult();
```

The HospitalService class is inheriting from StatelessService, which is one of the base classes in the Service Fabric API. We have two methods for listeners. The first one is CreateServiceInstanceListeners. It is returning a collection of listeners, ServiceInstanceListener, based on Kestrel. Kestrel is the web server included in .NET Core. In other words, this service is able to receive HTTP requests.

## Deploying the application in a local cluster using Visual Studio

To deploy the application in a local cluster, we will use the **Publish** tool of Visual Studio. Right-click on the application with the **Service Fabric** icon and select **Publish**. In the target file, we will select the number of nodes configured in our cluster. In this case, we configured a single node in the connection endpoint. Ensure that your application will publish to a local cluster. In the end, the .xml files that include the application parameters and the startup service parameters are requested to finish the publishing process.

![](img/66aceec30d3e7dead2f71bb68a887688_149_0.png)

Click on the **Publish** button, then we will check our application in Service Fabric Explorer.

![](img/66aceec30d3e7dead2f71bb68a887688_150_0.png)

Figure 5.25 – Service Fabric Explorer – application deployment

A solution includes multiple services. To fully take advantage of the Service Fabric platform, we need to communicate from one service to another. To do this, we can implement an interface or API, for example. In this case, the interface is the public-facing API for all the services that want to invoke the hospital’s functionality. Another service, such as a search service, can use the same interface. Both services are going to reference the interface. In a microservices architecture, it’s perfectly valid to invoke one service directly from another. You can do this through the HTTP protocol and by exposing an API from the target microservice.

## Exercise 5 – deploying an app to a Service Fabric managed cluster and containers

When the application is tested and deployed locally, we can publish it remotely to the Azure Service Fabric instance that we created previously. To do that, we will use the same publishing tool in Visual Studio.

## Deploying an ASP.NET Core application to Azure Service Fabric

Right-click on the application, then select **Publish**. In the target profile, we will select `PublishProfiles\Cloud.xml`. We will select the account related to the Azure portal. In the connection endpoint, we can select an already created Azure Service Fabric cluster or you can create your cluster using Visual Studio. To do that, we will click on **Add Custom Endpoint**.

![](img/66aceec30d3e7dead2f71bb68a887688_151_0.png)

Figure 5.26 – Add Custom Endpoint

Next, we will add value to the endpoint. After following the previous steps to create the cluster, we will click on **Create a new Cluster**. The following figure shows the window for creating a Service Fabric cluster:

Figure 5.27 – Create Service Fabric Cluster

We need to define the cluster name, select the subscription and location, define the number of nodes, and select an existing resource group or create a new one. We need to fill in all of the information for every tab. For the Certificate tab, we need to add the certification information needed. In VM Detail, we will configure the VM that is used by the cluster. In Advanced, we need to add ports. After that, we will click on Create.

A new cluster will be created. At the bottom of the window, we need to select the application parameter files and the startup service parameter files that will be used for the cloud, so we will select Cloud.xml. This configuration ensures that each service is going to be deployed to all the nodes in the cluster. After this, we will click on Finish.

## Building and executing a Docker container in Service Fabric

Another application model supported in Service Fabric is the use of Docker containers.

Containers are an ideal complement to the microservices architecture, thanks to the fact that they are isolated and lightweight processes.

Let’s create a new Service Fabric application. First, we need to select a container template, set the service name, and specify the container image name. The image has to be reachable. The container image can be a public image in Docker Hub, for example, or a private image in Azure Container Registry or any private registry. In this case, we need to add a username and a password. You can also specify the host port and the container port that you want to use; for example, we can use port 80. Then, we will click on **Create**.

![](img/66aceec30d3e7dead2f71bb68a887688_153_0.png)

This type of application model doesn’t add a service project since you won’t write any code. This is because all the code is already inside the container. If we expand the service manifest for the package, the `ContainerHost` element contains the image name. Optionally, you can also set environment variables in the container using the `EnvironmentVariables` element. Locally, we can deploy this type of application in the local cluster. Remotely, you can deploy the application to Azure Service Fabric.

If you need to learn about creating a container, you can check out this book: *A Developer’s Guide to Cloud Apps Using Microsoft Azure* by Packt Publishing.

## Summary

In this chapter, we talked about the Azure Service Fabric platform. We explored the different elements of Service Fabric, such as clusters and nodes, the main difference between Service Fabric and Kubernetes, and we prepared a development environment to create and deploy an application.

We created a Service Fabric application using the Azure portal and the Azure CLI, we learned about scaling an Azure Service Fabric cluster, and we discovered more about the different models of Service Fabric template in Visual Studio 2022. We also created a .NET Service Fabric application, and we built and executed a Docker container in Service Fabric.

To publish our applications to Azure Service Fabric, we used the **Publish** tool in Visual Studio.

Applications use a database. In the next chapter, we will learn about the different classifications of data and how data is defined and stored. We will identify the characteristics of relational and non-relational data and the different Azure services for databases.

We will also explain the use of Azure Storage, Azure SQL Database, and Azure Cosmos DB, or a combination of them, to determine the operational needs, latency requirements, and transaction requirements of your data.

## Questions

- 1. What is an Azure Service Fabric cluster?
- 2. What is the difference between Kubernetes and Service Fabric?
- 3. How do we deploy a containerized application on Azure Service Fabric?
