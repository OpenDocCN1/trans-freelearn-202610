# 14

# WebLogic and OSB
Configuration Management

Managing the configurations of middleware environments is a difficult proposition for medium and large enterprises. "Knowing what you have" enables IT managers to make decisions about future investments and retiring old assets, standardize existing assets, manage change across the enterprise, and troubleshoot performance problems related to configurations.

Most configuration management solutions today are compliant with the best practices laid out by the **IT Information Library (ITIL)** framework. All the assets are stored in a central repository called the **Configuration Management Database (CMDB)**. Configuration parameters such as instance parameters for a database, or start up parameters for a WebLogic application server, or port numbers for an HTTP server are stored in the CMDB. Any changes to these parameters are also tracked. It then becomes easy to audit the changes for a specific asset, compare assets to each other (for example, Managed Server 1 and Managed Server 2), compare different versions of the same asset, save a reference configuration to the CMDB, and use that to standardize across the enterprise. This chapter talks about managing configurations for a WebLogic and OSB environment. Specifically, this chapter talks about:

+   - Challenges
+   - Solution
+   - Step-by-step exercises:
+     - Viewing a WebLogic managed server configuration
+     - Saving a managed server configuration snapshot
+     - Viewing and saving an OSB configuration
+     - Comparing the current OSB configuration with a saved baseline
+     - Comparing the current managed server configuration with a saved baseline