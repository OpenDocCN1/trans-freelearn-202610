
## 9

## SOA Suite Cloning

Software maintenance often accounts for 70 percent of overall cost and that includes licensing. Enterprises have to worry about maintaining their software, which includes product updates, security updates, hardware updates, and environment refreshes. These are time-consuming tasks that require human expertise in several areas. As the software ages, an enterprise still needs to maintain this human expertise for legacy software that, while critical to the business, can be expensive and sometimes negate the overall business value by increasing the cost of ownership.

More and more enterprises are turning to automated lifecycle management tools to help with this heavy lifting related to software lifecycle maintenance. It is important to have the right strategy in place to plan for maintenance. Maintenance should be part of the overall strategy, not an afterthought.

This chapter covers:
- Challenges
- Solution
- Step-by-step exercises:
  - Adding an Oracle Application Server Cluster to Grid Control
  - Stopping application server processes
  - Creating a component in the Grid Control Software Library
  - Cloning an Oracle Application Server and SOA applications
  - Validating a newly created cluster

### Challenges

In today's data centers, services often need to be provisioned in short order. These services could consist of complex multitier configurations, such as Oracle Application Server and SOA application software. An administrator has to go through multiple steps to clone an SOA Suite from one installation to another. The steps include cloning copied files, configuring them, and making changes to reflect the new installation. Several Oracle administrators have database cloning skills, but most do not possess the expertise to clone Application Server, let alone the SOA Suite. Manual installations of such services are often time consuming and error prone.

### Solution

Grid Control provides a flexible and scalable way to clone tested images to multiple hosts. Specifically, Grid Control offers a predefined, fully customizable procedure for cloning Oracle Application Server and SOA applications. The procedure enables administrators to either create a new Oracle Application Server instance or Oracle Application Server Cluster based on an existing, reference application server or cluster; thereby automating common IT tasks such as creating a production SOA Suite environment based on a test or stage, or such as extending an existing Oracle Application Server Cluster to accommodate an increase in load.

### Step-by-step exercises

This set of step-by-step exercises will walkthrough cloning an Oracle SOA Suite 10.1.3.x deployed on Oracle Application Server 10.1.3.x.

### Adding an Oracle Application Server Cluster to Grid Control

The first step is to create the application server cluster target in order to clone it later.

![](img/7529bbd615e859608c21b965162348b2_138_0.png)

> In order to create a new cluster via cloning, Grid Control must be aware of an existing, installed cluster from which to clone.

1. From the Grid Control home page, click on the Targets | Middleware tab.

![](img/7529bbd615e859608c21b965162348b2_139_0.png)

In the table, you should see a minimum of two Oracle Application Server targets: one hosting the Grid Control application and the other hosting the SOA Suite, as discussed in previous chapters. While the Oracle Application Server instance hosting the SOA Suite has been discovered in Enterprise Manager, the cluster in which it is a member has not yet been discovered.

2. From the Add drop-down menu, select Oracle Application Server Cluster and click on Go, as shown in the following screenshot:

![](img/7529bbd615e859608c21b965162348b2_139_1.png)

3. A page appears prompting for information on the existing cluster. Provide a name for the cluster, such as Cluster 10131.

4. To specify an application server instance that is a member of the cluster, click on the flashlight icon.

![](img/7529bbd615e859608c21b965162348b2_140_0.png)

5. On the Search and Select: Targets page, select the name of an application server instance that is a member of the cluster and click on the Select button, as shown in the following screenshot:

![](img/7529bbd615e859608c21b965162348b2_140_1.png)

6. The remaining fields on the Add Oracle Application Server Cluster page are automatically populated based on your selection, as shown in the next screenshot. Click on OK.

### Add Oracle Application Server Cluster

In order to add an Oracle Application Server Cluster to Grid Control for monitoring and central management, you must specify the name of an Oracle Application Server (version 10.1.3 or higher) instance that is a member of the Oracle Application Server Cluster to be added.

Oracle Application Server Cluster Name Cluster 10.1.3.1
Application Server Name soa.gcsoa.us.oracle.com
OPMN Request Port 6003
Application Server Control URL http://gcsoa.us.oracle.com:7777/em

TIP The application server instance name and its OPMN request port are used to contact the application server, and query its Oracle Application Server Cluster membership. Based on the application server instance selected, Enterprise Manager specifies the default value for the OPMN request port. If the port value specified by Enterprise Manager is incorrect, then review the application server's opmn.xml file (i.e. $ORACLE_HOME/opmn/conf/opmn.xml) to obtain the correct value.

7. A confirmation message appears at the top of the screen to confirm that the cluster was successfully added to Enterprise Manager, and Cluster 10.1.3.1 appears as a new target in the table with the soa.gcsoa.us.oracle.com application server shown as a member of that cluster, as shown in the following screenshot:

### Confirmation

The Oracle Application Server Cluster Cluster 10.1.3.1 has been successfully added and is now being monitored by Grid Control.

### Application Servers

| Select Name | Type | Status | Status Details | Alerts | Policy Violations | CPU Usage (%) | Memory Usage (MB) |
|---|---|---|---|---|---|---|---|
| All Application Servers |  |  |  |  |  |  |  |
| Cluster 10.1.3.1 | Oracle Application Server Cluster | n/a | 4 (↑4) | 0 0 | 0 0 0 |  |  |
| soa.gcsoa.us.oracle.com | Oracle Application Server |  | 4 (↑4) | 0 0 | 0 0 0 | 4.15 | 88.39 |
| EnterpriseManager() gcsoa.us.oracle.com | Oracle Application Server |  | 5 (↑5) | 0 0 | 0 0 0 | 3.87 | 964.18 |

TIP For an explanation of the icons and symbols used in this page, see the Icon Key.

### Related Links

Customize Table Columns

> Now that an Oracle Application Server Cluster has been added to Grid Control, you can use the cluster as a reference or source for performing a wide range of operations. For the purpose of this exercise, we will use the discovered cluster to create a "gold image" component in the Grid Control Software Library, and then use the gold image component in the library to create a new Oracle Application Server Cluster target.

### Stopping application server processes

Stopping the application server processes is recommended in order to take a copy of the data and the configuration.

> Prior to creating a gold image component or creating a new cluster via cloning, you should first stop the existing, installed application server processes already running on the machine.

To stop the Oracle Application Server processes, follow these steps:
- 1. Log in as the application server owner on the Linux or Windows machine (for example, oracle).
- 2. Navigate to the bin directory, that is, `cd /u01/app/oracle/product/OracleHomes/soa/opmn/bin`.
- 3. Stop the application server processes by using: `./opmnctl stopall`.
- 4. From the application server home page, you can validate that all components of the instance have been stopped.

> If the application server instance status continues to read Up, click on the Refresh icon on the page.
