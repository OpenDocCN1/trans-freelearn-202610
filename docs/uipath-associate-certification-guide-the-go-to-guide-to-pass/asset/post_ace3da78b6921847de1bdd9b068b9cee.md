
## Home screen and Resource Center

The Home screen presents a dashboard that gives you a high-level view of the Robot and Studio licenses, license usage, recently assigned tasks in Action Center, recently executed processes, and access to Orchestrator services. The latest version of the Cloud platform may look slightly different than Figure 3.1. In addition, the Community Cloud may also look different compared to the Enterprise version, as some of the features are restricted in terms of the environments based on the license type.

![](img/84b06ad2832c53c5f677f8ce4c80f184_68_0.png)

Figure 3.1 – Automation Cloud Home screen

As shown in Figure 3.1, the Home screen also gives you access to additional resources such as a download link to download UiPath Studio

## Actions

Actions give you the ability to manage and attend to all the tasks robots create for humans to verify. Figure 3.2 illustrates what Action Center tasks look like in the UiPath Cloud platform:

![](img/84b06ad2832c53c5f677f8ce4c80f184_69_0.png)

Figure 3.2 – Automation Cloud Actions screen

The **Actions** page shows all the completed tasks by users, including tasks that are Pending, Unassigned, and Completed. Users can also assign tasks to themselves or transfer them to a different user for further action.

## Apps

The **Apps** screen provides users with the ability to build their applications. **UiPath Apps** is a low-code platform that allows any user to quickly create a web interface by dragging and dropping the elements needed. Applications designed through Apps interact with the robots to execute automated processes using the data provided by the user through the application.

Apps helps to present a simple single interface for users to interact with complex automation solutions that require interaction with multiple applications behind the scenes. Figure 3.3 illustrates the Apps screen. Apps is not part of the Associate certification; however, we wanted to provide a glimpse of the other interesting features that are available.

![](img/84b06ad2832c53c5f677f8ce4c80f184_70_0.png)

Figure 3.3 – Automation Cloud Apps screen

As illustrated in Figure 3.3, UiPath Apps can create multiple applications seamlessly for different needs. Users can publish and share the application with other users who require access and the ability to utilize the power of automation in their day-to-day activities.

## Processes

The **Processes** page provides easy access to all automated processes that robots execute in Orchestrator, without *actually* logging in to Orchestrator. However, configuring these automated processes is required to be done through UiPath Orchestrator. _Figure 3.4_ illustrates a sample **Processes** page available in the UiPath Cloud platform. Although processes are part of the certification program, what is shown in the **Processes** section is not a part of the Associate certification.

![](img/84b06ad2832c53c5f677f8ce4c80f184_71_0.png)

Figure 3.4 – Automation Cloud Processes screen

As shown in _Figure 3.4_, the **Processes** page shows all available processes and a few additional details such as the version, last run date, and package feed, which the user can click and execute.

## Data Service

Data Service offers a platform for users to store the data passed through automated processes. This service provides cloud-based secure storage to store data in a relational model where robots can easily access the information to process. The goal of the Data Service is to provide a single point of storage for the data processed by the robots. The Data Service is an interesting feature to explore; however, it is not a part of the Associate certification.

![](img/84b06ad2832c53c5f677f8ce4c80f184_72_0.png)

Figure 3.5 – Automation Cloud Data Service screen

Figure 3.5 showcases some sample storage entities created in the platform to store data. The Entities page summarizes each entity, by displaying the description, number of columns, number of records in each entity, and the last modified date.

## AI Fabric

AI Fabric allows users to deploy and train AI models available out of the box or have custom-built AI models run within automation solutions. The AI models created in AI Fabric seamlessly integrate with the UiPath automation solutions built through UiPath Studio to provide intelligence to the tasks performed by robots. Figure 3.6 gives you a general idea of what the AI Fabric main screen looks like. Building and connecting with AI models is interesting; however, there are more advanced features. As a result, AI Fabric is not included in the Associate certification.

![](img/84b06ad2832c53c5f677f8ce4c80f184_73_0.png)

Figure 3.6 – Automation Cloud AI Fabric screen

All AI Fabric projects created will be listed on the main landing page of the AI Fabric platform.

