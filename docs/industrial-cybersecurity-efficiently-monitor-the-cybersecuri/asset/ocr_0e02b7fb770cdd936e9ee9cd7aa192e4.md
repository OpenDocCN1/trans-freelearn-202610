# Exercise 2 – Using Wazuh to add PowerShell Script Block Logging

In this exercise, we are going to add **PowerShell Script Block logs** to the Wazuh collection capabilities set. **PowerShell Script Block Logging** is a feature that comes with later versions of PowerShell (**PowerShell v5+**). It allows us to instruct Windows to create an event log for every command run in the PowerShell Terminal. A recent trend sees attackers moving away from traditional means of targeting systems through specialized tools and applications, and favoring a **living-off-the-land** approach, where they use native tools present on the computer systems being attacked instead. These renewed tactics make monitoring for and blocking of malicious tools more difficult—what are you going to do: block `cmd.exe` or `powershell.exe`? So, being able to record every command run on these native tools is a welcome addition to security monitoring and is often the only way to detect malicious activities.

As an added bonus, stemming from the way logging occurs is that the event log entry is not created until after the command is optionally decoded. So, in case someone is trying to fool the system with an `-EncodedCommand` PowerShell command, which takes a Base64-encoded command to run, the command is first decoded before it is logged. In that way, `powershell -EncodedCommand 'ZABpAHIAIA BjADoAXAA='` will be logged as `dir c:\`, as illustrated here:

![](bbox=[0.26, 0.55, 0.75, 0.59])

Figure 9.6 – Exercise 2: Decoded PowerShell command

Follow along with these instructions to get **PowerShell Script Block Logging** configured:

+   1. **PowerShell Script Block Logging** is enabled through a **Group Policy** setting. We can do this on the local machine (`gpedit.msc`), for Windows computers that are not part of a Windows domain (standalone machines), or it can be done at the domain level by configuring the correct **Group Policy Object (GPO)** setting. We will show the GPO option here, but apart from having to use the `gpedit` utility, the standalone method is identical. To start, log in to the domain controller VM (OT-DC1).