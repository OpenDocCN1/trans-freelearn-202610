
With the advent of agile methods and a **development-operations (DevOps)** culture, the use of a VCS in processes has become mandatory. Indeed, as mentioned in _Chapter 1, The DevOps Culture and Infrastructure as Code Practices_, the implementation of a **continuous integration/continuous deployment (CI/CD)** process can only be done with a VCS as a prerequisite.

In this chapter, we will see how to use one of the best-known VCSs, which is Git. We will start with an overview of Git and see how to install it. Then, we will see its main command lines to familiarize any developer with their uses. Finally, we will see the current process of using Git workflows and usage of Gitflow. The purpose of this chapter is to show the usage of Git daily with simple processes.

This chapter covers the following topics:

- Overviewing Git and its principal command lines
- Understanding the Git process and Gitflow pattern

This chapter does not cover the installation of a Git server, so if you want to know more about that, you can refer to the documentation at the following link: `https://git-scm.com/book/en/v2/Git-on-the-Server-The-Protocols.`

## Technical requirements

The use of Git does not have any technical prerequisites; we just need a command-line Terminal.

To illustrate its usage in this chapter, we will use Azure Repos from the Azure DevOps platform (formerly **Visual Studio Team System (VSTS)**), which is a cloud platform that has a Git repository manager. You can register here for free: `https://visualstudio.microsoft.com/team-services/`.

Check out the following video to see the code in action: `https://bit.ly/3LTcoOi`.

## Overviewing Git and its principal command lines

To understand the origin of Git, it is necessary to know that there are two types of VCSs: centralized and distributed systems.

The first type to emerge were **centralized systems**, such as **Subversion (SVN)**, **Concurrent Version System (CVS)**, and **Team Foundation Version Control (TFVC)** and Microsoft **Visual SourceSafe (VSS)**. These systems consist of a remote server that centralizes the code of all developers.

We can represent a centralized source control system like this:

![](img/d3344b39c888aab0b832625465f837c9_179_0.png)

Figure 6.1 – Centralized source control

All developers can archive and retrieve their code on the remote server. The system allows better collaboration between teams and a guarantee of code backup. However, it has its drawbacks, such as the following:

- In case of no connection (for a network problem or internet disconnection) between the developers and the remote server, no more archiving or code recovery actions can be performed.
- If the remote server no longer works, the code, as well as the history, will be lost.

The second type of VCS, which appeared later, is a **distributed system**, such as Mercurial or Git. These systems consist of a remote repository and a local copy of this repository on each developer’s local machine, as shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_179_1.png)

Figure 6.2 – Distributed source control

So, with this distributed system, even in the event of disconnection from the remote repository, developers can continue to work with the local repository, and synchronization will be done when the remote repository is accessible again. A copy of the code and its history is also present in the local repository.

Git is, therefore, a distributed VCS that was created in 2005 by Linus Torvalds and the Linux development community.

> Note
> To learn a little more about Git's history, read this page: `https://git-scm.com/book/en/v2/Getting-Started-A-Short-History-of-Git`.

Since its creation, Git has become a very powerful and mature tool that can be used by anyone for coding.

Git is a free, cross-platform tool, and it can be installed on a local machine for people who manipulate code—that is, in client mode—but can also be installed on servers to host and manage remote repositories.

Git is a command-line tool with a multitude of options. Nevertheless, there are many graphical tools today—such as Git GUI, GitKraken, GitHub Desktop, or Sourcetree—that allow you to interact with Git operations more easily and graphically without having to use command lines yourself. However, these graphical tools do not contain all of the operations and options available on the command line. Fortunately, many code editors such as **Visual Studio Code (VS Code)**, Visual Studio, JetBrains, and Sublime Text allow direct code integration with Git and remote repositories.

For remote repositories, there are several cloud and free solutions such as GitHub, GitLab, Azure DevOps, or Bitbucket Cloud. Also, there are other solutions called on-premises solutions that can be installed in an enterprise, such as Azure DevOps Server, Bitbucket, or GitHub Enterprise.

In this chapter, we will see the usage of Git—for example, with Azure DevOps as a remote repository—and we will see the usage of GitLab and GitHub in future chapters.

In this section, we have introduced Git, and we will now see how to install it on a local machine to develop and version our sources.

## Git installation

We will now detail the steps of installing and configuring Git on Windows, Linux, and macOS systems.

To install on a Windows machine manually, we must download the **Git for Windows tool** executable from `https://gitforwindows.org/`, and, once downloaded, we click on the executable file and follow the next different configuration steps during the installation:

1. Select an installation path for the Git binaries. We keep the default path, as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_181_0.png)

Figure 6.3 – Git installation path

2. Choose Git integration components in Windows Explorer by marking the **Windows Explorer integration** checkbox, as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_182_0.png)

Figure 6.4 – Choosing Git installation components

3. Choose your code editor **integrated development environment (IDE)**; in our case, we use VS Code by selecting the **Use Visual Studio Code as Git’s default editor** option, as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_182_1.png)

Figure 6.5 – Choosing Git installation editor

4. Configure the name of the default branch. We can keep the default option to use `master` as the default branch name, as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_183_0.png)

Figure 6.6 – Git configuration default branch name

5. When choosing an **Adjusting your PATH environment** option, we can leave the default choice proposed by the installer, as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_183_1.png)

Figure 6.7 – Git installation PATH configuration

6. Choose a Secure Shell (SSH) client to use by keeping the default option to use the integrated `ssh.exe` client, as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_184_0.png)

Figure 6.8 – Git installation SSH tool

7. Choose a type of HyperText Transfer Protocol Secure (HTTPS) transport, which we will also leave by default at the Use the OpenSSL library option, as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_184_1.png)

Figure 6.9 – Git installation using OpenSSL

8. Choose an option for the encoding of the end files. We will also select the default option, which archives in Unix format, as illustrated in the following screenshot:

Figure 6.10 – Choosing encoding of Git installation files

9. Select a default terminal emulator for Git Bash. We choose MinTTy, as illustrated in the following screenshot:

Figure 6.11 – Git installation terminal emulator

10. Choose the default behavior of the `git pull` command. We keep the default option, as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_186_0.png)

Figure 6.12 – Git installation default git pull command

11. Choose a credential manager helper by keeping the default option, as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_186_1.png)

Figure 6.13 – Git installation credential manager
