# Note

Disabling administrative shares on servers, particularly **Domain Controllers (DCs)**, can have a significant impact on the functionality and operation of systems within a domain-based environment. Therefore, it is important to proceed with caution if you plan to disable this feature. Furthermore, if PsExec is being used in your environment, disabling the admin (ADMIN$) share may limit the functionality of this software.

The way to disable the feature is either by adding a registry key or disabling the `LanmanServer` service. You can disable the administrative shares by adding the following registry value:

```
HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters
DWORD Name = "AutoShareServer" Value = "0"
```

Then run the following commands from an administrative prompt, such as CMD or PowerShell:

```
net stop server
net start server
```

Be aware that this change does not apply to the IPC$ share or shares that you create manually.

The second approach is to disable the `LanmanServer` service, which will affect the ability to share files and printers with other devices on our network. It is also mentioned in the security guidelines for Windows Server from Microsoft that it should not be disabled and is therefore not recommended: https://learn.microsoft.com/en-us/windows-server/security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server.

Also, one thing that I’ve been using more and more is the use of a canary token. A canary token is a type of honeypot or decoy system used in computer security. It’s a special type of URL or file that is created to mimic a target that an attacker might be interested in, such as a login page or a sensitive document. When accessed, the canary token sends an alert, allowing us to track the source of the attack and respond accordingly. The idea behind a canary token is to provide early warning of an attack so that the target can take action to prevent or mitigate the damage.

While there are many options for the tool and **Software as a Service (SaaS)**, here is an example with the use of one canary offering together with Microsoft Sentinel: https://learnsentinel.blog/2022/03/24/deception-in-microsoft-sentinel-with-thinkst-canaries/.

These types of tools can be useful for detecting in the initial phases when the ransomware attackers are trying to find sensitive information on your network shares, so having a canary token for a file called secret or sensitive information would perhaps trigger an alert.