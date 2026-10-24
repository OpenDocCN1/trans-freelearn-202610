![](bbox=[0.113, 0.089, 0.854, 0.464])

Figure 12.18 – Azure Pipelines SonarQube: preparing analysis

Here are the details of the configuration of these tasks:

+   1. The **Prepare analysis on SonarQube** task includes configuring SonarQube with the following:
+   
+      • An endpoint service, which is the connection to SonarQube with its URL and token that we generated previously in the SonarQube configuration
+   
+      • The key and name of the SonarQube project
+   
+      • The version number of the analysis

+   2. Then, we build and execute the unit tests of the application with `npm build` and `npm test`.

+   3. The **Run Code Analysis** task retrieves the test results, analyzes the TypeScript code of our application, and sends the data from the analysis to the SonarQube server.

Then, we save, start executing the CI build, and wait for it to finish.