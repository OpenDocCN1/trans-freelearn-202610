Further information and syntax about dcdiag can be found at https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731968(v=ws.11).

# netdom

This tool allows you to manage AD DS trusts; it can also join a computer to a domain, manage computer accounts, query for domain information such as which DCs hold the FSMO roles, and more. It is available when the AD DS role is installed on a server and is also included as part of the AD DS tools in the RSAT tools.

Further information and syntax about netdom can be found at https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc772217(v=ws.11).

In this section, we looked at some of AD DS's monitoring and troubleshooting tools. In the next section, we will complete a hands-on exercise to reinforce some of the concepts covered in this chapter.

# Hands-on exercise

To support your learning with practical skills, let's learn how to create some of the services we looked at in this chapter. You will learn how to install AD DS on Windows Server.

# Getting started

To start this hands-on exercise, you will need access to a physical or virtual machine running Windows Server 2012 Standard/Datacenter or later.

For this exercise, we could use a nested virtualization environment in Azure, meaning no on-premises hardware is required. However, to help you with your learning and demo purposes, we will complete this exercise using IaaS VMs in an Azure environment where we have the correct level of access to create the required resources.

You can create a free Azure account at https://azure.microsoft.com/free. This free Azure account provides the following:

+   - 12 months of free services
+   - $200 credit to explore Azure for 30 days
+   - 25+ services that are always free

If you will be using Azure IaaS VMs for DCs, then recommended practice is that each VM should have a data disk attached to store the AD DS database, log files, and SYSVOL. Alternatively, you could install them on the default paths provided for learning purposes. However, this should not be done in a production scenario.