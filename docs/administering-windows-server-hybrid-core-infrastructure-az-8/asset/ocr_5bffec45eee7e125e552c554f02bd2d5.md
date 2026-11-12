registered for **Update Management** cannot report to *more than one* Log Analytics workspace. System information updates from SCOM agents in a management group connected to the workspace can be used to collect information.

Windows machines must use Microsoft Update to report to **Windows Server Update Services (WSUS)**.

When **Update Management** is enabled, a *Hybrid Runbook Worker group* is automatically added to the automation account. All machines connected directly to the Log Analytics workspace have the *System Hybrid Runbook Worker* configured to support the **Update Management** runbooks.

The following are the supported Windows OSs for **Update Management**:

+   * Windows Server 2008 R2 (*RTM and SP1 standard*)
+   * Windows Server 2012
+   * Windows Server 2012 R2 (Standard/Datacenter)
+   * Windows Server 2016 (Standard/Datacenter, *not Server Core*)
+   * Windows Server 2019 (Standard/Datacenter, *including Server Core*)

At the time of publishing, the *Windows Client OS* (Windows 7/10/11) is not supported for **Update Management**. Microsoft Endpoint Configuration Manager is the recommended patch management solution for **Azure Virtual Desktop (AVD)**.

This section introduced Azure Automation for hybrid servers and covered the Azure **Update Management** capability. The next section will look at Microsoft Defender for Cloud for hybrid Windows servers.

# # Implementing Microsoft Defender for Cloud for hybrid Windows servers

Microsoft Defender for Cloud is Microsoft’s **Cloud Security Posture Management (CSPM)** and **Cloud Workload Protection Platform (CWPP)** solution. We can use it to monitor and manage the security posture of hybrid Windows servers in on-premises environments.

In addition, we can implement Microsoft Sentinel, which provides a cloud-native SIEM and **security orchestration, automation, and response (SOAR)** solution for onboarded hybrid Windows servers.

When Defender for Cloud is enabled, the Log Analytics agent needs to be deployed to the machines; the agents then send data to a connected Log Analytics workspace. Defender for Cloud can then determine the security posture of the machines, applications, data, and networks and provide a secure score.

For Azure VMs, the Log Analytics agent is automatically provisioned by Defender for Cloud. For on-premises hybrid machines, the agent can be manually installed or automated through a solution such as Microsoft Endpoint Protection Manager.