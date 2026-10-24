SonarQube is designed for developer teams and provides them with a dashboard and reports that are customizable so that they can present the quality of the code in their applications.

It allows for the analysis of static code in a multitude of languages (over 25), such as **PHP: Hypertext Preprocessor (PHP)**, Java, .NET, JavaScript, Python, and so on. A complete list can be found at `https://www.sonarqube.org/features/multi-languages/`.

In addition, apart from code analysis with security issues, code smell, and code duplication, SonarQube also provides code coverage for unit tests. For more details about these issue concepts, read the documentation here: `https://docs.sonarqube.org/latest/user-guide/concepts/`.

Finally, SonarQube integrates very well into CI/CD pipelines so that it can automate the code analysis during developer code commits. This reduces the risk of deploying an application that has security vulnerabilities or code complexity that is too high.

On the other hand, it is important to note that they have a multitude of plugins that can be paid for. A list of plugins is available here: `https://www.sonarplugins.com/`.

Now that we've provided an overview of SonarQube, we will look at its architecture and components. Finally, we will look at the different ways of installing it.

# # Installing SonarQube

SonarQube is an on-premises solution—in other words, it must be installed on servers or **virtual machines (VMs)**. In addition, SonarQube consists of several components that will analyze the source code of applications, retrieve and store the data from this analysis, and provide reports on the quality and security of the code.

Before we install SonarQube, it is essential that we look at its architecture and components.

# # Overview of the SonarQube architecture

SonarQube is a client/server tool, which means that its architecture is composed of artifacts on the server side and also on the client side.