# Navigating to application servers

As the SOA Suite and BPEL Process Manager server is hosted on an application server (for example, Oracle Application Server, Oracle WebLogic Server), the first task is to visit the application server home page:

+   1. Navigate to http://<Grid_Control_server_name>:<server_port>/em.
+   2. Log in as sysman. Use the password you set during the installation.
+   3. Click on the Targets tab.
+   4. Click on the Middleware sub-tab.

![](bbox=[0.13, 0.32, 0.2, 0.36])

> This is a view showing all of the monitored application servers (Oracle AS, BEA WebLogic, and so on) within the enterprise. For each target, a summary of status with details, alerts, policy violations, CPU usage, and memory usage on the host are tabulated.

# Discovering a BPEL PM target

The administrator needs to inform Grid Control to explicitly discover the application server and the associated targets such as BPEL PM. Before attempting discovery, the Grid Control Agent (10.2.0.3 or higher) needs to be installed on the host with the application server software. Note that a remote agent can also be used to discover an application server. The only drawback with a remote agent is the host and operating system metrics for the application server machine will not be picked up.