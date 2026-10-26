![](bbox=[0.26, 0.07, 0.75, 0.37])

Figure 12.48: Warehouses in the Dataverse environment

Figure 12.48 shows the location of warehouses from Dynamics 365 Finance and Operations loaded into Dataverse.

# How it works

While provisioning the environment, you are given the option to either create a database or not. You might choose to not create a database if the environment is to be used solely for Power Apps and Power Automate development, with source data coming from somewhere other than the Dataverse environment. By creating the Dataverse environment with a database and checking the box to Deploy Dynamics 365 apps to Yes, additional tables are added to the environment to support the chosen applications.

It is important that you set the role for the default team to System Administrator or System Customizer. By default, the team created as part of the provisioning process has no rights within the system, while the user who created the environment is given the System Administrator role, among others. However, the synchronization performed by the connection sets, projects, and tasks currently uses the team permissions, not the user permissions, when writing to the Dataverse environment.

Creating a data integration project consists of three tasks, creating connections (steps 1 to 5), creating a connection set (steps 11 to 18), and, finally, creating a project that contains the tasks to perform (steps 19 to 30). A connection simply holds the authentication credentials for a data source, similar to defining a data source connection in Power BI (data source settings). A connection set simply defines a source and destination system. Unlike Power BI, where the destination is always the Power BI semantic model, data integration projects can synchronize data between any two source systems. Finally, a project defines the tasks to be performed during data integration. The tasks hold the mappings between columns in the source and destination systems. Projects can be scheduled to run at regular intervals in order to ensure data remains current.