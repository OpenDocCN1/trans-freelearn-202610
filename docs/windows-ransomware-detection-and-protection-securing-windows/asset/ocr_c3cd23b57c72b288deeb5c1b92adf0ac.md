Depending on what kind of operating system you have running on the virtual machine, you also have other different extensions available from the list. For instance, if you did not choose the option during the creation of the virtual machine, you also have the option to install an Azure AD-based login extension or the auto-manage extension, which we will cover shortly.

Extensions can also be used to run custom scripts, for example, when a virtual machine is created or after a machine is created and running. This can be, for instance, combined with **Infrastructure as Code (IaC)** tools to automate the creation of virtual machines and install agents.

The next part is getting automated patching for virtual machines to ensure that security updates are installed. Within Azure, you have some different options when it comes to patch management:

+   - Automatic, by the platform
+   - Update Management using Azure Automation (for Windows Server and Linux)
+   - Endpoint Manager (for Windows 10/11 running in Microsoft Azure)

"Automatic, by the platform" refers to automated patch management by Azure, which has the following features:

+   - Patches that are classified as *critical* or *security* are automatically downloaded and installed on the VM by the platform.
+   - Patches are automatically installed during off-peak hours depending on the VM's time zone.
+   - Azure manages the patch orchestration process and applies patches according to availability-first principles. This means that system prioritization is given to maintaining the availability of services and systems, rather than immediately applying all available patches.

The service monitors the VM's health post-update to verify that a patch has successfully been installed.

If you have workloads that are spread across different regions or availability sets, the service will ensure that patches are only applied to one region, one availability zone, and one availability set at a time. This is to ensure the availability of the workload.

This feature can be enabled either through IaC, PowerShell, or the Azure portal. If you already have a VM running, you can enable the feature via PowerShell using the following commands:

```
Set-AzVMOperatingSystem -VM $VirtualMachine -Windows
-ComputerName $ComputerName -Credential $Credential
-ProvisionVMAgent -EnableAutoUpdate -PatchMode
"AutomaticByPlatform"
```

If you want more control over when the patches should be installed, you will need to use Azure Update Management. Update Management is a combination of two services, namely Azure Automation and Log Analytics.