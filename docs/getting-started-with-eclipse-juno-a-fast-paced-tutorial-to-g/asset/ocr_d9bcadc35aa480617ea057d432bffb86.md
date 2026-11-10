+   - **File System**: Use this wizard if you already have a project in your workspace and want to add new existing resources in your filesystem. On the next page, select the resources you wish to import by checking them. Click on the **Browse** button to select the project and the folder where the resources will be imported. When you click on the **Finish** button, the resources will be copied to the project's folder inside your workspace.
+   - **Preferences**: You can import Eclipse preferences files to your workspace by selecting this option. Preferences file contains code style and compiler preferences, the list of installed JREs, and the **Problems** view configurations. You can choose which of these preferences you wish to import from the selected configuration file.

# Importing a project from Version Control Servers

Projects that are stored in Version Control Servers can be imported directly into Eclipse. There's a number of version control softwares, each with its pros and cons, and most of them are supported by Eclipse via plugins. GIT is one of the most used softwares for version control; we will discuss it with more details on how to use it efficiently inside Eclipse in *Chapter 4, Managing a Project*. **CVS** is the only version control system supported by default. To import a project managed by it, navigate to **CVS | Projects** from CVS in the Import wizard. Fill in the server information on the following page, and click on **Finish**.

# Introducing Java views

As discussed in *Chapter 1, Introduction*, Eclipse's user interface consists of elements called views. The following sections will introduce the main views related to Java development.

# The Package Explorer view

The **Package Explorer** view is the default view used to display a project's contents. As the name implies, it uses the package hierarchy of the project to display its classes, regardless of the actual file hierarchy. This view also displays the project's build path.