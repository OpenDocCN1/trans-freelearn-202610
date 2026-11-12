For this exercise, you could use the AD instance we created in the *Install AD DS on Windows server* exercise in Chapter 1. You should adjust the steps according to the environment you will use.

You will need the following accounts and permissions to complete the exercise:

+   - An AD account with *Enterprise Administrator* permissions and *Local Administrator* rights to the server where the Azure AD Connect tool is installed
+   - An Azure AD account with *Global Administrator* permissions
+   - A public domain that you own that has access to DNS records you can manage

We have broken down the exercise into individual tasks for better understanding:

# Task 1 – Preparing a non-routable domain for AD Connect directory synchronization

> **Note**
> This task is only applicable to readers who have a non-routable domain. You may skip this task if this is not relevant to your scenario.

In this task, we prepare a non-routable AD DS domain (such as a *.local* namespace) for directory synchronization by adding a UPN suffix that can be added to Azure AD and verified for use with Azure AD Connect. We then change the user's UPN suffix to this domain:

+   1. Log in to an *AD DS DC* and select **Tools | Active Directory Domains and Trusts** from **Server Manager**:

![](bbox=[0.135, 0.6, 0.865, 0.79])

Figure 4.10 – Server Manager tools