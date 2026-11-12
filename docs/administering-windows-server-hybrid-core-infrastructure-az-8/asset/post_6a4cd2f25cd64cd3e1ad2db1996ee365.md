
## JEA

JEA provides delegated administration through **role-based access control (RBAC)** and is built on the foundation of PowerShell remoting.

JEA limits the amount of access and privileges in terms of what users can do and which cmdlets they can run and provides auditing to understand better who did what during a session. It's a key defense against lateral movement attacks through _compromised privileged accounts_ and works based on _least privilege_ access and rights.

JEA is included in **PowerShell 5.0** and later. To check which version of PowerShell is installed, you can run the following PowerShell cmdlet:

```
$PSVersionTable.PSVersion
```

To start using JEA, you would want to see a result that shows as follows, with the **Major release** being 5 or higher:

| Major | Minor | Build | Revision |
|-------|-------|-------|----------|
| 5 | 1 | 14393 | 1000 |

Having the latest version of PowerShell available for your machine is always recommended.

The following list details the JEA availability via OS type:

- Pre-installed: Windows Server 2016+, Windows 10 1607+
- Pre-installed with reduced functionality: Windows 10 1603, 1511
- Full functionality with **Windows Management Framework (WMF)** 5.1: Windows Server 2012/R2, Windows 8, 8.1
- Reduced functionality with WMF 5.1: Windows Server 2008/R2, Windows 7
- Not available: Windows 10 1507

PowerShell remoting is a required foundation for JEA and must be enabled before JEA can be used. PowerShell remoting is enabled by default in Windows Server 2012/R2, 2016; the `Enable-PSRemoting` command is used to enable it.

JEA creates configured endpoints that users connect to; there are files associated with a configured endpoint, and these are as follows:

- Role capabilities file
- Session configuration file

The role capabilities file can be identified with a .prsc file extension. The session configuration file can be identified with a .pssc file extension.

As its name suggests, the role capabilities file describes what can be done in a session using the basis of RBAC; the cmdlets, providers, functions, and external programs are listed.

Who can use the endpoints and which role capabilities they have access to are defined in the session configuration file.

A template for a blank configuration file can be created using the following command:

```
New-PSSessionConfigurationFile -SessionType
RestrictedRemoteServer -Path .\MyJEAEndpoint.pssc
```

The final piece after the role capability file and session configuration file have been created is the registration of the endpoint; the following PowerShell command is used for this:

```
Register-PSSessionConfiguration -Path .\AZ800JEAConfig.pssc
-Name 'JEAServeDEsk' -Force
```

Further information on configurations can be found in the following Microsoft documentation article: https://docs.microsoft.com/en-gb/powershell/scripting/learn/remoting/jea/session-configurations.

This section looked at configuring PowerShell remoting and some challenges and risks. In the next section, we will complete a hands-on exercise section to re-enforce some of the concepts covered in this chapter.

## Hands-on exercise

To support your learning with some practical skills, we will utilize the concepts and understanding gained from the earlier sections of this chapter and put them into practical application.

We will look at the following exercise:

- Exercise – implementing WAC within the Azure portal

## Getting started

To get started with this section, if you do not already have an Azure subscription, you can create a free Azure account at https://Azure.microsoft.com/free. This free Azure account provides the following:

- 12 months of free services
- USD $200 credit to explore Azure for 30 days
- 25+ services that are always free

Let's move on to the exercise for this chapter.

## Exercise – implementing WAC within the Azure portal

This exercise will look at implementing WAC within the Azure portal to manage an Azure VM.

You will need the following in place for this exercise:

- An Azure VM to act as the management target.
- Access to the VM with a local admin account.
- Access to the same VNet as the VM from a management machine used to connect to the Azure portal; this could be via a VPN, public IP address, or another Azure VM on the same VNet used as a jump host. If connecting via a public IP address, you should create an inbound port rule for HTTP 443 and TCP 6516.

Follow these steps to get started:

1. You should click on **Windows Admin Center (preview)** from the VM to be managed under the **Settings** section:

![](img/cf35f46a8dd5f11e58349c1d81611b75_410_0.png)

Figure 12.3 – Azure VM to be managed

2. From the Windows Admin Center (preview) blade, click Install:

## Windows Admin Center

Windows Admin Center provides a free set of best-in-class tools to manage Windows and Windows Server on this virtual machine directly from within Azure.
Get an overview of Windows Admin Center
Privacy terms for Windows Admin Center extension

Seamless server management
Seamlessly manage Windows Server within Azure.

Modern server tools
Access state-of-the-art Windows Server management tools.

Increased control
Gain full control over all aspects of your servers hosted in Azure.

Inbound Port *
6516
Open this port for me (Recommended for testing only)
Open an outbound port for Windows Admin Center to install
Install
License terms for Windows Admin Center extension

Figure 12.4 – Installing WAC extension to VM

3. You will receive a notification in the portal that the WAC extension was successfully installed on the VM:

Successfully installed Windows Admin Center extension
Successfully installed Windows Admin Center extension for sm-49-az800-wac-azportal-vm.

Figure 12.5 – WAC extension successfully installed on VM

4. Now that the WAC extension has been installed on the VM, we can use WAC within the Azure portal to manage the VM without the need for RDP or PowerShell.

5. To connect to the VM, navigate to the VM in the Azure portal, select an IP address to connect to the VM (public or private), and select Connect:

![](img/cf35f46a8dd5f11e58349c1d81611b75_412_0.png)

**Windows Admin Center**
Windows Admin Center provides a free set of best-in-class tools to manage Windows and Windows Server on this virtual machine directly from within Azure.

![](img/cf35f46a8dd5f11e58349c1d81611b75_412_1.png)

Figure 12.6 – Connecting WAC to VM

6. You will be prompted to sign in; you should use a local admin account for the VM for this exercise (the admin account set when creating the VM was used in this lab example).

7. You will now have access to WAC running integrated within the Azure portal:

![](img/cf35f46a8dd5f11e58349c1d81611b75_413_0.png)

Figure 12.7 – WAC within Azure portal

With this, we have completed this exercise. This exercise taught us the skills needed to install WAC on an Azure VM and connect to it using WAC within the Azure portal. Now, let's summarize this chapter.

## Summary

This chapter provided coverage for the AZ-800 Administering Windows Server Hybrid Core Infrastructure: Manage Windows Servers and workloads in a hybrid environment exam.

This chapter's content further developed your knowledge and skills for hybrid management services.

You learned about deploying a WAC gateway for the management of multiple VMs and how to install WAC so that a VM can be managed using WAC from within the Azure portal; we then concluded by looking at PowerShell remoting. We then finished the chapter with a hands-on exercise to develop your skills further.

You added new skills through the information provided, with the chapter's goal to take your knowledge beyond the exam objectives so that you are prepared for a real-world, day-to-day, hybrid environment-focused role.

The next chapter will teach you about implementing and managing Azure network infrastructure.
