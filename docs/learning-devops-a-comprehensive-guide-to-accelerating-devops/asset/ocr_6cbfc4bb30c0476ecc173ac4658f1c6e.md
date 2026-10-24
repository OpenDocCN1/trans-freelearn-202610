# # The following screenshot shows the remote GitLab repository:

![](bbox=[0.133, 0.127, 0.867, 0.504])

Figure 7.63 – GitLab repository

The code of our application has been deposited in GitLab, and we can now create our CI process with GitLab.

# # Creating a CI pipeline

In GitLab CI, the creation of a CI pipeline (and CD) is not done via a **graphical UI (GUI)**, but with a YAML file at the root of the project.

This method, which consists of describing the process of a pipeline in a file that is located with the code, can be called **Pipeline as Code (PaC)**, in the same way as **Infrastructure as Code (IaC)**. We'll proceed as follows:

+   1. To create this pipeline, we will create, at the root of the application code, a `.gitlab-ci.yml` file with the following content:
+   
+   ```
+   image: microsoft/dotnet:latest
+   stages:
+     - build
+     - test
+   ```