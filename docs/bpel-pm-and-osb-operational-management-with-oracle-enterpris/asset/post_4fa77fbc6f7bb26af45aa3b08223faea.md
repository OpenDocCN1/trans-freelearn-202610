
## 2
## Discovering BPEL PM

A management solution needs to locate and identify whatever it needs to manage. This process is called discovery, where the management solution locates and identifies all the targets. This is typically done using management agents that are deployed on servers that contain the targets that need to be managed. Discovery of targets enables administrators to have a picture of the environment along with the various components that need to be managed.

For any small, medium, or large enterprise, the challenge is to manage several disparate targets from a single console. The traditional hardware and software components such as servers, storage devices, operating systems, databases, middleware, and applications have been managed by system management solutions. However, a new breed of BPEL-based applications and infrastructure poses new problems. First, they need to be managed along with existing hardware and software. Second, as the BPEL projects proliferate across the enterprise, the infrastructure team is faced with increasing problems to manage these projects. Third, for the administrators who are new to BPEL, supporting these environments in production poses unknown challenges. Finally, the IT managers want to avoid hiring new administrators with increasing BPEL projects.

Grid Control provides a way to discover the BPEL components, whether it's on production or otherwise. For the infrastructure team, this is a convenient way to manage several installations from one console. Once the agent is installed on each of the servers hosting BPEL PM, the discovery process can be initiated from the console.

In this chapter, we will discuss how to discover and configure a BPEL Process Manager target. Discovery of BPEL PM targets is the first step before performing other management tasks. Further, creating the infrastructure service is mandatory to manage BPEL processes and related infrastructure. The following areas are covered in this chapter:

- Support for managing BPEL PM
- BPEL PM target discovery
  - Navigating to application servers
  - Discovering a BPEL PM target
  - Navigating to the BPEL PM target home page
  - Configuring a BPEL PM target

### Support for managing BPEL PM

The BPEL PM product ships with a console that is useful for administration activities such as turning a process on or off, deleting instances, checking the flow of an instance, and so on. However, it cannot provide management capabilities such as monitoring the availability and health of the servers, processes, configuration management, and bulk deployment.

Grid Control introduced BPEL management functionality with the 10.2.0.3 (or 10gR3) version, released in January 2007. The first release provided most of the functionality for monitoring the BPEL infrastructure including the BPEL PM server. Additionally, it had process-level monitoring, error-instance management, and service-level management.

The 10.2.0.4 (10gR4) version, released on Linux platforms in November 2007, brought additional new features for managing BPEL PM. Configuration management, deployment automation, and adapter monitoring were the main features of the release. The 10.2.0.5 (10gR5) version, released on Linux platforms in February 2009, added reporting capabilities for tracking troublesome BPEL instances.

### BPEL PM target discovery

This set of step-by-step exercises will walkthrough the discovery of a BPEL PM target and setting up monitoring configuration for the target.

### Navigating to application servers

As the SOA Suite and BPEL Process Manager server is hosted on an application server (for example, Oracle Application Server, Oracle WebLogic Server), the first task is to visit the application server home page:

- 1. Navigate to http://<Grid_Control_server_name>:<server_port>/em.
- 2. Log in as sysman. Use the password you set during the installation.
- 3. Click on the Targets tab.
- 4. Click on the Middleware sub-tab.

![](img/7529bbd615e859608c21b965162348b2_31_0.png)

> This is a view showing all of the monitored application servers (Oracle AS, BEA WebLogic, and so on) within the enterprise. For each target, a summary of status with details, alerts, policy violations, CPU usage, and memory usage on the host are tabulated.

### Discovering a BPEL PM target

The administrator needs to inform Grid Control to explicitly discover the application server and the associated targets such as BPEL PM. Before attempting discovery, the Grid Control Agent (10.2.0.3 or higher) needs to be installed on the host with the application server software. Note that a remote agent can also be used to discover an application server. The only drawback with a remote agent is the host and operating system metrics for the application server machine will not be picked up.

- 1. As shown in the following screenshot, from the drop-down list on the right, add an Oracle Application Server target. Click on Go.

![](img/7529bbd615e859608c21b965162348b2_32_0.png)

- 2. On the Add Application Server page:
  - Click on the flashlight icon to pick a host.
  - In the pop-up window, select the radio button for the host and click on Select, as shown in the following screenshot:

![](img/7529bbd615e859608c21b965162348b2_33_0.png)

- 3. On the **Add Application Server** page, click on **Continue** on the right.
- 4. On the **Discover Application Server: Results** page, click on **OK**.
- 5. In a very short time, you should get a confirmation page. Click on **OK**.
- 6. On the **Application Servers** page, click on **Expand All** and view your application server.

> Notice the components of this application server. Notice the two OC4J targets, as well as the Oracle BPEL Process Manager target.

![](img/7529bbd615e859608c21b965162348b2_34_0.png)

### Navigating to the BPEL PM target home page

The BPEL PM target is a child target of the application server target. There are multiple ways to navigate to the home page of the BPEL PM target; the most common path is to click on the Oracle BPEL Process Manager target name hyperlink.

> On the Oracle BPEL Process Manager home pages, you will see the Status label, which shows the availability status of the BPEL PM server, Availability (%) (last 24 hours), Oracle Home, and Host.

![](img/7529bbd615e859608c21b965162348b2_35_0.png)

### Configuring a BPEL PM target

Once the BPEL PM target is discovered, the administrator needs to provide the target with basic configuration information. This enables the BPEL target in Grid Control to talk to the BPEL PM server. This communication is needed because the BPEL target collects out-of-box monitoring metric data by polling the BPEL PM server through APIs.

- 1. Under the **Related Links** section at the bottom, click on **Monitoring Configuration**.
- 2. Enter the administrator username for BPEL Process Manager as **oc4jadmin**.
- 3. Enter the password for the administrator as *welcome1*.
- 4. Enter the **Initial Context Factory** as **com.evermind.server.rmi.RMIInitialContextFactory**.
- 5. Enter the **Context Provider URL** as **opmn:ormi://<Application_Server_Host_Name>:6003:oc4j_soa/orabpel**.
- 6. Leave the other fields blank and click on OK, as shown in the following screenshot:

![](img/7529bbd615e859608c21b965162348b2_36_0.png)

This concludes the discovery and configuration of the BPEL PM target deployed on Oracle Application Server.

### Summary

Discovery is the first step to manage a target in the enterprise. Discovering and managing multiple targets from a single console is a powerful way to ramp up on new projects while maintaining a standardized management platform. This avoids hiring new administrators for new projects that increase the number of managed targets.

In this chapter, we looked at how to discover and configure the BPEL PM target.

These are mandatory steps to enable subsequent management tasks. We discovered the BPEL PM target indirectly by discovering the application server (in this case Oracle Application Server). Note that the discovery process is similar for discovering BPEL PM targets installed on WebLogic and WebSphere application servers. Detailed discovery information (including 10.2.0.4 patches) for WebLogic and WebSphere application server-based BPEL PM is listed in the following document on Oracle Technology Network:

```
http://download.oracle.com/docs/cd/B16240_01/doc/em.102/e12650/toc.htm.
```

The next few chapters will walkthrough hands-on exercises to set up and perform additional management tasks for the discovered BPEL PM target.
