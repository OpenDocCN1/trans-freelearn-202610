
The initial step is to check whether you find any unwanted or abnormal folders on the operating system:

- C:\users\download (for ISO files, ZIP files, or INK files) – Also check whether there are any mounted files currently on the machine
- C:\windows\temp (for .exe or .bat scripts)
- C:\users\username\appdata\local\temp (for .exe or .bat scripts)
- C:\ – If there are any folders here with a recent timestamp, in most cases, the attackers create a temp folder where they store scripts and vulnerabilities that they use

Depending on what kind of ransomware variant was used here, sometimes, there may be nothing to find in other files because the attackers deleted all traces before they started the encryption process. Also, the folder used during the initial phases of discovery might also differ depending on the ransomware variant.

If we do not find anything there, the second part is to try and look for abnormal events in the event viewer, which might give us an indication of what happened. In the following example, we have a payload that was delivered using an ISO file, via email, where a user opened and executed the malware.

Windows logs ISO mounting within Event Viewer using Event ID 12 in Microsoft-Windows-VHDMP-Operational.evtx, as shown in the following screenshot:

![](img/7b49a27a3c2adc52d097f2e77f3bf394_230_0.png)

Figure 8.1 – VHD file mounted on a machine

In addition, you should also look for the following event IDs:

- 1000 – Application errors
- 1 – Service started
- 1102 – Audit log cleared
- 1116 – Microsoft Defender Antivirus malware detected
- 1117 – Microsoft Defender Antivirus malware action taken
- 4624 – Network logon (mostly indicate RDP login)
- 4688 – Process creation events
- 4700 – A scheduled task was enabled
- 7045 – New service was installed on the system

The ransomware might also use different mechanisms to handle persistent access, so it is also important to check for any abnormal tasked jobs and BITS jobs. Firstly, we can check whether there have been any changes to tasked jobs in Event Viewer under the Microsoft-Windows-Task-Scheduler/Operational event log.

> **Note**
> You can check whether the event log has been cleared by looking for Event ID 1102 under the Security log. This is often done by the attackers to try and clear any evidence and is often one of the final things that is done before the encryption process starts.

If the event logs have been cleared, we can check for tasked jobs using the following PowerShell command:

```
Get-ScheduledTask | format-list
```

Or you can use the Task Scheduler application to get a more visual view of the different jobs. The following example shows a scheduled task run every 5 minutes and is used as part of the STOP/DJVU ransomware:

![](img/7b49a27a3c2adc52d097f2e77f3bf394_231_0.png)

Figure 8.2 – Task Scheduler showing a task to maintain persistent access

介绍 Intel RealSense LiDAR Camera L515，它是一款具有高精度depth感知、适用于室内应用的小型深度摄像头。

![](img/7b49a27a3c2adc52d097f2e77f3bf394_232_0.png)

## 主要特点

- 高精度深度感知
- 小型轻量化设计
- 适用于室内应用
- 支持多种开发平台

## 技术参数

| 参数 | 规格 |
| --- | --- |
| 深度分辨率 | 1024×768 |
| 深度帧率 | 30 fps |
| 测量范围 | 0.25m - 9m |
| 接口 | USB 3.0 |

We can look for scheduled tasks as an initiating process:

```
DeviceProcessEvents
| where FileName =~ "schtasks.exe"
| project Timestamp, DeviceName
```

Alternatively, we can look at indicators from registry changes in case the attackers have used other initiating processes to set up scheduled tasks:

```
DeviceRegistryEvents
| where ActionType == @"RegistryKeyCreated"
| where RegistryKey startswith @"HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree\"
| project Timestamp, DeviceName, ActionType, InitiatingProcessFileName, RegistryKey
```

By running certain queries within Microsoft Defender for Endpoint, it is possible to determine whether a scheduled task has been added to the system. While a further investigation into the registry may be necessary to determine the purpose of the task, Microsoft Defender for Endpoint can also be used to detect other known malicious activities, such as tasks that disable system restoration or delete backup copies. This can indicate potential security threats to the system:

```
DeviceProcessEvents
| where FileName =~ "wmic.exe"
| where ProcessCommandLine has "shadowcopy" and ProcessCommandLine has "delete" | project DeviceId, Timestamp, InitiatingProcessFileName, FileName, ProcessCommandLine, InitiatingProcessIntegrityLevel, InitiatingProcessParentFileName
DeviceProcessEvents
| where InitiatingProcessFileName =~ 'rundll32.exe' and InitiatingProcessCommandLine !contains " " and InitiatingProcessCommandLine != "" and FileName in~ ('schtasks.exe') and ProcessCommandLine has 'Change' and ProcessCommandLine has 'SystemRestore' and ProcessCommandLine has 'disable'
```

