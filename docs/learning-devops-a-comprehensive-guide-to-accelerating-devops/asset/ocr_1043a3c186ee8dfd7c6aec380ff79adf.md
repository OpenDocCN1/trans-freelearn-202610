With the advent of agile methods and a **development-operations (DevOps)** culture, the use of a VCS in processes has become mandatory. Indeed, as mentioned in _Chapter 1, The DevOps Culture and Infrastructure as Code Practices_, the implementation of a **continuous integration/continuous deployment (CI/CD)** process can only be done with a VCS as a prerequisite.

In this chapter, we will see how to use one of the best-known VCSs, which is Git. We will start with an overview of Git and see how to install it. Then, we will see its main command lines to familiarize any developer with their uses. Finally, we will see the current process of using Git workflows and usage of Gitflow. The purpose of this chapter is to show the usage of Git daily with simple processes.

This chapter covers the following topics:

+   - Overviewing Git and its principal command lines
+   - Understanding the Git process and Gitflow pattern

This chapter does not cover the installation of a Git server, so if you want to know more about that, you can refer to the documentation at the following link: `https://git-scm.com/book/en/v2/Git-on-the-Server-The-Protocols.`

# ## Technical requirements

The use of Git does not have any technical prerequisites; we just need a command-line Terminal.

To illustrate its usage in this chapter, we will use Azure Repos from the Azure DevOps platform (formerly **Visual Studio Team System (VSTS)**), which is a cloud platform that has a Git repository manager. You can register here for free: `https://visualstudio.microsoft.com/team-services/`.

Check out the following video to see the code in action: `https://bit.ly/3LTcoOi`.

# # Overviewing Git and its principal command lines

To understand the origin of Git, it is necessary to know that there are two types of VCSs: centralized and distributed systems.