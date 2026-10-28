
We have three main features that emerge from this scenario, thanks to redundant servers: we ensure availability, the creation of several replicas of data, as well as an evolution in terms of the partitioning of services ensuring reliability. Partitioning is a key feature of the reliable services programming model. Service Fabric is undoubtedly an excellent option for hosting and running the most critical software solutions.

## The differences between Service Fabric and Kubernetes

Containers are used in cloud-native applications. This technology has impacted the software development industry and has changed how systems are designed, operated, distributed, and monitored. If we have multiple containers to manage, we need an orchestrator so that we’ll have various container orchestrators. One of the main features of an orchestrator is container life cycle management, such as container provisioning, the scaling of existing containers, and resource allocation. Kubernetes is one of the most widely used container orchestrators in the industry.

The main difference between Kubernetes and Service Fabric is that Service Fabric has three mechanisms to deploy and run applications:

- **A proprietary programming model that you can optionally use in your applications**: This programming model allows you to build a stateful or a stateless reliable service, and also reliable actors. The actor model is based on having distributed objects that communicate with each other through messages.
- **The use of containers**: Both Linux and Windows containers are supported.
- **The use of any service built with any platform and any language, with no modification required at all**: Kubernetes supports both Linux and Windows containers. Likewise, Microsoft Azure includes Azure Kubernetes Service, which is a fully managed platform for Kubernetes.

## The Azure Service Fabric development environment

We have many different options to adopt Service Fabric. Service Fabric is a free, open source project whose source code is available on GitHub. Service Fabric supports three different execution environments:

- **On-premises environment**: You can install Service Fabric locally on your servers or VMs. This scenario is suitable for the first cloud migration of your workloads.
- **Public cloud**: Azure Service Fabric allows cluster creation. Note that we have the same Service Fabric functionalities as the previous environment because we can use the same runtime and SDK.
- **Public cloud with other providers**: We can install Service Fabric on any VM using any operating system.

We can consider another environment using containers as if they were nodes of a cluster. As we said before, Service Fabric is open source so it is free. However, if you deploy to a cloud provider, then you pay for the computing resources that you provision.

Let’s now talk about the development environment and the prerequisites. We will present the different tools used to build and deploy a .NET application. The Service Fabric SDK also supports the Java programming language and is able to host any executable, regardless of the platform or language used to build it.

We must meet these prerequisites in order to be able to configure the development environment to create a Service Fabric cluster:

- Windows 10 or 11, which supports Hyper-V, is required for Docker installation
- Visual Studio 2022 or 2019, or Visual Studio Code
- The .NET Core SDK
- Microsoft Service Fabric SDK
- Docker tools for Windows
- The Azure CLI

You can download Visual Studio from https://visualstudio.microsoft.com/. The Community Edition is always free to use and includes Service Fabric templates. You can also use the other editions.

To download and install the Service Fabric SDK, use this link: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK.

You can download Docker for Windows from the Docker website.

Use this link to install Docker Desktop and to create an account in Docker Hub: https://www.docker.com/get-started/.

Finally, we will install the Azure CLI tool; you can use this link to install it for Windows: https://aka.ms/installazurecliwindows.

Now, we have all the required software to build a Service Fabric application.

Let’s check the installation now. We will press Start and type fabric to search for Service Fabric Local Cluster Manager. Then, press Enter in order to set up a local cluster as presented in the following figure:

![](img/66aceec30d3e7dead2f71bb68a887688_133_0.png)

Figure 5.4 – Service Fabric Local Cluster Manager

From the system tray, we will right-click the Service Fabric icon, as presented in the next screenshot. Then, we will navigate to Setup Local Cluster to select the number of nodes: we have the 1 Node or 5 Node options for a Windows or Linux environment. When we select the number of nodes to use, we wait for a few seconds until we see the Service Fabric Local Cluster Manager setup completed successfully notification.

You can create an application using the Service Fabric template in Visual Studio 2022. Select any application type, then compile it, and the application will be added to the Solution Explorer. It’s another method to verify whether your cluster is configured or not.

![](img/66aceec30d3e7dead2f71bb68a887688_135_0.png)

Figure 5.7 – Service Fabric template in Visual Studio 2022

We can deploy any application locally and we can debug the clusters using Service Fabric Explorer. Service Fabric Explorer is a monitoring tool used by developers. We can also deploy our cluster remotely on Azure Service Fabric. We will see in the next section how we can create an Azure Service Fabric cluster.

## Exercise 1 – creating a Service Fabric cluster using the Azure portal

In Azure, we will navigate to the main page of the Azure portal. From there, we will select the Create a resource button, and then we will search for Fabric. Or, just select Containers on the left of the window and, in the end, we will find Service Fabric Cluster, as shown in the following figure. Let’s select Service Fabric Cluster, then click on the Create button.

### Create Service Fabric cluster

Home > Create a resource >

### Project details

Select a subscription to manage deployed resources and costs. Use resource groups like folders to organize and manage all your resources.

| Field | Value |
|---|---|
| Subscription * | MVP |
| Resource group * | packtrg (Create new) |

### Cluster details

| Field | Value |
|---|---|
| Cluster name * | hospitalmanagement.eastus2.cloudapp.azure.com |
| Location * | (US) East US 2 |
| Operating system * | Windows Server 2019 Datacenter with Containers |
| Username * |  |
| Password * |  |
| Confirm password * |  |

Figure 5.9 – Step 1 – Create Service Fabric cluster

Now, we will complete the cluster configuration related to the primary node. We will set up the initial VM scale set capacity, then we will define the node types. Node types allow you to define different hardware configurations and also allow you to specify a different scale. In our case, we will select the default values.

### Primary node type details

Node types define the scale sets that will be used to manage your cluster. A Service Fabric cluster can consist of more than one node type. In that event, the cluster consists of one primary node type and one or more non-primary node types. Configure your primary node type here. Learn more about node types.

| Field | Value |
|---|---|
| Initial VM scale set capacity * |  |
| Node types * | Standard D2s v3 (Select VM size) |

### Security

Service Fabric requires a primary certificate to authenticate the nodes in a cluster and provide secure communication. This certificate must contain a private key and have a subject name that matches the domain used to access the cluster. You can create a new certificate now or select an existing one from an Azure Key Vault. The certificate must be stored in an Azure Key Vault in the same subscription and region as the cluster. Learn more about Service Fabric security.

| Field | Value |
|---|---|
| Key vault and primary certificate * | Select a certificate |

Figure 5.10 – Step 2 – Create Service Fabric cluster

Now, in the Security blade, we’re required to select or create an Azure Key Vault resource where we will store certificates. So, let’s click on this option and we will create a new key vault. Let’s name this servicefabrickeycert, for example. We will select the same resource group as we selected for Service Fabric. After, we will click on the Create button. So, now the Azure portal will deploy this Azure Key Vault resource because it’s required in order to create the certificate and store it there.

![](img/66aceec30d3e7dead2f71bb68a887688_138_0.png)

Figure 5.11 – Select certificate from Azure Key Vault

After the key vault is provisioned, you will see a message that says that it’s not enabled for deployment, so we need to click on the Edit access policies button. The key vault has to be enabled for deployment.