These preceding examples will run the query against all devices from which it has collected information. You can also add a filter to run the query only against a specific device, such as using this additional line in your query:

```
| where DeviceName == "nameofdevice"
```

Once we have the information about what kind of group policies were changed and what kind of settings were changed, we should try and revoke those settings. In most cases, however, I have seen that the attackers use a script that automatically creates new group policies that apply all these settings, so removing these policies should revert most of the changes.

## Closing the door

While we have focused a lot on how to analyze the ransomware variant, we also need to ensure that we can close the door that the attackers used so that we do not fall victim to the same approach again.

With the example I mentioned earlier of Quantum ransomware, their primary attack vector tends to be phishing emails. However, what if we were hit by something else?

A timeline is an important tool for understanding the origin of an attack. When gathering evidence from user accounts and logs, it is important to create a timeline that traces the first events that may have indicated initial compromise. To do this, consider the primary methods of compromise and work backward to determine how the attack may have occurred:

- End user workstations
- Misconfigured servers that are publicly available
- A zero-day vulnerability on an external service

If the initial compromise occurred through a phishing email sent to an end user, it is important to determine what type of attachment was used to start the malware loader on the end user’s machine. This information can help to identify the source of the attack and understand how it was able to gain access to the system.

Firstly, you need to go through the following:

- Block the sender and emails from that specific domain. This is only a temporary solution since many phishing domains have a short lifespan.
- If the attachment that was sent used a malformed attachment type that is not regularly used, block that type of attachment in the email service.
- If the attack used a known document attachment, such as .docx or .xlsx, we need to analyze the attachment. Upload the attachment containing the malicious document to an analytics site such as virustotal.com. As an example, here is a VirusTotal report showing analytics related to a zero-day vulnerability in Microsoft Office called Follina, which provides some insight into the capabilities of VirusTotal: https://www.virustotal.com/gui/file/4a24048f81afbe9fb62e7a6a49adbdlfaf41f266b5f9feecdceb567aec096784/detection.
- Once you know which vulnerability was used, you need to understand how to remove that vulnerability. With the Follina exploit, there is a software update available from Microsoft that

removes the ability to abuse that vulnerability, as documented here: https://msrc-blog.microsoft.com/2022/05/30/guidance-for-cve-2022-30190-microsoft-support-diagnostic-tool-vulnerability/.

It should be noted that endpoints should automatically have the latest security patches installed for the operating system, browsers, and Office products automatically. This then closes the door for attackers to be able to utilize that exploit.

Now, what if the attack did not start with an end user but with a misconfigured service, such as a server that is made publicly available using RDP? It should be noted that one of the most common entry points that Microsoft sees within Microsoft Azure is servers that have a public IP and RDP open.

To detect what services and ports are publicly available, it is necessary to map out the services that are accessible from the internet. This can help to identify any vulnerabilities or potential security risks.

Looking at the firewall may give some good indication depending on what kind of vendor you have, but sometimes, to double-check, I tend to start with a simple port scan of the public IP range of your organization using a tool such as Nmap.

You can download Nmap from here - https://nmap.org/download.html - which is a CLI tool used to easily scan an IP range using the following command:

```
Nmap -Pn 192.168.0-200
```

This will do a port scan against the entire network between 192.168.0.0 and 192.168.0.200. The -Pn option tells Nmap to assume that the host is online and to skip host discovery. It will perform a ping scan on the IP address range of 192.168.0-200, which is the private IP address range for the class C network. It will scan all the hosts on the network and attempt to map the network and identify the hosts, operating systems, and services running on them. Another option is to use tools such as Microsoft External Attack Surface Management or Shodan, which can also do a more in-depth scan.

If you find a server open with a service such as RDP, you should look at the logs that are collected from that particular server to check for any authentication attempts in the security log, which can indicate compromise. If you have Sentinel enabled for that particular server, you can use the following KQL query to look for RDP brute-force attacks:

```
SecurityEvent
| summarize
LOGONFAILED=countif(EventID == "4625"),
by IpAddress, Computer
```

If you find the server in this query has a high number of failed login attempts, it will most likely indicate that there has been a brute-force attack against the machine. It should also be noted that if you use public cloud services to host your virtual machines, you might also have public IP addresses from the cloud provider that you need to check whether services are externally available through.
