writing this book, the latest version is 7.2, which can be found at https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.2.

Second, we should enable some additional logging mechanisms on the PowerShell engine to monitor for suspicious activity. This requires that we are collecting our endpoint's event logs to be able to monitor for abnormal use of PowerShell. These logs should be collected to a centralized log service using a SIEM tool or product such as Microsoft Sentinel. It should also be noted that Defender for Endpoint and other EDR products also provide insight into the usage of PowerShell cmdlets and modules.

In PowerShell, we have the following log mechanisms:

+   - **Engine lifecycle logging** – This allows PowerShell to log the startup and termination of the PowerShell session on the device. If it is running PowerShell higher than version 5, it will also log command-line arguments. By default, engine lifecycle logging is enabled and can be accessed in the event viewer under Applications and Services Logs\Microsoft\Windows\PowerShell\Operational log.
+   - **Module/pipeline logging** – Starting with PowerShell version 3.0, it is possible to log pipeline events to Windows Event Logs on a module-by-module basis or globally. This feature can be enabled through Group Policy or using Endpoint Manager.
+   - **Script block tracing** – With version 5.0, it is possible to log detailed information, including the code that was run, to the event log. This output can provide insight into the actions taken by the system.

Now, by default, PowerShell should be set to only allow signed scripts. However, it is important to note that there are numerous methods of bypassing the script execution policy, so this is just one small part of defining security in PowerShell.

Many ransomware attacks also leverage PowerShell to download additional payload as well as use commands such as invoke-webrequest.

Invoke-WebRequest is a PowerShell cmdlet that allows you to send HTTP and HTTPS requests to a web page or web service. It can be used to download the contents of a web page, download files from a web server, and upload data to a web server.

When used in the context of ransomware, it can be used to download and execute malware payloads or scripts from a command and control server, thereby allowing an attacker to remotely control the compromised system. Additionally, it's used to exfiltrate sensitive data from the victim's network.

In terms of PowerShell, there is also additional guidance available from NSA and CSI, which can be found here: https://media.defense.gov/2022/Jun/22/2003021689/-1/-1/1/CS I_KEEPING_POWERSHELL_SECURITY_MEASURES_TO_USE_AND_EMBRACE_20220622.PDF.