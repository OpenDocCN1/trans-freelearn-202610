You can download Mimikatz here to try it out yourself (https://blog.gentilkiwi.com/mimikatz); however, it should be noted that you should avoid this on a corporate device and should only be tried for educational purposes on an isolated device. You can read more about the different commands and use cases here https://adsecurity.org/?page_id=1821.

> **Note**
> If you are downloading Mimikatz, it will most likely trigger SmartScreen in Microsoft Edge and Microsoft Defender on the **operating system (OS)**, and will mark it as a Trojan and block you from downloading it. This is expected and it should only be used for educational purposes.

As an example shown in the following screenshot, I can, as long as I have administrator privileges, extract either a hash of a user’s password or the password by using Mimikatz:

![](bbox=[0.16, 0.36, 0.82, 0.63])

Figure 10.1 – Mimikatz and collecting a local password hash

The sekurlsa::logonpasswords command in Mimikatz is used to extract the credentials stored in the **Local Security Authority (LSA)** process’ memory on a Windows system. The LSA process is responsible for authenticating users who log on to the system, and it stores the credentials of users who have recently logged on in memory for use in authenticating future service requests.

The sekurlsa::logonpasswords command in Mimikatz is used to extract these credentials, which include the user’s password, **New Technology LAN Manager (NTLM)** and **LAN manager (LanMan)** hash, and Kerberos tickets. The extracted credentials can be used to authenticate to other network resources as the user or to perform password-cracking attacks.

It should be noted that the use of this command or similar tool can be illegal and used for malicious purposes and should only be used in a controlled environment with proper authorization or permission.