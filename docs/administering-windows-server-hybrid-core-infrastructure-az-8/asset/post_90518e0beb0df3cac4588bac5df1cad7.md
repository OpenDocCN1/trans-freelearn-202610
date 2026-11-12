

![](img/cf35f46a8dd5f11e58349c1d81611b75_384_0.png)

### Figure 11.5 – Azure core quota limit at 100%

The following shows the message that is displayed when the quota is exceeded for a new VM:

![](img/cf35f46a8dd5f11e58349c1d81611b75_384_1.png)

### Figure 11.6 – Azure core quota exceeded message

However, this can be resolved with a service request to Microsoft, who can raise the quota, with the caveat that your purchase plan type must support an increase in quota; sandbox environments don't allow a core quota increase. The request quota increase is represented in the following figure:

![](img/cf35f46a8dd5f11e58349c1d81611b75_384_2.png)

### Figure 11.7 – Quota limit increase request

You must also ensure you resize to the same VM type; for example, if you have a VM size of Dds4, this means you have a D series with four vCPUs that provide a local temp disk and support SSD disks. You want to ensure you resize to keep those VM characteristics. That is, resize from Dds4 to Dds8, not D8; D8 would not support your SSD disks or provide the local temp disk.

Changing the VM size will cause it to be restarted (at the time of writing). This should be considered if you are using dynamic IP address allocation, as this will be lost when the VM is restarted; you should configure static IP allocation to resolve this.

This chapter will look at changing the VM size in the *Hands-on exercise* section.

In this section, we looked at the operations of resizing VMs. In the next section, we will look at configuring the continuous delivery of an Azure IaaS VM.

## Configuring continuous delivery for an Azure VM

**Continuous delivery** in the context of Azure first-party solutions for Azure IaaS VMs relates to Azure Pipelines.

**Azure Pipelines** is better known as a software development tool but can be used to implement an **Infrastructure-as-Code (IaC)** approach to automate the deployment of *artifacts* to IaaS VMs in Azure as the compute target. The following figure visualizes where continuous delivery fits into the larger DevOps methodology.

![](img/cf35f46a8dd5f11e58349c1d81611b75_385_0.png)

### Figure 11.8 – CD within DevOps methodology

How this continuous delivery approach relates specifically to the IaC approach for deployment is illustrated in the following figure, which outlines a simplified view of the flow and components; for clarity, not all flow components are shown:

![](img/cf35f46a8dd5f11e58349c1d81611b75_386_0.png)

### Figure 11.9 – Continuous delivery using Azure DevOps

The Azure portal continuous delivery solution for VMs is based on Azure Pipelines; this is shown in the following screenshot:

![](img/cf35f46a8dd5f11e58349c1d81611b75_386_1.png)

### Figure 11.10 – Azure VM continuous delivery

To configure continuous delivery with a deployment pipeline, you will need to have an **Azure DevOps organization** that you can select; if you do not have an Azure DevOps organization to select, you will be prompted to create one. This is shown in the following figure:

![](img/cf35f46a8dd5f11e58349c1d81611b75_387_0.png)

### Figure 11.11 – Configuring a deployment pipeline

Full details on how to configure a deployment pipeline for an Azure VM using Azure DevOps can be found in the following Microsoft documentation: https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-devops-azure-pipelines-classic.

This section looked at configuring continuous delivery for an Azure VM. In the next section, we will look at configuring connections to VMs.

## Configuring connections to VMs

Once an Azure VM has been created, we must consider how to allow administrative access. We will look at the access methods of **Windows Remote Management (WinRM)** and **Remote Desktop Services Protocol (RDP)** in the following sections.

### WinRM access

We can connect via PowerShell Remoting to run PowerShell commands on remote computers such as Azure IaaS VMs; this uses the WinRM protocol and allows us to establish a remote connection to the computer.

To enable PowerShell Remoting, we need to use the following PowerShell cmdlet: Enable-PSRemoting. The WinRM service will be started so that commands can be operated on the remote computer. The ports used are HTTP: 5985 and HTTPS: 5986; all PowerShell remote communications are always encrypted by WinRM regardless of the protocol used (whether HTTP or HTTPS).

The outline of the process for setting up WinRM access to an Azure VM is as follows:

- 1. An Azure key vault is created.
- 2. A self-signed certificate is created.
- 3. The self-signed certificate is stored in Azure Key Vault.
- 4. The URL for the self-signed certificate stored in Azure Key Vault is retrieved.
- 5. When creating a VM, the URL for the self-signed certificate is referenced for access.
- 6. Ensure the VM is enabled for WinRM by enabling PSRemoting with the following PowerShell cmdlet:
  ```
  Enable-PSRemoting -Force
  ```
- 7. A connection can then be made using the following:
  ```
  Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
  ```

Further information on setting up can be found in the following Microsoft documentation: https://docs.microsoft.com/en-gb/azure/virtual-machines/windows/connect-winrm.

### RDP access

As an alternative remote connection method, we can connect to an Azure VM using a full interactive desktop session using RDP for Windows VMs and Secure Socket Shell (SSH) for Linux servers. For the scope of the exam objective, we will cover Windows Server access using RDP.

Connectivity to the Windows Server Azure VM requires using TCP port 3389 by default. If you are using a firewall or a Network Security Group (NSG), you will need to allow inbound access to this port and protocol; for security purposes, you limit the IP address range you will accept connections from. Further details on configuring the NSG for RDP access can be found in the following Microsoft documentation: https://docs.microsoft.com/en-us/troubleshoot/azure/virtual-machines/troubleshoot-rdp-nsg-problem.

If you are not using a VPN or ExpressRoute to connect to resources in the Azure environment, you must also have a public IP associated with the VM. Further details on configuring this can be found in the following Microsoft documentation: https://docs.microsoft.com/en-gb/azure/virtual-network/ip-services/associate-public-ip-address-vm.

Chapter 6 included a hands-on exercise for remotely accessing an Azure VM using RDP and configuring an NSG to allow inbound RDP traffic.

In this section, we looked at configuring connections to VMs. In the next section, we will complete a hands-on exercise section to reinforce some of the concepts covered in this chapter.

## Hands-on exercise

To support your learning with some practical skills, we will utilize the concepts and understanding gained from the earlier sections of this chapter and put them into practical application.

We will look at the following exercises:

- Exercise – managing a data disk using the Azure portal
- Exercise – resizing an Azure VM

### Getting started

To get started with this section, if you do not already have an Azure subscription, you can create a free Azure account at https://Azure.microsoft.com/free. This free Azure account provides the following:

- 12 months of free services
- $200 credit to explore Azure for 30 days
- 25+ services that are always free

Let's move on to the exercise for this chapter.

### Exercise – managing a data disk using the Azure portal

This exercise will look at managing data disks using the Azure portal.

For this exercise, you will need an Azure VM created to attach and resize data disks; once you have created a VM, return here to complete the exercise or continue if you have an existing VM to use.

In the following sub-sections, you can see the procedure to complete the exercise, segregated into tasks for a better understanding.

