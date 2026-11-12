
## Introducing UiPath Orchestrator

UiPath Orchestrator is the heart of the entire automation suite, which manages all the published automation solutions developed in Studio, the execution of tasks by robots, and the execution schedules. Orchestrator is available as a cloud platform or as an on-premise solution. It offers many capabilities for its users. Some of those capabilities are as follows:

- **Provisioning**: Creating new robots and maintaining existing robots and users.
- **Deployment**: Deploying workflows created in UiPath Studio for execution on demand or using schedules to ensure efficient planning and execution.
- **Configuration**: Creating and maintaining user groups, robots, and user access for processes and other resources.
- **Monitoring**: Tracking the status of executed processes, robot status, and user permissions.
- **Logging**: Stores data related to executions, robots, audit logs, and other actions in a SQL database or Elasticsearch.
- **Queues**: Acts as an intermediary data storage and processing system for transactional data through queues by prioritizing the transactions processed. Each transaction is treated and monitored separately during the execution, ensuring the overall process is not affected by any transaction errors.
- **Integrated Connectivity**: Enables secure connections for communication between third-party services through API or other integration methods.
- **Version Control**: Orchestrator maintains the versions of the packages and libraries published. Orchestrator also allows the use of different package versions across multiple robots for execution.

UiPath Automation Cloud enables the users to manage the virtual workforce. To gain the best out of the capabilities that Orchestrator offers, understanding the key fundamentals is essential.

## Understanding the fundamental concepts of UiPath Orchestrator

There are a few fundamental concepts that everyone needs to know before using UiPath Orchestrator. Some of these fundamental concepts are listed as follows.

### Robots

**Robots**, also known as **execution agents**, execute the workflows created in UiPath Studio. Orchestrator manages these execution agents through the **Robots** page. The agent is installed separately or with Studio, depending on the license model. There are different types of robots available in Orchestrator. At the top of the Robot type hierarchy, Orchestrator categorizes robots into two categories, as shown in Figure 3.11:

| Robot Type | Description |
|---|---|
| Standard | Configured when the machine that the robot works on does not change |
| Floating | Configured when the machine that the robot works on changes frequently or when the same user works on multiple devices |

Figure 3.11 – Robot types in UiPath Orchestrator

Orchestrator allows the creation of either type according to how the robot allocation should occur. Each type mentioned is categorized into four additional robot types according to how the robot is triggered and the environment installed, as shown in Figure 3.12:

| Robot Type | Description |
|---|---|
| Attended Robot | Is it triggered manually by the user or through user events. The robot agent runs in the same machine where the human user works, assisting the user's daily activities. |
| Unattended Robot | Runs in virtual machines and executes processes without any human intervention. The Orchestrator manages the unattended robots by controlling the executions, monitoring, and managing the data through queues. |
| Development Robot | Features are the same as the unattended robot but only used to connect the UiPath Studio to Orchestrator and for development purposes. |
| Non-Production Robot | Used only for development and testing purposes. |

Figure 3.12 – Robot types in UiPath Automation Cloud

There can be more types other than these; however, for the purpose of the Associate certification, we will focus only on the robot types referenced in the preceding table.

### Folders

The use of **folders** enables the user to better organize the processes, robots, assets, and queues for better management and access control. UiPath Orchestrator supports **Modern** and **Classic** folder types, each with a range of features.

Modern folders offer the following features:

- They allow the dynamic allocation of robots and user roles.
- They support a hierarchical structure where you can have up to six subfolders under each first-level folder.
- They enable large-scale automation initiatives by easily integrating with Windows Active Directory.
- Each folder has its package feed to store packages related to the context of the folder.

Classic folders do not have the same features that Modern folders offer. The following outlines the limitations of Classic folders compared with Modern folders:

- They contain robots and environments in the traditional way.
- They cannot create or maintain subfolders.
- Robots created in Classic folders can only work within the scope of the folder.

One important point to mention here is that Classic folders are a part of the Associate v1.0 certification. However, users may not be able to use them in the Community version as this feature is discontinued. The latest version of Orchestrator only uses Modern folders.

Folders are created and managed at the tenant level of Orchestrator. Figure 3.13 illustrates the **Folder management** page of Orchestrator:

![](img/84b06ad2832c53c5f677f8ce4c80f184_81_0.png)

Figure 3.13 – Folder creation in UiPath Orchestrator

Folders provide better control over what users see and control. The folder management screen grants access to the folder’s resources for different users with different permission levels. Orchestrator shows Classic folders in red, and Modern folders in blue. In addition, Modern folders allow the creation of subfolders for the further division of resources, whereas Classic folders do not allow subfolders.

On the other hand, there is another type of folder, named "MyWorkspace." Developers mainly use this option to separate their development work from other users. MyWorkspace is unique and other users cannot access another user’s workspace. MyWorkspaces offer the same functionality as other folders, although they are restricted to the assigned users.

### Environment

The Environment feature in UiPath Orchestrator allows the grouping of robots. Grouping was the traditional method of categorizing robots before the introduction of folders. Grouping is usually done based on the business groups that execute their automation solutions. For example, a Human Resource department may have multiple user groups running their automation solutions that are disconnected from the rest of the department. Each environment may consist of multiple robots, and a robot can be a part of more than one environment. The management of environments is handled through the Environment page in Orchestrator. Environments are only available in Classic folders.

### Organizational units

UiPath Orchestrator allows the creation of multiple business units within the same Orchestrator instance. Each instance will consist of its dedicated environments, queues, assets, and robots. The separation of business units ensures the proper organization of processes, robots, and the security of the data and processes available in Orchestrator.

### Packages

The published workflow solutions are known as packages. Packages can be published both locally and in UiPath Orchestrator. Uploading packages to Orchestrator can be done directly through UiPath Studio or manually through the Orchestrator UI. Orchestrator manages the versions of the packages deployed and used within Orchestrator. The Packages screen of Orchestrator allows the management of all packages and their versions.

The Packages screen displays the release notes or upgrade packages depending on how they want to use them. Also, the same package management display allows the users to maintain the history of release versions by deleting or keeping track of release information for better visibility of changes. The usage of a package determines the status of the package as Active or Inactive. If the package is used in one or more processes, the status of the used package version is set as Active. Figure 3.14 shows the Packages page with all the available packages:
