# Remote management services

In most ransomware cases I have encountered, attackers were able to leverage RDP to access and jump between servers inside a corporate network. Microsoft has also stated in their annual report, *Microsoft Digital Defence Report 2022*, which can be found here – https://www.microsoft.com/en-us/security/business/microsoft-digital-defense-report-2022 – that RDP is also one of the main attack vectors when it comes to ransomware attacks.

**Note**
It's a common joke that RDP stands for Ransomware Deployment Protocol, but there is some truth behind this. According to Shodan, there are over 4 million servers on the internet that have RDP enabled and are publicly accessible.

The issue with RDP is that it does not have any built-in MFA mechanism by default. You can, publish RDP with MFA via other **Remote Desktop Services (RDS)** features, such as RDS Gateway, which is part of Windows Server.

Using RDS Gateway to publish it externally, all RDP traffic will be proxied via the gateway, which uses TCP 443 (and UDP 3391). RDS Gateway can be configured using MFA via the NPS server role, but this still requires that you expose RDS Gateway to the internet to provide remote access.

Equally, when setting up Windows servers on the public cloud, cloud providers always make it almost too easy to set up a machine with a public IP address and RDP enabled for remote management. This unfortunately has the side effect of making the machine in question a popular target of brute-force attacks, which can be targeted almost within minutes of becoming available.

Therefore, it is important to try and reduce the usage of RDP to the bare minimum, not just for server management but also for remote access. You should never under any circumstances have RDP servers publicly available.

Regarding remote management for Windows-based infrastructure, you should implement the following:

+   - Ensure that for servers that need to have RDP enabled that you have **Network Level Authentication (NLA)** enabled. NLA provides an extra level of authentication before a connection is established and is enabled by default for Server 2012 R2 and upwards.
+   - Ensure that you have disabled drive mapping and clipboard mapping for servers that require RDP to be enabled. This is because ransomware operators often use the clipboard feature to copy files from a compromised machine to the destination server.
+   - Ensure that for servers that require RDP access, you restrict what kind of users and groups can log in. By default, local administrators and remote desktop users are allowed to log on. One best practice here is to remove administrators’ ability to log in through RDP.