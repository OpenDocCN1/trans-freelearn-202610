
This section looked at implementing IPAM, including the different deployment topologies. In this next section, we will look at how to manage IPAM.

## Managing IPAM

In the previous sections, we learned that IPAM provides a unified view of the three core networking services areas in an IT estate of a DHCP server, DNS zones, and IP address management.

The following are some of the tasks that can be carried out across these three areas:

- Manage DNS server tasks:
  - Examine DNS servers and zones
  - Create DNS zones and records
  - Manage conditional forwarders
  - Access the DNS console for any IPAM-managed server
- Manage DHCP server tasks:
  - Configure DHCP servers
  - Examine, create, and edit scopes
  - Discover assignment of specific IP addresses
  - Create reservations
  - Edit server properties and options
  - Import and replication
  - Configure failover
  - Access the DHCP console for any IPAM-managed server
- Manage IP addressing tasks:
  - Discover DHCP server address spaces and data on utilization
  - Detect overlapping IP address spaces across DHCP servers
  - Import IP address information from CSV files
  - Monitor DHCP and DHCP servers
  - Collect data from NPS servers and AD DS domain controllers, such as sign-in information

RBAC is used to provide granular access control for who can carry out tasks. You define a **role, access scope**, and **access policies** to manage these operation tasks. There are several built-in _role-based security groups_. We will not cover these aspects here in this chapter; we have, however, provided links to reference information in the _Further reading_ section at the end of this chapter.

This section concluded with the topic of managing on-premises IP addresses using IPAM. We looked at what IPAM is, how it works, how to implement it, and how to manage it. In the next section, we look at using Windows Server to provide remote access to on-premises environments.

## Implementing and managing remote access

This section will look at implementing on-premises network connectivity using the options available in Windows Server.

The first skills area we will look at in this section is _what are the remote access options in Windows Server?_

### What are the remote access options in Windows Server?

While there are many third-party vendor remote access solutions, in this section, we will look to introduce some of Windows Server's built-in remote access capabilities.

### What is DirectAccess?

Microsoft's **DirectAccess** is one of the technology options that can be implemented for secure remote network access, although it should probably now be considered legacy. This remote network access technology was intended as an alternative to the traditional _virtual private network_ (VPN) approach of secure remote access to an organization's resources on its private networks. It was first introduced with Windows Server 2008 R2 and Windows 7.

### What is the Remote Access Service?

The Windows Server **Remote Access Service (RAS)** server role can also be used, which implements the following remote network access technology services:

- Routing
- VPN
- Web Application Proxy (WAP)

The RAS role is installed on Windows Server using Server Manager or PowerShell. The RAS role is managed using the **Remote Access Management** console, the **Routing and Remote Access (RRAS)** console, or **PowerShell**.

### What is routing?

This comprises routing protocols and routing tables to direct traffic (network packets) across a network(s) to their desired destination using the most efficient route. This is represented in the following diagram:

![](img/cf35f46a8dd5f11e58349c1d81611b75_185_0.png)

Figure 5.22 – Routing

To use an analogy, we can think of this as satellite navigation for network packets; it requires knowing where the packet is coming from and the destination and will calculate and determine the most appropriate and efficient journey. This is less commonly performed by a server, and often this function is performed by a vendor’s specialized hardware appliance.

This section introduced the concept of network routing. In the following section, we look at using VPNs in Windows Server.

### What is a VPN?

This can be considered almost the de facto way to provide users with a secure way to access an organization’s resources and services on its internal private networks when they are not directly connected. An encrypted tunnel is set up between the organization’s sites and the user’s remote location outside the organization’s private networks. The following diagram aims to represent this:

![](img/cf35f46a8dd5f11e58349c1d81611b75_186_0.png)

Figure 5.23 – VPN

When selecting a VPN as the remote access solution in Windows Server, you will need to select a tunneling protocol and an authentication option.

Here are the tunneling protocol options:

- Secure Socket Tunneling Protocol (SSTP)
- Internet Key Exchange version 2 (IKEv2)
- Layer 2 Tunneling Protocol with Internet Protocol Security (L2TP/IPsec)
- Point-to-Point Tunneling Protocol (PPTP)

Due to security vulnerabilities in PPTP, IKEv2 should be used, offering advantages over L2TP.

Here are the authentication protocol options:

- Password Authentication Protocol (PAP): The least secure as passwords are in plain text.
- Challenge-Handshake Authentication Protocol (CHAP): Uses challenge-response and a Message Digest 5 (MD5) industry standard hashing for response encryption. Due to requiring a reversibly encrypted password, Microsoft-CHAPv2 (MS-CHAPv2) should be considered.
- MS-CHAPv2: Uses CHAP but with a one-way encrypted password. Improves on CHAP.
- Extensible Authentication Protocol (EAP): More of an authentication framework than a protocol. One of the common authentication methods in this framework is EAP-Transport Layer Security (EAP-TLS). This is a mutual authentication method that is certificate-based for its authentication.

This section introduced the VPN remote access option in Windows Server. In the following section, we look at using NPS.

### What is NPS?

NPS is a role that can be installed on a Windows Server and is Microsoft's **Remote Authentication Dial-In User Service (RADIUS)** implementation. RADIUS is used to provide an authentication exchange between components in a remote access solution and is an industry-standard authentication protocol.

NPS is used to implement authentication, authorization, and accounting for network access and is based on enforcing policies. NPS provides *connection request policies* and *network policies*. These policies have the following four categories: *Overview*, *Conditions*, *Constraints*, and *Settings*.

The authentication methods available are set out here:

- PAP
- **Shiva Password Authentication Protocol (SPAP)**
- CHAP
- MS-CHAP
- MS-CHAP v2
- EAP

Due to security concerns with the PAP, CHAP, and MS-CHAP authentication protocols, these should not be considered for a production environment.

For accounting, user authentication requests, access messages, accounting requests, responses, and status updates can be stored in a Microsoft SQL Server database.

NPS cannot be installed on Windows Server Core editions.

This section introduced NPS in Windows Server. In the following section, we look at using a **public key infrastructure (PKI)** in Windows Server.

### What is a PKI?

A PKI is a collective terminology that covers implementing, issuing, and managing digital certificates. These certificates are issued by a **certification authority (CA)** and are used to confirm an identity.

Two CA types can provide certificates: a **private CA** and a **public CA**. Each CA type has advantages and disadvantages, as follows:

- A **private CA** provides greater control and lower costs than a public CA and provides customized templates and an auto-enrollment capability. The disadvantage is that private CA-issued certificates are not trusted by external clients and have a higher administrative burden.
- A **public CA** has the advantage that it will be trusted by most external clients and requires lower administration effort. The disadvantage is the higher cost and slower procurement of certificates.