## Admin

The Admin page includes several sections that allow you to configure the services, licenses, user access, and organizational settings. The functions you see on the Admin page differ based on the version (Community or Enterprise) and the current release.

## Tenants

The **Tenants** page includes the instances of different applications that you use in your cloud platform. One of these applications is UiPath Orchestrator. Currently, the **Tenants** page only supports instances of the Orchestrator application.

Figure 3.7 provides a glimpse of how the **Tenants** page appears in Automation Cloud. The **Tenants** page may include more than one Orchestrator tenant, thereby enabling multitenancy. By having multiple tenants, users are able to separate automation solutions into different environments. One of the best examples of multitenancy is the use of separate tenants for development, QA, and production.

![](img/84b06ad2832c53c5f677f8ce4c80f184_74_0.png)

Figure 3.7 – Automation Cloud Tenants screen

As shown in **Figure 3.7**, the **Tenants** page lists all Orchestrator instances you have under your organization's instance. The page also allows users to edit the allocation of licenses to the tenants and manage the other services used, along with Orchestrator instances such as Action Center, Data Service, and AI Fabric.

## Users and Groups

User and group management is one of the key features provided in Automation Cloud. The Accounts and Groups page gives you access to manage users and groups by creating users, customizing permissions at the organizational and service levels, inviting new users, and deleting existing users. Figure 3.8 illustrates what the Users tab looks like in Automation Cloud:

![](img/84b06ad2832c53c5f677f8ce4c80f184_75_0.png)

Figure 3.8 – Automation Cloud user management screen

In addition to the mentioned features, the Accounts and Groups page allows the bulk inviting of users through the Invite Users option.

## Licenses

License management occurs under three different categories: Community Plan, Enterprise Trial, and Enterprise License Plan. The Licenses page displays the license plan assigned to you, along with the number of robots and other services you have access to in the Cloud platform.

![](img/84b06ad2832c53c5f677f8ce4c80f184_76_0.png)

Figure 3.9 – Automation Cloud Licenses screen

As shown in Figure 3.9, the licensing information is displayed under three different tabs:

- The Robots tab illustrates how many attended, unattended, testing, and non-production robot licenses are available for the Cloud platform.
- The Studios tab displays the available named user Studio, Studio X, and Studio Pro licenses for the user.
- The Other Services tab showcases the license availability for services such as AI Fabric, Computer Vision, GPUs, and Document Understanding.

## Audit Logs

The Audit Logs page displays all the logs related to user actions, such as new user creations and membership plans. Figure 3.10 illustrates the Audit Logs page:

| Category | User | Action | Operation | Time |
| :--- | :--- | :--- | :--- | :--- |
| User | saurabh kadave | Login | User 'saurabh kadave saurabh.packt@gmail.c... | 6 minutes ago |
| User | saurabh kadave | Login | User 'saurabh kadave saurabh.packt@gmail.c... | a day ago |
| User | saurabh kadave | Login | User 'saurabh kadave saurabh.packt@gmail.c... | a day ago |
| RobotAccount | saurabh kadave | Create | {"adminName": "saurabh kadave", "users": "R... | a day ago |
| User | saurabh kadave | Login | User 'saurabh kadave saurabh.packt@gmail.c... | a day ago |
| User | saurabh kadave | Login | User 'saurabh kadave saurabh.packt@gmail.c... | 8 days ago |

Figure 3.10 – Automation Cloud Audit Logs screen

The **Audit Logs** page also allows the user to download all the audit logs for auditing purposes. Additionally, the page will enable you to be compliant with the organization’s rules and regulations by tracking user actions.

## Organization Settings

The **Organization Settings** page provides the capability to change Orchestrator URLs, the Automation Cloud platform name, and language settings.

- Automation Cloud houses instances of UiPath Orchestrator and many other services. Knowing the Automation Cloud platform helps the user to easily configure and provision Orchestrator instances within minutes.
- Orchestrator uses the licenses available in the Automation Cloud platform to connect with robots and run automation processes. Each Orchestrator created in Automation Cloud is assigned several licenses to perform the required actions.

The next section of the chapter illustrates how to create Orchestrator instances on the Automation Cloud platform.
