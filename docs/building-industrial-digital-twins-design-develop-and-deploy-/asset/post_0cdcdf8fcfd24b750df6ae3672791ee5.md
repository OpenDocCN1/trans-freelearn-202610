
The first step is to create a free Azure account that will host your ADT service and any other IoT and analytics services that you will need for your first Digital Twin. See the following screenshot for an overview of how to do this:

![](img/f445915fceb4649a77b6797f1425850b_148_0.png)

![](img/f445915fceb4649a77b6797f1425850b_148_1.png)

Figure 5.7 – Microsoft Azure portal UI

Once you’ve created an account, you will be directed to the Azure service portal. This is the central management environment for all Azure services, including ADT, Azure databases, IoT services, and all the other products that Microsoft provides in its Azure product range.

ADT is a PaaS solution that provides the infrastructure to create and use Digital Twins. It is a development environment for Digital Twins that is focused on developers to code Digital Twins. It is not an end user-focused application with a UI for business users to configure Digital Twins. It will require other Azure services in your Azure account to perform these actions.

ADT can be configured through the web-based Azure Portal or in a command line through the **Azure command-line interface (Azure CLI)**. Both approaches are documented in the official Microsoft documentation at `http://bit.ly/idt-azuredocs`. We will describe the portal configuration approach at a high level for the purpose of the book.

The configuration and setup of ADT and the supporting app service to code against is done in five main steps, as illustrated in the following diagram:

![](img/f445915fceb4649a77b6797f1425850b_149_0.png)

Figure 5.8 – Microsoft ADT resource and application configuration

The first step is to set up an Azure account. Microsoft provides a free account option at the time of writing the book. The second step is to create an ADT resource or service in the Azure account that you’ve created. The third step is to give yourself user and data access to the ADT resource you created. You can also give others access to your ADT service during this step. The ADT resource that you created requires an Azure app resource to be able to code against the ADT service. We configure the app resource during the fourth step. Finally, in the fifth step, we give the app resource access and permissions to the ADT service.

You can see an overview of this in the following screenshot:

![](img/f445915fceb4649a77b6797f1425850b_149_1.png)

**Recent resources**

| Name | Type | Last Viewed |
|---|---|---|
| idtbook | Resource group | 42 minutes ago |
| idtbooktwins | Azure Digital Twins | an hour ago |

Figure 5.9 – Microsoft Azure portal UI

The UI shown in Figure 5.9 indicates that you have successfully created your free Azure account and shows a list of services available at the time of writing this book. The next step in our process is to create a new service for ADT. Select the **+ Create a resource** option. A range of existing resources should show, and in the event that ADT is not on the display, you can easily search for it in the search bar, as shown in the following screenshot:

![](img/f445915fceb4649a77b6797f1425850b_150_0.png)

Figure 5.10 – Creating a new Azure resource

Once you’ve located the ADT resource, click on the **Create** button to start the process of configuring the resource.

Creating the resource has two main components—the first is to choose a subscription that the resource will run in, and the second part is to choose a hosting location and a name for the ADT resource or service.

The following screenshot shows the subscription options, and it is a good practice to name the resource group in an easily identifiable way. For the purposes of building your first Digital Twin, we recommend that you create a resource group for your digital-twin application. You can think of a resource group as a container that holds related resources for an Azure solution. For later reference in the book, we called the resource group digitaltwinbook:

![](img/f445915fceb4649a77b6797f1425850b_151_0.png)

Figure 5.11 – Creating an Azure resource subscription

The second part of the process to create an Azure subscription is to choose a hosting location and name the resources. Hosting locations that provide ADT resources are available from the drop-down list. The decision of a location is most often based on your geographical proximity, to reduce latency. Privacy and data considerations may, however, prescribe your choice if you are in the **European Union** (EU), for example.

The following screenshot shows the location and name that we chose to use for the Digital Twin that we will configure in Chapter 6, Building the Digital Twin Prototype. Clicking on the Review + create button will give you the option to review all the settings before the ADT resource is finally created in your chosen subscription and preferred location:

![](img/f445915fceb4649a77b6797f1425850b_152_0.png)

Figure 5.12 – Choose the Azure resource location and name

All the ADT resource information is available in the portal view, as shown in Figure 5.13. This concludes the first two steps shown in Figure 5.8. The third step is the user access configuration to enable you to access the digital-twin resource when you create your first Digital Twin in Chapter 6, Building the Digital Twin Prototype.

Next, we see the successful creation of an ADT resource in the following screenshot:

![](img/f445915fceb4649a77b6797f1425850b_153_0.png)

Select the **Access control (IAM)** option from the left menu and click on **Add role assignments** to open the configuration pane. This will enable you to give your user and credentials access to the digital-twin resource. Figure 5.14 shows the UI at the time of writing the book.

It is important to set your own login user as the data owner for the digital-twin resource when you set this up in a personal or free account. There may be other governance requirements if you are doing this in your organization’s Azure subscription or tenant; however, we recommend that you do the initial prototype in a free account.

You can see an overview of the UI here:

![](img/f445915fceb4649a77b6797f1425850b_153_1.png)

Select **Azure Digital Twins Data Owner** from the available options in the **Role** dropdown on the right-hand side of the screen. This option will give you full access and ownership to all the capabilities of the ADT resource, as shown in Figure 5.15.

The **Assign access to** option can be left at the default **User, group, or service principal** setting. Select or type in an Azure free account username in the **Select** drop-down field.

Your user will appear as an icon below the drop-down fields. Select your user and save the access rights with the **Save** button on the page, as illustrated in the following screenshot:

![](img/f445915fceb4649a77b6797f1425850b_154_0.png)

Figure 5.15 – Creating a role assignment for your user in ADT

You have now successfully given your user account access, and this concludes the third step of Figure 5.8. Now, have a look at the following screenshot:

inbook

# Access control (IAM)

![](img/f445915fceb4649a77b6797f1425850b_155_0.png)

Figure 5.16 – User access successfully assigned in ADT

The fourth step in Figure 5.8 is to create an Azure app that will be able to interact with the digital-twin service.

It is common practice to interact with an ADT instance through an application such as **ADT Explorer** (`http://bit.ly/adt-adtexplorer`) or your own custom application. These applications need to authenticate themselves to the ADT service by using **Azure Active Directory** (**Azure AD**). This means that you need to create an Azure app service in step 4 of the process in Figure 5.8. Once this is complete, the new app servers will be given access and permissions to the Digital Twin, similar to the user access in the previous step.

## 138 Setting Up a Digital Twin Prototype

This is done from the main page of your Azure portal instance or by choosing **Azure Active Directory** from the menu on the left on the screen, as shown in the following screenshot:

![](img/f445915fceb4649a77b6797f1425850b_156_0.png)

Figure 5.17 – Creating a new Azure AD resource

A screen with app registration options similar to those shown in the following screenshot provides an option to create and manage new applications on Azure. Choose **+ New registration** to start the wizard shown in Figure 5.19:
