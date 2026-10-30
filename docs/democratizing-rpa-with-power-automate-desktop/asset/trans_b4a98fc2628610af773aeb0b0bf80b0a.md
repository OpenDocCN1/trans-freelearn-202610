

which are then managed via the console. For this flow, a simple Windows machine was created.

The UI flow shown in the following figure logs into AWS with the credentials (1), then calls the available EC2 instances and stores them in a variable (2).

The subsequent loop goes through all instances (3) and checks each one to see whether it is stopped or not (4). If stopped, a message box asks whether the machine should be started (5). This is what the flow looks like:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_282_0.png)

Figure 9.18 – Working with AWS EC2 instances

## Working with Azure actions

In the first section of this chapter, we saw that Azure is the foundation for all Microsoft Cloud services. To explain the concept of Azure without going too much into the details, take a look at the following diagram:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_283_0.png)

Figure 9.19 – Azure relationship between different components

The tenant is equivalent to an organization or a company. Typically, the tenant contains an Azure Active Directory where we can define and register new applications that need to access resources within the tenant.

A subscription is a financial boundary for any kind of resource. Because resources consume CPU and/or memory, there must be financial coverage of the consumption. There can be multiple subscriptions within a tenant.

The next level is a resource group, which usually relates to a specific purpose or topic. For example, it could be possible to create a resource group that contains all resources for automating a specific workload or application. These resources can be storage (disk or volume to persist data) or virtual machines and many more types of resources.

For PAD to be able to access resources in Azure, we need to register a new app within Azure Active Directory. This would have a new client ID as a result, which we can use for the corresponding field in the Actions Parameter dialog. Within this app registration, it is also possible to create a client secret, which is a fixed password to use for logging in with this client. In the Azure portal, go to Azure Active Directory | App registrations and create a new registration:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_284_0.png)

Figure 9.20 – Azure Active Directory – App registrations

Together with the Tenant ID (or Directory (tenant) ID in the preceding screenshot) and Subscription ID properties (needs to be looked up in the subscription), we have all the information for the Azure session to log on:

### Create session
Creates an Azure session using service principal More info

### Select parameters
Client ID :

Client secret:

Tenant ID:

Subscription ID:

Variables produced AzureClient

On error

Save

Cancel

Figure 9.21 – Creating a connection to Azure

### Configuring additional security settings

For the Azure session to work, additional security-related steps must be taken. First, the app registration must be granted API permissions for Azure resources to access. These include permission for Azure Service Management and Azure Storage. Second, the app registration must be given additional role assignments at the subscription level. For this, go to the subscription management in the Azure portal, switch to the Access Control (IAM) area, select the Role Assignment tab, and use the +Add button to start the wizard. Select the Virtual Machine Contributor role and add the app registration that was previously created as a member of the role assignment.

The following example shows how to work with actions for Azure virtual machines and disks:

the background. Finally, the new disk is attached to the VM and is available directly in **Disk Management** as an unformatted drive.

As a consequence, other scenarios can be feasible. For example, it would be possible to fill a new disk with data from a snapshot or another storage account.

Furthermore, the VM could also be equipped with an agent for virtual desktops so that UI elements can be identified and used in a flow.

In this section, we learned that we can use IaaS providers such as Microsoft or AWS via the native PAD actions to move workloads from on-premises machines to VMs and then control them through a locally executed UI flow.

This can save resources, especially if an application is not used for automation daily or uses a lot of compute capacity (CPU/memory).

## Summary

Through integration with Power Platform, PAD can close the gap between workflows in the cloud and the local network. We have seen that this can create a seamless overall concept for an automation solution, as the various components are very well integrated.

Additionally, we learned about native actions for using IaaS offerings from Microsoft and AWS, which allow us to outsource the execution of desktop programs or add more resources to our flow as needed.

In the next chapter, we’ll look at how we can incorporate artificial intelligence into a UI flow while learning about offerings from various vendors.

## Further reading

Further information is listed here:

- Sign up for a free Dynamics 365 Sales trial:
  https://learn.microsoft.com/en-us/dynamics365/sales/sign-up-for-sales-trial
- Power Platform environments overview:
  https://learn.microsoft.com/en-us/power-platform/admin/environments-overview
- Connection versus Connection references:
  https://learn.microsoft.com/en-us/power-apps/maker/data-platform/create-connection-reference
- Introduction to Amazon EC2:
  https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html
- Amazon Service endpoints:
  https://docs.aws.amazon.com/general/latest/gr/rande.html
- Get started with Amazon EC2 Windows instances:
  https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html
- Register an application with the Microsoft identity platform:
  https://learn.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app

## Leveraging Artificial Intelligence

The topic of **artificial intelligence (AI)** has been hotly debated for some time and has gained further popularity with the advent of OpenAI and ChatGPT. In this chapter, we want to shed light on what possibilities we have within PAD in this regard and look at the following areas:

- An introduction to cognitive services
- Diving into the offerings of Microsoft, Google, and IBM
- Classification in the practical work with PADT

After reading this chapter, we will have a solid understanding of what AI means, in which areas this technology helps optimize business processes, and how these functionalities can be deployed and used with PAD.

## Technical requirements

The services addressed in this chapter are accessed online by the respective providers. This requires a corresponding subscription with the vendors, who all provide a test period with certain credits free of charge. The process for starting a trial and subsequent steps are different in each case and are highlighted in the corresponding sections.

## Introduction to cognitive services and artificial intelligence

The functionalities discussed in this chapter all deal with AI and **machine learning (ML)**. Before we turn to the individual providers and their functionality, we will first classify the topic in general and consider what benefits AI can bring to everyday life and in the business context.

In general, it can be said that AI can help to recognize structures in text, image, or sound information so that they become machine-readable. Cognitive services hereby refer to a set of cloud-based **application programming interfaces (APIs)** and tools provided by Microsoft, IBM, Google, and other vendors that enable developers and creators to add advanced AI capabilities to their applications without needing to have deep expertise in ML or data science. The actions offered in PAD by the different vendors provide more or less the same functionalities, shown in the following diagram. Only the naming of the services is slightly different in some cases.

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_290_0.png)

Figure 10.1 – An overview of cognitive services

Let’s take a slightly closer look at these capabilities and how they can be used in a business context.

## An overview of cognitive services

Cognitive services can also be described as a set of APIs and tools that use ML algorithms to enable computers to perform tasks that normally require human-level intelligence. These services are typically hosted in the cloud and can be accessed through a REST API or **software development kit (SDK)**. The technology behind cognitive services can be broken down into a few key components:

