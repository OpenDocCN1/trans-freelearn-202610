Here, if you run one of the following scenarios, such as **Block creation of child process by Office application**, it should trigger an event that will be generated under **Microsoft-Windows-Windows Defender/Operational** with an **EventID** value of **1122**, where it says **Microsoft Defender Exploit Guard audited an operation that is not allowed by your IT administrator**, as shown in the following screenshot:

![](bbox=[0.212, 0.213, 0.807, 0.544])

Figure 4.3 – Exploit Guard Event ID

> **Note**
> 
> For instance, the rule that blocks the creation of child processes by the Office application was one of the mitigations for the MSHTML CVE vulnerability that came in 2021, which you can read more about at `https://www.microsoft.com/security/blog/2021/09/15/analyzing-attacks-that-exploit-the-mshtml-cve-2021-40444-vulnerability/`.

The best approach is to configure as many of these rules as possible, starting with a small group such as the IT department and other power users, by collecting audit logs and determining whether there are any ASR rules that might block potential applications. Once the verification is complete, you should define the ASR rules in blocked mode by default.