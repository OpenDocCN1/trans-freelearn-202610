Azure Databricks includes many popular Python libraries already installed. In order to check whether the library we want to install is already available, we can see which libraries are included in Databricks Runtime by looking at the System Environment subsection of the Databricks Runtime release notes for your Databricks Runtime version.

# Workspace libraries

Workspace libraries act as a local repository from where cluster-installed libraries can be created. These libraries can be custom code created by you or your company or organization; it can be a particular version of an open-source library that has been standardized or simply might hold features that are necessary for the job you are doing.

We must first install the workspace library on a cluster before it can be made available to notebooks or jobs running on that cluster. We can create a workspace library in the Shared folder, which will be made available to all users in that workspace, or we can create it in a user folder, which will make the library available only to that user.

To install a library, right-click on the workspace folder where you want to store the library and select the Create | Library option:

![](bbox=[0.25, 0.47, 0.72, 0.7])

Figure 7.1 – Creating a library in a directory