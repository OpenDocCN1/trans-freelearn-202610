
## Step-by-step exercises

This set of step-by-step exercises will walkthrough managing configurations for the BPEL and application server targets.

### Navigating to BPEL target configuration management

Grid Control has rich configuration management functionality for all its managed targets such as database, application server, BPEL PM, and so on.

- 1. Navigate to the BPEL target home page.
- 2. Click on the Administration tab.
- 3. Note the links related to configuration management for the target.

> Grid Control configuration management framework picks up key configuration parameters for BPEL on a daily basis. These parameters are stored in the Grid Control repository. Key configuration tasks can be accessed from here.

![](img/7529bbd615e859608c21b965162348b2_109_0.png)

### Viewing the last collected configuration

The Grid Control agent collects configuration metrics at a regular interval (default once in 24 hours) and uploads to the configuration management repository. This enables Grid Control to track the configuration history of the managed target. There are several ways to look at the configuration history, one of them being the last collected configuration.

- 1. Click on **Last Collected Configuration** to view configuration parameters.
- 2. View the configuration settings on the **Last Collected Configuration** page.
- 3. The **Process Manager** tab shows generic information about the server including URLs, dehydration store, and cluster parameters.

![](img/7529bbd615e859608c21b965162348b2_110_0.png)

- 4. Click on the **Domains** sub-tab. The **Domains** tab lists the BPEL domains and important configuration parameters for each domain.

![](img/7529bbd615e859608c21b965162348b2_111_0.png)

- 5. Click on **Processes** sub-tab. The **Processes** tab shows a list of deployed processes with version information, underlying files, and last modified time.

![](img/7529bbd615e859608c21b965162348b2_111_1.png)

### BPEL Configuration Management

- 6. Click on **Configuration Files**. Key configuration files from the BPEL server are now stored in the Grid Control repository and displayed on this tab. You can click on the file to view the contents in read-only mode.

![](img/7529bbd615e859608c21b965162348b2_112_0.png)

### Saving a configuration snapshot to the repository

In a large enterprise it is important to standardize configuration across a target type. This helps in ease of maintenance and is also important for compliance reasons. To standardize configurations, Grid Control provides a way to create a "gold" configuration based on an existing target configuration, and then use that to standardize against other targets:

- 1. Click on the **Save** button on the top right.
- 2. Select the option **Save to EM Repository**.

- 3. Enter a description such as GCSOA_<date>_BPEL_Baseline, as shown in the following screenshot:

![](img/7529bbd615e859608c21b965162348b2_113_0.png)

- 4. Click on OK to save the current configuration to the Grid Control repository.

- 5. Note the confirmation message and the saved configuration with timestamp, name, description, saved date, and owner.

### Navigating to Oracle Application Server configuration management

It is useful to look at an Oracle application server configuration as well because BPEL is hosted by the application server:

- 1. From the Grid Control home page, click on the Targets tab.
- 2. Select the Application Servers sub-tab.
- 3. Click on the name of the application server such as soa.gcsoa.us.oracle.com.

- 4. This takes you to the home page for the application server.

![](img/7529bbd615e859608c21b965162348b2_114_0.png)

- 5. Click on the Administration tab.

> Grid Control configuration management framework picks up key configuration parameters for all Oracle Application Server targets and individual components (web server, OC4J, BPEL PM, and many more) on a daily basis. These parameters are stored in the Grid Control repository.

### Viewing last collected configuration

Often, the first thing the administrators want to see is the current configuration of a target. This typically corresponds to the "last collected configuration" in Grid Control, and can be refreshed to initiate an on-demand collection for up-to-date information.

- 1. Select the Last Collected Configuration link, as shown in the following screenshot:

![](img/7529bbd615e859608c21b965162348b2_115_0.png)

- 2. The current configuration of the application server is displayed. The General tab displays key components of the application server and also some key configuration parameters, as shown in the following screenshot:

![](img/7529bbd615e859608c21b965162348b2_115_1.png)

- 3. Click on the J2EE Applications tab to view all the J2EE applications deployed on this application server.

![](img/7529bbd615e859608c21b965162348b2_116_0.png)

- 4. Click on the Configuration Files tab to view the key configuration files. You can click on the files to view a read-only version of the files.

![](img/7529bbd615e859608c21b965162348b2_116_1.png)

### Saving a configuration snapshot to the repository

Similar to the BPEL configuration snapshot, the application server configuration can also be saved to the Grid Control repository in order to standardize across the enterprise:

- 1. Click on the Save button to save the current configuration.
- 2. Select the option Save to the EM Repository.
- 3. Provide a description such as GCSOA_<date>_AS_Baseline.
- 4. Select the checkbox for saving all the configuration of the application server components, as shown in the following screenshot:

![](img/7529bbd615e859608c21b965162348b2_117_0.png)

- 5. Click on OK to save the current configuration to the Grid Control repository.

- 6. Note the confirmation message, and the saved configuration with target name, type, and timestamp.

![](img/7529bbd615e859608c21b965162348b2_118_0.png)

### Making changes to the BPEL environment

The Grid Control console is the management interface for one or more BPEL PMs, as well as other target types. For administrative functions such as start/ stop, changing configuration properties, and so on, the administrator needs to utilize the product console, in this case BPEL console.

- 1. Navigate to the BPEL target home page.
- 2. Under **Related Links**, click on **Administer (BPEL Console)**.
- 3. Log in using the **Username** as **oc4jadmin** and **Password** as *welcome1*.
- 4. Click on the **Manage BPEL Domain** link.
- 5. This takes you to the admin page for BPEL where you can set domain-level parameters. Select the **Configuration** tab.

![](img/7529bbd615e859608c21b965162348b2_119_0.png)

- 6. Change the following properties:
  - AuditDetailThreshold to 60k
  - dspMaxThreads to 6
  - dspMinThreads to 2
  - LargeDocumentThreshold to 60k

- 7. Click on the Apply button to save the changes.

- 8. Select the **Logging** tab and select the dropdown to change the logging level for all to **Warn**. The debug level of warn indicates warning messages that are logged to some log, but the application is able to carry on without a problem.

![](img/7529bbd615e859608c21b965162348b2_120_0.png)

- 9. Click on **Apply** to save the changes.

- 10. Select **Logout** to exit the BPEL Control.

### Making changes to the Oracle Application Server environment

Similar to the BPEL Console, the application server also comes with its own administrative console to make configuration changes.

- 1. From the Grid Control home page, click on the **Targets** tab.
- 2. Select the **Application Servers** sub-tab.

- 3. Click on the name of the application server—soademo.gcsoa.us.oracle.com.
- 4. This takes you to the home page for the application server.
- 5. Click on the Administer link at the bottom, under the Related Links section.
- 6. Log in to the AS Control console by using the username as oc4jadmin and password as welcome1.
- 7. Click on the oc4j_soa link to go to the home page for this oc4j instance, as shown in the following screenshot:

![](img/7529bbd615e859608c21b965162348b2_121_0.png)
