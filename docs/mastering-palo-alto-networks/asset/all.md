![](Images/Cover.png)

Mastering Palo Alto Networks

Third Edition

Copyright © 2025 Packt Publishing

*All rights reserved*. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

**Early Access Publication**: Mastering Palo Alto Networks

**Early Access Production Reference**: B31589

Published by Packt Publishing Ltd. 

Grosvenor House

11 St Paul’s Square

Birmingham 

B3 1RB, UK.

ISBN: 978-1-83664-481-1

www.packt.com

## Contents

1.  [Mastering Palo Alto Networks: The complete journey to firewall mastery from setup to advanced security](#B31589_Disclaimer.xhtml#_idParaDest-1)
2.  [Understanding the Core Technologies](#B31589_01_Epub.xhtml#_idParaDest-2) 
    1.  [Technical requirements](#B31589_01_Epub.xhtml#_idParaDest-3)
    2.  [Understanding the zone-based firewall](#B31589_01_Epub.xhtml#_idParaDest-4)
    3.  [Expected behavior when determining zones](#B31589_01_Epub.xhtml#_idParaDest-5)
    4.  [Understanding App-ID and Content-ID](#B31589_01_Epub.xhtml#_idParaDest-6) 
        1.  [How App-ID gives more control](#B31589_01_Epub.xhtml#_idParaDest-7)
        2.  [How Content-ID makes things safe](#B31589_01_Epub.xhtml#_idParaDest-8) 
            1.  [Inline evaluation](#B31589_01_Epub.xhtml#_idParaDest-9)
    5.  [The management and data planes](#B31589_01_Epub.xhtml#_idParaDest-10)
    6.  [Authenticating and authorizing users with User-ID](#B31589_01_Epub.xhtml#_idParaDest-11)
    7.  [Summary](#B31589_01_Epub.xhtml#_idParaDest-12)
3.  [Setting Up a New Device](#B31589_02_Epub.xhtml#_idParaDest-13) 
    1.  [Technical requirements](#B31589_02_Epub.xhtml#_idParaDest-14)
    2.  [Gaining access to the user interface](#B31589_02_Epub.xhtml#_idParaDest-15) 
        1.  [Accessing the interface](#B31589_02_Epub.xhtml#_idParaDest-16)
        2.  [Connecting to the web interface and CLI](#B31589_02_Epub.xhtml#_idParaDest-17)
    3.  [Adding licenses and setting up dynamic updates](#B31589_02_Epub.xhtml#_idParaDest-18) 
        1.  [Creating a new account](#B31589_02_Epub.xhtml#_idParaDest-19)
        2.  [Registering a new device](#B31589_02_Epub.xhtml#_idParaDest-20)
        3.  [Activating licenses](#B31589_02_Epub.xhtml#_idParaDest-21) 
            1.  [Activating licenses via the customer support portal](#B31589_02_Epub.xhtml#_idParaDest-22)
            2.  [Activating licenses via the web interface](#B31589_02_Epub.xhtml#_idParaDest-23)
        4.  [Downloading and scheduling dynamic updates](#B31589_02_Epub.xhtml#_idParaDest-24)
    4.  [Upgrading the firewall](#B31589_02_Epub.xhtml#_idParaDest-25) 
        1.  [Understanding the partitions](#B31589_02_Epub.xhtml#_idParaDest-26)
        2.  [Upgrade considerations](#B31589_02_Epub.xhtml#_idParaDest-27)
        3.  [Upgrading via the CLI](#B31589_02_Epub.xhtml#_idParaDest-28)
        4.  [Upgrading via the web interface](#B31589_02_Epub.xhtml#_idParaDest-29)
        5.  [Limiting access via an access list](#B31589_02_Epub.xhtml#_idParaDest-30)
        6.  [Accessing internet resources from offline management](#B31589_02_Epub.xhtml#_idParaDest-31)
        7.  [Admin accounts](#B31589_02_Epub.xhtml#_idParaDest-32) 
            1.  [Dynamic accounts](#B31589_02_Epub.xhtml#_idParaDest-33)
            2.  [Role-based administrators](#B31589_02_Epub.xhtml#_idParaDest-34)
            3.  [Password security](#B31589_02_Epub.xhtml#_idParaDest-35)
        8.  [External authentication](#B31589_02_Epub.xhtml#_idParaDest-36) 
            1.  [The TACACS+ server profile](#B31589_02_Epub.xhtml#_idParaDest-37)
            2.  [The LDAP server profile](#B31589_02_Epub.xhtml#_idParaDest-38)
            3.  [The RADIUS server profile](#B31589_02_Epub.xhtml#_idParaDest-39)
            4.  [The Kerberos server profile](#B31589_02_Epub.xhtml#_idParaDest-40)
            5.  [The SAML server profile](#B31589_02_Epub.xhtml#_idParaDest-41)
            6.  [The MFA profile](#B31589_02_Epub.xhtml#_idParaDest-42)
            7.  [Setting up the authentication profile](#B31589_02_Epub.xhtml#_idParaDest-43)
    5.  [Understanding the interface types](#B31589_02_Epub.xhtml#_idParaDest-44) 
        1.  [VWire](#B31589_02_Epub.xhtml#_idParaDest-45)
        2.  [The Layer 3 interface](#B31589_02_Epub.xhtml#_idParaDest-46) 
            1.  [Exploring the interface](#B31589_02_Epub.xhtml#_idParaDest-47)
            2.  [VR](#B31589_02_Epub.xhtml#_idParaDest-48)
        3.  [The Layer 2 interface and VLANs](#B31589_02_Epub.xhtml#_idParaDest-49)
        4.  [Tap interfaces](#B31589_02_Epub.xhtml#_idParaDest-50)
        5.  [The Decryption Port Mirror interface](#B31589_02_Epub.xhtml#_idParaDest-51)
        6.  [The loopback interface](#B31589_02_Epub.xhtml#_idParaDest-52)
        7.  [The tunnel interface](#B31589_02_Epub.xhtml#_idParaDest-53)
        8.  [Subinterfaces](#B31589_02_Epub.xhtml#_idParaDest-54)
        9.  [HA interfaces](#B31589_02_Epub.xhtml#_idParaDest-55)
        10.  [AE interfaces](#B31589_02_Epub.xhtml#_idParaDest-56)
    6.  [Summary](#B31589_02_Epub.xhtml#_idParaDest-57)
4.  [Building Strong Policies](#B31589_03_Epub.xhtml#_idParaDest-58) 
    1.  [Technical requirements](#B31589_03_Epub.xhtml#_idParaDest-59)
    2.  [Understanding and preparing security profiles](#B31589_03_Epub.xhtml#_idParaDest-60) 
        1.  [The Antivirus profile](#B31589_03_Epub.xhtml#_idParaDest-61)
        2.  [The Anti-Spyware profile](#B31589_03_Epub.xhtml#_idParaDest-62)
        3.  [The Vulnerability Protection profile](#B31589_03_Epub.xhtml#_idParaDest-63)
        4.  [URL Filtering profile](#B31589_03_Epub.xhtml#_idParaDest-64) 
            1.  [Custom URL categories](#B31589_03_Epub.xhtml#_idParaDest-65)
            2.  [Configuring the URL Filtering profile](#B31589_03_Epub.xhtml#_idParaDest-66)
            3.  [URL Filtering priorities](#B31589_03_Epub.xhtml#_idParaDest-67)
        5.  [The File Blocking profile](#B31589_03_Epub.xhtml#_idParaDest-68)
        6.  [The WildFire Analysis profile](#B31589_03_Epub.xhtml#_idParaDest-69)
        7.  [Custom objects](#B31589_03_Epub.xhtml#_idParaDest-70) 
            1.  [The Custom Spyware/Vulnerability objects](#B31589_03_Epub.xhtml#_idParaDest-71)
            2.  [The custom data pattern](#B31589_03_Epub.xhtml#_idParaDest-72)
        8.  [Security profile groups](#B31589_03_Epub.xhtml#_idParaDest-73)
    3.  [Understanding and building security rules](#B31589_03_Epub.xhtml#_idParaDest-74) 
        1.  [Dropping “bad” traffic](#B31589_03_Epub.xhtml#_idParaDest-75) 
            1.  [Action options](#B31589_03_Epub.xhtml#_idParaDest-76)
        2.  [Allowing applications](#B31589_03_Epub.xhtml#_idParaDest-77) 
            1.  [Application dependencies](#B31589_03_Epub.xhtml#_idParaDest-78)
            2.  [Application-default versus manual service ports](#B31589_03_Epub.xhtml#_idParaDest-79)
        3.  [Controlling logging and schedules](#B31589_03_Epub.xhtml#_idParaDest-80)
        4.  [Address objects](#B31589_03_Epub.xhtml#_idParaDest-81)
        5.  [Tags](#B31589_03_Epub.xhtml#_idParaDest-82)
        6.  [Policy Optimizer](#B31589_03_Epub.xhtml#_idParaDest-83) 
            1.  [The Apps Seen column](#B31589_03_Epub.xhtml#_idParaDest-84)
    4.  [Creating NAT rules](#B31589_03_Epub.xhtml#_idParaDest-85) 
        1.  [Inbound NAT](#B31589_03_Epub.xhtml#_idParaDest-86)
        2.  [Outbound NAT](#B31589_03_Epub.xhtml#_idParaDest-87) 
            1.  [Hide NAT or one-to-many NAT](#B31589_03_Epub.xhtml#_idParaDest-88)
            2.  [One-to-one NAT](#B31589_03_Epub.xhtml#_idParaDest-89)
            3.  [U-turn or hairpin NAT](#B31589_03_Epub.xhtml#_idParaDest-90)
            4.  [Enable DNS Rewrite](#B31589_03_Epub.xhtml#_idParaDest-91)
    5.  [Summary](#B31589_03_Epub.xhtml#_idParaDest-92)

# Landmarks

1.  [Cover](cover.xhtml)

# Mastering Palo Alto Networks: The complete journey to firewall mastery from setup to advanced security

**Welcome to Packt Early Access**. We’re giving you an exclusive preview of this book before it goes on sale. It can take many months to write a book, but our authors have cutting-edge information to share with you today. Early Access gives you an insight into the latest developments by making chapter drafts available. The chapters may be a little rough around the edges right now, but our authors will update them over time.You can dip in and out of this book or follow along from start to finish; Early Access is designed to be flexible. We hope you enjoy getting to know more about the process of writing a Packt book.

1.  Chapter 1: Understanding the Core Technologies
2.  Chapter 2: Setting Up a New Device
3.  Chapter 3: Building Strong Policies
4.  Chapter 4: Taking Control of Sessions
5.  Chapter 5: Services and Operational Modes
6.  Chapter 6: Identifying Users and Controlling Access
7.  Chapter 7: Managing Firewalls through Panorama
8.  Chapter 8: Managing firewalls through Strata Cloud manager
9.  Chapter 9: Upgrading Firewalls and Panorama
10.  Chapter 10: Logging and Reporting
11.  Chapter 11: Virtual Private Networks
12.  Chpater 12: Advanced Protection 
13.  Chapter 13: Troubleshooting common session issues
14.  Chapter 14: A Deep Dive into Troubleshooting
15.  Chapter 15: Cloud based firewall deployments

# 1

# Understanding the Core Technologies

Welcome to the first chapter! In this book, we’re going to explore the ins and outs of the Palo Alto Networks Strata suite. We’ll start off by learning about all the different features of the firewall and how to configure them before we move on to more complex features and additional services that will help you complete your deployment. On the way, you’ll pick up important knowledge nuggets that will help you both understand the technology and pass the PCNSE exam.

In this chapter, we’re going to examine the core technologies that make up the Palo Alto Networks firewall. We are going to take a closer look at the way in which security zones control how security, **Network Address Translation **(**NAT**), and routing verdicts are made. We will review the mechanics behind App-ID and Content-ID so you get a deeper understanding of how packets are processed and security decisions are made by the firewall, and we will review how User-ID contributes to a more robust security stance by applying group-based or user-based access control.

This chapter will cover the following topics:

*   Understanding the zone-based firewall
*   Understanding App-ID and Content-ID
*   The management and data planes
*   Authenticating users with User-ID

By the end of this chapter, you will have a better understanding of how the core technology is built up and will be able to apply these skills when we start building configuration. If you’re preparing for the PCNSE exam, this chapter will also help you understand the fundamentals required to tackle some of the scenario-based questions.

# Technical requirements

For this chapter, no physical installation is required; technology is only explained. A good understanding of basic networking protocols like UDP and TCP is necessary to fully benefit from the following materials. It is helpful if you’ve already worked with Palo Alto Networks firewalls, but it is not required. Some experience with firewalls or web proxies in general is recommended, as this will make the subject matter more tangible.

# Understanding the zone-based firewall

Traditionally, when considering a firewall as an element of your network, most likely you will imagine a network design like the one in *Figure 1**.1*, with two to four areas surrounding a box. Most of the time, whatever is placed in the north is considered dangerous as it represents the internet, the east and west are somewhat gray areas as they are the **demilitarized zones** (**DMZs**) that are partly exposed to the internet, and the south is the happy place where users do their daily tasks. The box in the middle is the firewall. All these areas will be defined as zones in the firewall:

![](Images/B31589_01_01.png)

Figure 1.1: Basic network topology

In reality, a network design may look a lot more complex due to network segmentation, remote offices being connected to headquarters via all sorts of different technologies, and the adoption of cloud vendors.

In a **route-based firewall**, zones are simply an architectural or topological concept that helps identify which areas comprise the global network that is used by the company; they are usually represented by tags that can be attached to a subnet object. They hold no bearing in any of the security decisions made by the system when processing security policies.

The **zone-based firewall**, on the other hand, will use zones as a means to internally classify the source and destination in its state table.

The following diagram illustrates the phases of packet processing from the first step when the first packet of a new session enters the firewall to the last step where the packet egresses the firewall:

![](Images/B31589_01_02.png)

Figure 1.2: Phases of packet processing

Let’s look at the process workflow for initial packet processing:

1.  When a packet is first received, a source zone lookup is performed. If the source zone has a protection profile associated with it, the packet is evaluated against the profile configuration. If the first packet is a TCP packet, it will also be evaluated against the TCP state where the first packet needs to be a SYN packet, and a SYN cookie is triggered if the protection profile threshold is reached.
2.  Then, a destination zone is determined by checking the **policy-based forwarding **(**PBF**) rules and, if no results are found, the routing table is consulted.
3.  Lastly, the NAT policy is evaluated as the destination IP may be changed by a NAT rule action, thereby changing the destination interface and zone in the routing table. This would require a secondary forwarding lookup to determine the post-NAT egress interface and zone.

After these zone lookups have been performed in the **initial packet processing**, the firewall will continue to the **security pre-policy** evaluation.

In the pre-policy evaluation, the “six-tuple” (**6-tuple**) is used to match an incoming session against the rule base before establishing or dropping/denying a session. At this stage, the firewall does not consider the application just yet, as this can usually not be determined by the first packet in a session. The six-tuple consists of the following elements:

*   Source IP
*   Source zone
*   Destination IP
*   Destination zone
*   Destination port
*   Protocol

Zones are attached to a physical, virtual, or sub-interface. Each interface can only be part of one single zone. Zones can be created to suit any naming convention and can be very descriptive in their purpose (**untrust, DMZ, LAN**, and so on), which ensures that, from an administrative standpoint, each area is easily identifiable.

It is best practice to use zones in all security rules, and leveraging a clear naming convention prevents misconfiguration and makes security rules very readable. Networks that are physically separated for whatever reason but are supposed to be connected topologically (for example, users spread over two buildings that come into the firewall on two separate interfaces) can be combined into the same zone, which simplifies policies.

It is important to note that there are implied rules that influence intrazone or interzone sessions. These rules can be found at the bottom of the security policy:

*   **Default intrazone connections**: Packets flowing from and to the same zone will be implicitly allowed
*   **Default interzone connections**: Packets flowing from one zone to a different zone are implicitly blocked

Security rules can be set to only accept traffic within the same zone, between different zones only, or both. This setting can be changed in the rule **Type** and is set to **Universal** by default. As illustrated in *Figure 1**.3*, the **Universal** rule allows sessions to flow from all zones in the **Source** field to all zones in the **Destination** field, from **LAN** to **LAN** and **DMZ**, and from **DMZ** to **LAN** and **DMZ**.

Rules set to the **intrazone** type only allow sessions to flow inside the same zone regardless of whether multiple zones are added to the security rule: from **DMZ** to **DMZ** and from **LAN** to **LAN**, but not from **LAN** to **DMZ** or from **DMZ** to **LAN**.

Rules set to the **interzone** type only allow sessions to flow between different zones: from **DMZ** to **LAN** and from **LAN** to **DMZ**, but not from **DMZ** to **DMZ** or from **LAN** to **LAN**, even though both are listed in the source and destination.

This means that you can perfectly control between which interfaces traffic is allowed to flow to even if you are unable to define subnets in the source or destination, which, for traditional firewalls, means sessions will be allowed to flow everywhere.

![](Images/B31589_01_03.png)

Figure 1.3: Different security rule types and default rules

Now that we’ve seen the important role zones play while making security decisions, let’s look at the expected behavior when determining zones.

# Expected behavior when determining zones

When a packet arrives on an interface, the PBF policy or routing table will be consulted to determine the destination zone based on the original IP address in the packet header.

Let’s consider the following routing table:

```
> show routing route
flags: A:active, ?:loose, C:connect, H:host, S:static, ~:internal, R:rip, O:ospf, B:bgp,
      Oi:ospf intra-area, Oo:ospf inter-area, O1:ospf ext-type-1, O2:ospf ext-type-2, E:ecmp, M:multicast
VIRTUAL ROUTER: default (id 1)
  ==========
destination       nexthop       metric flags  interface   
0.0.0.0/0         198.51.100.1  10A S    ethernet1/1                 
198.51.100.0/24   198.51.100.2  0      A C    ethernet1/1
198.51.100.2/32   0.0.0.0       0      A H                           
192.168.0.0/24    192.168.0.1   0      A C    ethernet1/2             
192.168.0.1/32    0.0.0.0       0      A H        
172.16.0.0/24     172.16.0.1    0      A
C    ethernet1/3                
172.16.0.1/32     0.0.0.0       0      A
H                                                                   
total routes shown: 7
```

Let’s assume `ethernet1/1` is the external interface with IP address `198.51.100.2` set to zone **external**, `ethernet1/2` is the DMZ interface with IP address `192.168.0.1` set to zone **DMZ**, and `ethernet1/3` is the LAN interface with IP `172.16.0.1` and set to zone **LAN**. The default route is going out of interface `ethernet1/1` to `198.51.100.1` as a next-hop. There are a few scenarios that will influence how the zone is determined:

*   **Scenario 1**: A packet is received from client PC `172.16.0.5` with destination IP `1.1.1.1`.

The firewall quickly determines the source zone is **LAN** and a route lookup determines the destination IP is not a connected network, so the default route needs to be followed to the internet. The destination zone must be **external** because the egress interface is `ethernet1/1`.

*   **Scenario 2**: A packet is received from client PC `172.16.0.5` with destination IP `1.1.1.1` but a PBF rule exists that forces all traffic for `1.1.1.1` to the next-hop IP `192.168.0.25`.

As PBF overrides the routing table, the destination zone will become **DMZ** as the egress interface is now `ethernet1/2`.

*   **Scenario 3**: A packet is received from internet IP `203.0.113.1` with destination IP `198.51.100.2`. This is a typical example of what **NAT** looks like to the firewall: it receives a packet with its external IP address as the destination.

From the perspective of the NAT policy, the source zone will be **external** as the IP is not from a connected network and no static route exists, and the destination zone will *also *be **external** as the IP is connected to that interface.

From a security aspect, however, once NAT is applied, the destination zone will change to the zone that the post-NAT destination IP is connected to (usually **DMZ**).

**Important note**

Remember that NAT policy evaluation happens after the initial zones have been determined but before the security policy is evaluated. This will cause outbound NAT rules to come from **LAN** and go to **external**, but inbound NAT rules to match as coming from **external** and also going to **external** while the inbound security rule will use the appropriate destination zone. See *Figure 1**.2*.

In this section, we saw how the first round of security decisions relies heavily on zones, which should also reflect any rule base you create going forward: use zones in the source and destination as much as possible to fully control the flow of traffic and prevent unexpected behavior. In the next section, we’ll look at what happens in the second round, which also makes a **next-generation firewall** (**NGFW**) “next generation.”

# Understanding App-ID and Content-ID

App-ID and Content-ID are two technologies that go hand in hand and make up the core inspection mechanism. App-ID relies on decoders to identify and classify flows based on the protocol and layer 7 application. This allows more granular control over what is being allowed or blocked while ensuring an application behaves as expected. Content-ID relies on threat prevention engines to do deep inspection flows for threats, classify URL categories, and prevent data exfiltration.

## How App-ID gives more control

Determining which application is contained within a specific data flow is the cornerstone of any NGFW. It can no longer be assumed that any sessions using TCP ports 80 and 443 are simply plaintext or encrypted web browsing. Today’s applications predominantly use these ports as their base transport, and many malware developers have leveraged this convergence to well-known ports in an attempt to masquerade their malware as legitimate web traffic while exfiltrating sensitive information or downloading more malicious payloads into an infected host.

The following image illustrates the steps taken by App-ID to identify applications within flows:

![](Images/B31589_01_04.png)

Figure 1.4: How App-ID classifies applications

When a packet is received, App-ID will go through several stages to identify just what something is:

1.  First, the **6-Tuple** is checked against the security policy to verify whether a certain source, destination, protocol, and port combination is allowed. This will take care of low-hanging fruit if all the unnecessary ports have been closed off and *unusual* destination ports can already be rejected.
2.  Next, the packets will be checked against known application signatures and the app cache to see if the session can be rapidly identified.
3.  This is followed by a second security policy check against the application, now adding App-ID to the required set of identifiers for the security policy to allow the session through.
4.  If, at this time or in future policy checks, it is determined that the application is SSH, TLS, or SSL, a secondary policy check is performed to verify whether decryption needs to be applied. If a decryption policy exists, the session will go through decryption and will then be checked again for a known application signature, as the session encapsulated inside TLS or SSH may be something entirely different.
5.  If, in this step, the application has not been identified (a maximum of 4 packets after the handshake, or 2,000 bytes), App-ID will use the base protocol to determine which decoder to use to analyze the packets more deeply.
6.  If the protocol is known, the decoder will go ahead and decode the protocol, then run the payload against the known application signatures again. The outcome could either be a known application (like `ssl` or `web-browsing`) or an unknown generic application, like `unknown-tcp`. This generic application can also be controlled via a security policy to allow or block the session.
7.  The session is then re-matched against the security policy to determine whether it is allowed to pass or needs to be rejected or dropped. If the protocol is unknown, App-ID will apply heuristics to try and determine which protocol is used in the session.
8.  Once it is determined which protocol is used, another security policy check is performed.
9.  Once the application has been identified or all options have been exhausted, App-ID will stop processing the packets for identification.

Throughout the life of a session, the identified application may change several times as more information is learned from the session through inspecting packet after packet. For example, a TCP session may be identified as SSL, which is the HTTPS application as the firewall detects an SSL handshake. The decryption engine and protocol decoders will then be initiated to decrypt the session and identify what is contained inside the encrypted session. Next, it may detect application `web-browsing` as the decoder identifies typical browsing behavior such as an HTTP GET. App-ID can then apply known application signatures to identify `flickr`. Each time the application context changes, the firewall will quickly check whether this particular application is allowed in its security rule base.

If, at this point, `flickr` is allowed, the same session may later switch contexts again as the user tries to upload a photo, which will trigger another security policy check. The session that was previously allowed may now get blocked by the firewall as the sub-application `flickr-uploading` may not be allowed.

Once the App-ID process has settled on an application, the application decoder will continuously scan the session for expected and deviant behavior, in case the application changes to a sub-application or a malicious actor is trying to tunnel a different application or protocol over the existing session.

App-ID signatures and decoders are regularly (usually once a month around the 15^(th)) updated to account for changes to existing applications or protocols and adding new signatures for previously unknown applications or sub-applications to existing apps to add more depth and control (for example, Facebook chat, file sharing, or games).

App-ID, therefore, allows you to control not only which sessions are allowed to pass through the firewall but also how these applications are allowed to behave. In the next section, we will look at how threats can be prevented and malware blocked.

## How Content-ID makes things safe

If the appropriate security profiles have been enabled in the security rules, the Content-ID engine will apply the URL filtering policy and will continuously, and in parallel, scan the session for threats like vulnerability exploits, virus or worm infections, suspicious DNS queries, **command and control **(**C&C** or **C2**) signatures, DoS attacks, port scans, malformed protocols, or data patterns matching sensitive data exfiltration. TCP reassembly and IP defragmentation are performed to prevent packet-level evasion techniques. In the following image, you can see how single-pass pattern matching enables simultaneous scanning for multiple types of threats and how URL filtering is added to the mix:

![](Images/B31589_01_05.png)

Figure 1.5: How Content-ID scans packets

All of this happens in parallel because the hardware and software were designed so that each packet is simultaneously processed by an App-ID decoder and a Content-ID stream-based engine, each in a dedicated chip on the chassis or through a dedicated process in a **virtual machine** (**VM**). This design reduces latency versus serial processing, which means that enabling more security profiles does not come at an exponential cost to performance as is the case with other firewall and IPS solutions.

### Inline evaluation

To extend the signature-based evaluation that we just discussed, the firewall also comes with **Inline Machine Learning** (**ML**) and **Inline Cloud Analysis** capabilities that allow it to evaluate dynamic content independently without needing to wait for a signature update to start blocking malicious content. Some of these are as follows:

*   **WildFire** is capable of evaluating **portable executable** (**PE**), **executable and linked format** (**ELF**), MS Office files, PowerShell, and shell scripts in real time by applying ML models.
*   Starting from PAN-OS 11.2, certain chassis (**PA-5400** and **PA-VM** at the time of writing) get local deep learning AI that expands the ML capabilities.
*   **URL Filtering** can leverage local and cloud Inline ML categorization to evaluate website details to protect users from phishing variants and JavaScript exploits.
*   **Anti-Spyware** can tap into live Inline Cloud Analysis to access five analysis engines for C&C-based threats over HTTP, HTTP2, SSL, unknown-UDP, and unknown-TCP.
*   **Vulnerability Protection** can leverage Inline Cloud Analysis to analyze SQL and PowerShell code injection.

All cloud inline detection does require the firewall to have an active internet connection so plan accordingly if internet access is limited for your deployment.

In this section, you learned how all the (OSI) layer 7 content inspection components work together to provide you with more visibility into which applications are traversing the firewall while blocking any malicious payload.

# The management and data planes

Hardware and VM design are focused on enabling the best performance for parallel processing while still performing tasks that cost processing power and could impede the speed at which flows are able to pass through the system. For this reason, each platform is split up into so-called *planes*.

There are two main planes that make up a firewall, the **data plane** and the **management** plane, which are physical or logical boards that perform specific functions. While all platforms have a management plane, larger platforms like the PA-5200 have an additional **control plane** and two to three data planes. The largest platforms have replaceable hardware blades (line cards) that have up to three data plane equivalents per line card and can hold up to 10 line cards. Smaller platforms like the PA-220 only have one hardware board that virtually splits up responsibilities among its CPU cores.

The **management plane** is where all administrative tasks happen. It serves the web interfaces used by the system to allow configuration, provide URL filtering block pages, and serve the client VPN portal. It performs cloud lookups for URL filtering and DNS security, and downloads and installs content updates onto the data plane. It also performs the logic part of routing and communicates with dynamic routing peers and neighbors. Authentication, User-ID, logging, and many other supporting functions are not directly related to processing packets.

The **control plane** takes on the task of facilitating communications between multiple data planes and the management plane as well as monitoring processes on the data planes.

The **data plane** is responsible for processing flows and performing all the security features associated with the NGFW. It scans sessions for patterns and heuristics. It maintains IPsec VPN connections and has hardware offloading to provide wire-speed throughputs. Due to its architecture and the use of interconnected specialty chips, all types of scanning can happen in parallel as each chip processes packets simultaneously and reports its findings.

A switch fabric enables communication between planes so the data plane can send lookup requests to the management plane, and the management plane can send configuration updates and content updates.

The following diagram illustrates how these components interact with each other:

![](Images/B31589_01_06.png)

Figure 1.6: Management and data planes

Now that we’ve covered the most basic functions and you have a firm grasp of how the hardware is organized, let’s look at identity-based authorization. The ability to identify users and apply different security policies based on identity or group membership is an important feature of the NGFW as it allows for more dynamic security rules that don’t rely on static access lists but, instead, allow users to roam inside and outside the campus and still have all the access they need without exposing internal resources.

# Authenticating and authorizing users with User-ID

Frequently neglected but very powerful when set up properly is a standard (no additional license required) feature called User-ID. Through several mechanisms, the firewall can learn who is initiating which sessions, regardless of their device, operating system, or source IP. Additionally, security policies can be set so users are granted access or restricted in their capabilities based on their individual ID or group membership.

User-ID expands functionality with granular control of who is accessing certain resources and provides customizable reporting capabilities for forensic or managerial reporting.

Users can be identified through several different methods:

*   **Server monitoring**: 
    *   Microsoft Active Directory security log reading for log-on and authentication events
    *   Microsoft Exchange Server log-on events
    *   Novell eDirectory log-on events
*   The interception of **X-Forwarded-For** (**XFF**) headers, forwarded by a downstream proxy server
*   **Client probing** using NetBIOS and WMI probes
*   **Direct user authentication**: 
    *   The Captive Portal to intercept web requests and serve a user authentication form or transparently authenticate using Kerberos
    *   GlobalProtect VPN client integration
*   **Port mapping** on a multiuser platform such as Citrix or Microsoft Terminal Server where multiple users will originate from the same source IP
*   The **Extensible Markup Language** (**XML**) API
*   A **syslog listener** to receive forwarded logs from external authentication systems

You will have noticed there are many ways to leverage User-ID, so we will revisit this topic in depth in *Chapter 6*, *Identifying Users and Controlling Access*.

# Summary

Now that you’ve completed this chapter, you are able to identify the strengths of using a zone-based firewall versus a route-based one. You understand how applications can be identified even though they may all be using the same protocol and port, and you understand how deep packet inspection is achieved in single-pass parallel processing. Most importantly, you have a firm grasp of which phases a packet goes through to form a session. It’s okay if this information seems a bit overwhelming; we will see more practical applications, and implications, in the next two chapters. We will be taking a closer look at how security and NAT rules behave once you start playing with zones, and how to anticipate expected behavior by simply glancing at the rules.

If you are preparing for the PCNSE exam, this chapter covered parts of the *Planning and Core Concepts* and *Deploy and Configure* domains. Make note of *Figure 1**.2* regarding packet processing, remember that route lookups and PBF form the basis of zoning, and take note of how App-ID and Content-ID interoperate.

In the next chapter, we will learn how to set up a firewall from scratch and get up and running in no time. We will glance over the physical and virtual components and how to configure them so traffic can flow through, and NAT can be applied where needed.

# 2

# Setting Up a New Device

In this chapter, we will cover how you can gain access to the console and web interface of a fresh-out-of-the-box firewall appliance or a cleanly staged **virtual machine** (**VM**). You will learn how to license, update, and upgrade the firewall so that the latest features are available when you start building your security policy, and the latest signatures are always loaded onto the device to protect your users and infrastructure from malware and vulnerability exploits.

We are going to harden your management configuration to ensure a rigid security stance, and we will also look at the different types of network interface modes—aggregated interfaces and routing.

In this chapter, we’re going to cover the following main topics:

*   Gaining access to the user interface
*   Adding licenses and setting up dynamic updates
*   Upgrading the firewall
*   Hardening the management interface
*   Understanding the interface types

By the end of this chapter, you’ll be able to quickly set up a fresh firewall, register it, and upgrade it to a desirable level in a short amount of time. You’ll be able to apply best practices and leverage strong authentication for your administrative access, and you will be able to quickly identify which interface configuration will suit any given network topology that the firewall needs to be placed in.

# Technical requirements

For this chapter, a basic understanding of network appliances is required as we will be looking at physically connecting to a device, configuring the management environment, and choosing the data plane interface’s deployment mode. Basic knowledge of standing up a virtual appliance in a virtual environment, including connecting it to virtual switches or virtual interfaces and providing it with network access on a hypervisor, is also required.

# Gaining access to the user interface

If you are deploying your firewall on a cloud provider like Azure or AWS, take a look at *Chapter 14*, *Cloud-Based Firewall Deployments*.

When taking a new device out of the box or setting up a VM on a local hypervisor, such as VMware ESXi, Fusion, NSX, Hyper-V, KVM, and so on, one of the first things that needs to be accomplished is making it possible to gain access to the user interface, be it a web interface or a console connection. We’ll go over the most common ways of gaining this access in this section.

One of the first things you *may* need to do is to connect a console cable to gain access to the **command-line interface** (**CLI**). Older models only come with an RJ45 console port, so for those, you will need a standard DB9-to-RJ45 console cable, optionally patched through a serial-to-USB cable so a modern laptop is able to interface with the port. The pinout for the DB9 should be as follows:

```
1 - Empty - Data Carrier Detect (DCD)
2 - 3 - Receive Data (RXD)
3 - 6 - Transmit Data - (TXD)
4 - 7 - Data Terminal Ready (DTR)
5 - 4 - Ground (GND)
6 - 2 - Data Set Ready (DSR)
7 - 8 - Request To Send (RTS)
8 - 1 - Clear to Send - (CTS)
9 - Empty - Ringing Indicator (RI)
```

Luckily, there are USB-to-RJ45 cables available as well that will save you the trouble of figuring out the correct pinouts:

![](Images/B31589_02_01.png)

Figure 2.1: RJ45-to-USB console cable

All but the very old models also come with a micro-USB port, which allows a console connection to be made using a standard USB-A-to-micro-USB cable, as in the following picture:

![](Images/B31589_02_02.png)

Figure 2.2: PA-460 RJ45 and the micro-USB console ports

In all cases, you will need to find which COM (hardware interface) or **TTY** (**TeleTYpewriter**) port is being used on your computer’s operating system.

On a Windows machine, the first time you plug in the cable, a driver may need to be installed. Once the installation has been completed, you need to find the virtual COM port number that has been assigned to the console cable. In most cases, you can determine this virtual COM port number by following these steps:

1.  Open the **Device Manager**.
2.  Click **Start | Control Panel | Hardware and Sound | Device Manager** (under **Devices and Printers**).
3.  In the** Device Manager** list, look in **Ports** and find the virtual COM port assigned to the USB port. This entry will look similar to **USB to Serial Port (COM#)**, where COM# is the number to be used in the following step.

Next, you will need a terminal emulation client to connect to the console. You can use a free client for this, such as PuTTY from [https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

Besides the COM port, you may need to provide more settings to be able to connect. If asked, use these settings:

```
Bits per second: 9600        
Data Bits: 8    
Parity: none              
Stop bits: 1
Flow control: none           
```

On macOS and Linux, a USB serial connection will usually create a new `tty` entry in the `/dev/` directory; a USB-to-DB9 dongle may create a **call-up** (**CU**) entry in the `/dev/` directory.

Find the proper device by searching with either of these commands:

```
ls /dev/tty.*
ls /dev/cu.*
```

You will find `/dev/cu.usbserialxxxxx` or `/dev/tty.usbmodemxxxxx`, where `xxxxx` is the serial device name.

Once you determine the appropriate device, you can connect to the console port by using the `screen` command set to `9600` bits per second:

```
screen /dev/tty.usbmodemxxxxx 9600
```

Now, go ahead and connect the console cable or micro-USB to your laptop and appliance. If you have a port free on your management network, go ahead and connect the firewall’s MGT port to the switch. If you don’t have a management connection available yet, you will need to connect your laptop directly to the MGT port for easier access once the IP is set up on the management interface. Lastly, plug in the power cable.

If the firewall is loaded in a VM or cloud entity, hit the **Start** button to boot up the virtual appliance.

Once you’ve logged on to the console, you will see the operating system boot up, and if the firewall is already connected to a DHCP-enabled management network, you will see something similar to the following, where the DHCP address is already listed for your convenience:

![](Images/B31589_02_03.png)

Figure 2.3: PA-VM post-boot DHCP information

If you missed this information, you can log on and use the following command to see the DHCP information:

```
admin@PA-440> show system info
hostname: PA-440
ip-address: 192.168.27.116
public-ip-address: unknown
netmask: 255.255.255.0
default-gateway: 192.168.27.1
ip-assignment: dhcp
```

If, for some reason, you have not received a DHCP address yet from your DHCP server, you can initiate a `renew` action from the CLI by using a `> request dhcp client management-interface renew` command.

**Important note**

The default username and password for a factory settings appliance or VM are as follows:

`Username: admin`

`Password: admin`

The first time you log on, you will be asked to change this default password. Remember to commit the configuration to save your new password.

## Accessing the interface

Now that you have access to the user interface, you will need to make an important decision right away: whether you will be using **zero-touch provisioning** (**ZTP**) or not (we’ll see a bit more about that later in *Chapter 7*, *Managing Firewalls through Panorama*). In short, ZTP enables a device to automatically receive coordinates for a central management Panorama system, if you have one, and immediately download its configuration from there. By default, this option is enabled on all new devices and is bound to interface `ethernet1/1`. If you do not need this functionality, you must turn it off before you can start using this interface. You can do so by issuing the command `set system ztp disable`, as follows:

```
admin@PA-440> set system ztp disable
Executing this command will disable Zero Touch Provisioning (ZTP), and remove all logs and configuration. The system will restart in standard mode for regular configuration of the firewall. Are you sure you want to continue?  (y or n)
```

If you hit the *Y* key, the system will reboot and ZTP will be disabled. If you change your mind later and want to enable it again, you can do so by issuing the following command: `set system ztp enable`.

Once your system has rebooted, continue with the initial configuration.

If your network does not have a DHCP server, or you connected the firewall directly to your laptop, you will need to set an IP address manually. Copy and paste the following into a text file and alter the `<IP>` entries with the appropriate IP for your management interface, the default gateway it will use to reach out to the internet, and the DNS servers it will use to resolve the domain names. Type the netmask in quad decimals, not in CIDR (slash notation subnet, such as `/16` and `/24`):

```
configure
set deviceconfig system type static
set deviceconfig system ip-address <IP>
set deviceconfig system netmask <x.x.x.x>
set deviceconfig system default-gateway <IP>
set deviceconfig system dns-setting servers primary <IP>
set deviceconfig system dns-setting servers secondary <IP>
commit
```

You can chain `set` commands that belong in the same path and class so that you do not need to set each attribute in individual `set` commands; instead, you can add all the desired settings at once.

In the next example, we go into configuration mode, switch the management interface from DHCP to static configuration, and then combine all the configuration parameters for the management interface into one `set` command. If you haven’t done so already, start by changing the default password to a new one, and then add the interface configuration:

```
admin@PA-440> set password
Enter old password :
Enter new password :
Confirm password   :
Password changed
admin@PA-440> configure
Entering configuration mode
[edit]                                                       
admin@PA-440# set deviceconfig system type static
[edit]                                                                                                            admin@PA-440# set deviceconfig system ip-address 192.168.27.5 netmask 255.255.255.0 default-gateway 192.168.27.1 dns-setting servers primary 1.1.1.1 secondary 1.0.0.1
[edit]                                                                                                            admin@PA-440# commit
Commit job 2 is in progress. Use Ctrl+C to return to command prompt
...........................................55%....75%.....98%.......................100%
Configuration committed successfully
[edit]
admin@PA-440#
```

You may need to log back in after running the `commit` job when the admin password is changed.

**Important note**

The `>` prompt in `username@hostname>` indicates that you are in *operational* mode and can execute runtime commands. The `#` prompt in `username@hostname#` indicates that you are in *configuration* mode and can add configuration parameters.

Operational commands can be run from configuration mode by prefixing `run` to a command—for example, `user@host# run show clock`.

Once the `commit` job finishes, you will be able to connect to the web interface through `https://<IP>` or by using an SSH client, such as PuTTY or the `ssh` command in Linux or macOS.

You are now able to get onto a freshly started firewall and configure it, so we can move on to the next step and gain access to the web interface.

## Connecting to the web interface and CLI

Now that your device has an IP address, you can connect to its web interface via any browser using `https://<IP>`.

You will be met with an unfriendly error message, as in the following screenshots. This is due to the web interface using a self-signed certificate that has not been validated by any authority. For now, this can be safely ignored:

![](Images/B31589_02_04.png)

Figure 2.4: Certificate warnings in Chrome and Firefox

An SSH client will provide you with a slightly friendlier question:

```
tom$ ssh -l admin 192.168.27.115
The authenticity of host '192.168.27.115 (192.168.27.115)' can't be established.
RSA key fingerprint is SHA256:Qmre8VyePwwGlaDmm6JTYtjou42d1i/Ru6xZmmEk8Yc.
Are you sure you want to continue connecting (yes/no)?
```

The SSH connection will provide you with mostly the same user experience as the console connection, but SSH is more responsive and secure, and you can now access your device from anywhere on the management network.

The web interface provides you with a whole new user experience. When prompted for your username and password, input the username `admin` with default password `admin` or the password you created on the terminal, or the username and password you created on the cloud provider.

Once you are logged in, the first screen you will see is the dashboard, which contains some general information about the health of your system, config changes, and which admins are logged on. The dashboard can be customized and additional widgets can be added from a list of prepared widgets, or widgets can be removed if they are not relevant.

For now, the **General Information** widget contains the most important information as you will need the **serial number** of the physical device, or the **CPU ID** and **UUID** on a virtual device, as shown in the following screenshot. The CPU ID and UUID will be needed to register and activate the VM while a physical device can be activated by its serial number:

![](Images/B31589_02_05.png)

Figure 2.5: On the left is a PA-VM device, and on the right is a PA-440 device

Now that we have access to the web interface and are able to collect the system’s base information, we can go ahead and register the firewall and activate any of the feature licenses that were purchased. We will now have a look at how to perform the registration and licensing procedures.

# Adding licenses and setting up dynamic updates

Before we can start adding licenses, the device needs to be registered. You will need to note down the device’s serial number or, if you do not have a support portal account, the sales order number to create a new account. The sales order number will typically be sent to you by your distributor or reseller after you’ve placed your order.

Open a new tab or browser and navigate to [https://support.paloaltonetworks.com](https://support.paloaltonetworks.com). If you do not have an account yet, you will first need to create a new one so you gain access to the portal from where you will be able to manage all your devices, activate your licenses, download software packages and updates, and access support cases. If you already have a **Customer Support Portal (CSP**) account, you can skip to* Registering a new device*.

## Creating a new account

When creating a new account, you will be asked for an email address and whether you want to register using a serial number or an **authorization** (**auth**) code, as in the following screenshot. The serial number is needed when registering a hardware appliance; the auth code is used when registering a VM device:

![](Images/B31589_02_06.png)

Figure 2.6: Serial or authorization code device registration

Alternatively, if you have set up a virtual appliance on one of the cloud providers, you can pick which provider your device is running on (such as Amazon Web Services, Azure, Google Cloud Platform, and so on).

You then need to provide some basic details, such as the address, the password, the device’s serial number, the auth code, and the sales order number or customer ID, if your company already has an account:

![](Images/B31589_02_07.png)

Figure 2.7: General information and device and sales order details

This account creation step will already register your first device; you can go ahead and register more devices in the following section.

## Registering a new device

Ensure you are logged in to your account on the support portal and click on **Register a Device** from the home page:

![](Images/B31589_02_08.png)

Figure 2.8: Register a Device from the support portal home page

You will be presented with the option to register using a serial number or an auth code. The serial number is needed when registering a hardware appliance and the auth code is used when registering a VM device:

![](Images/B31589_02_09.png)

Figure 2.9: Serial or auth code device registration

**Register device using Serial Number or Authorization Code** will ask you for the serial number, a friendly device name, and a tag if you have several “pools” or groups of devices in your account already. It will also request address details as to where the device will be deployed for **return merchandise authorization** (**RMA**) purposes.

If you are deploying a cloud instance, you can choose to register usage-based VM-Series models. You’ll be asked for the serial number, CPUID, and UUID:

![](Images/B31589_02_10.png)

Figure 2.10: Adding a cloud instance to the assets

Now that the devices are registered, it is time to activate the feature and support licenses.

## Activating licenses

Once the device is registered, you can add the licenses. You will have received one (a bundle) or several auth codes that you need to enter on the portal or via the **Device**  |  **Licenses **tab to activate the license and start using the feature on your device. There are multiple ways to activate licenses, which we’ll cover in the following sections, but let’s take a look at the different types of licenses first.

Some of the most common licenses include the following:

*   **Support**: **Platinum 4h** (**PLAT**), **Premium 24/7** (**PREM**), **Standard 9/5** (**STD**), **Partner-enabled 4h support** (**B4HR**), or **Regular Partner-enabled support**  (**BND**). Support licenses grant you access to a support organization, allow you to download software and app ID updates, and entitle you to replacement hardware if your firewall breaks.
*   **Threat Prevention** (**TP**): Antivirus, anti-spyware, threat prevention, daily updates.
*   **Advanced Threat Prevention** (**A-TP**) adds inline cloud analysis for SQL and command injection.
*   **PAN-DB URL filtering** (**URL4**) is the basic cloud-based URL category lookup license. This license will be phased out in favor of ADVURL.
*   **Advanced URL filtering** (**ADVURL**) is the **Machine Learning** (**ML**)-enabled URL filtering license that adds an automated mechanism to analyze unknown URLs in real time.
*   **GlobalProtect** (**GP**) enables mobile applications on Android, iOS, Win10 UWP, Chrome OS, and Linux. It enables **Host Information Profile** (**HIP**) checks and agentless VPNs. It also allows split tunneling based on host application or domain names.
*   **DNS security** (**DNS**): Dynamic DNS signature lookups and blocking or “sinkholing” of malicious DNS requests.
*   **Advanced DNS security** (**A-DNS**): Adds inline cloud analysis for command-and-control lookups.
*   **WildFire** (**WF**): Live threat signature feed, real-time ML-enabled analysis, and cloud-based sandbox analysis.
*   **Advanced WildFire** (**A-WF**): Live threat signature feed, real-time ML-enabled analysis, and cloud-based sandbox analysis.
*   **Decryption port mirroring**: Allows decrypted sessions to be copied to a different device for additional IDS scanning via a dedicated “port mirror” interface.
*   **Internet of Things** (**IOT**): Enables detection of IoT devices and generates rulebase adjustments to protect vulnerable IoT devices.
*   **Data loss prevention** (**DLP**): ML-powered data loss prevention scanning.

More features are being added as Palo Alto Networks announces new products.

### Activating licenses via the customer support portal

In the **CSP**, you can find your registered devices under the **Assets** tab as a device. There’s a pencil icon that allows you to activate auth codes:

![](Images/B31589_02_11.png)

Figure 2.11: The Devices page in the CSP

You will notice there is already a software warranty support license active for a limited amount of time. This is a temporary support license that allows an RMA to be started if your device arrives broken in the box. To add the actual support license and any feature licenses, click on the pencil icon in the **Actions** column you see in *Figure 2**.11*. This will call the pop-over feature activation window:

![](Images/B31589_02_12.png)

Figure 2.12: Adding auth codes to activate services

Once you’ve added all your licenses, the device should look something like this:

![](Images/B31589_02_13.png)

Figure 2.13: A fully licensed device

The little download icons next to each license allow you to download the license key file so that you can upload the key onto the firewall. This is required if you intend to run the firewall without an internet connection and want to be able to upload signature files and enforce security profiles.

Besides activating licenses via the support portal, they can also be activated directly from the firewall interface.

### Activating licenses via the web interface

This procedure requires that the management interface has an internet connection and is able to resolve internet domain names via DNS. If an internet connection is not available, see the previous section on how to download the license keys.

To activate licenses via the web interface, navigate to **Device** | **Licenses**. If you activated the licenses in the CSP and then proceeded to download the license key files, you can click on **Manually upload license key**.

If you activated the licenses on the CSP and want to fetch the licenses, click **Retrieve license keys from license server**. Make sure the firewall has been set up with a functional default gateway and DNS servers.

If you want to activate new licenses with an auth code, click on **Activate feature using authorization code** and you will see a popup where you can enter each auth code individually:

![](Images/B31589_02_14.png)

Figure 2.14: Activating a license using an auth code

With each added license, a section will be added containing the license information:

![](Images/B31589_02_15.png)

Figure 2.15: Active licenses on the device

To activate the support license, you may need to activate the auth key through the **Support** menu item:

![](Images/B31589_02_16.png)

Figure 2.16: Activate support using an authorization code

**Important note**

On hardware firewalls the support license is more like a contract than a license required for a feature to work; a support person will take your call if something goes wrong, a replacement device will be sent if your unit is broken, and so on. This is the only license that does not need to be on the device necessarily before features start to function (i.e., all base functionalities will work without the support license present).

On VM firewalls, the support license is tied to the VM’s activation and the number of CPU cores it supports, so on the VM series, you are required to load the support license.

After all licenses are activated on the device, the next step is to start downloading and scheduling updates to the different databases.

## Downloading and scheduling dynamic updates

Now that all the licenses are active, you can set up dynamic updates and start downloading all the content packages.

Navigate to the **Dynamic Updates** menu under the **Device** tab, where you can manually download content packages and set up schedules and installation preferences. The first time you visit this menu, it may look a bit off as the available content has not been loaded onto the device yet. Click the **Check Now** button to connect to the updates server and fetch the available packages for your system, as shown:

![](Images/B31589_02_17.png)

Figure 2.17: The initial Dynamic Updates view

Once the updates have been fetched, you may still notice that some antivirus packages are missing. This is because the device first needs to be brought up to date with all the app ID and content ID application and decoder updates before further packages can be loaded onto the system. Go ahead and download the latest **Applications and Threats** package:

![](Images/B31589_02_18.png)

Figure 2.18: Downloading the first Applications and Threats package

**Important note**

If no threat prevention license has been activated, there will only be an **Applications** package available for download. This will be indicated by the package name starting with panupv2-all-apps instead of panupv2-all-content and the **FEATURES** column only listing **Apps**.

Once the package has been downloaded, click **Install**. Once the installation has been completed, click **Check Now** again, and the antivirus will become available. Go ahead and download and install the latest package of antivirus updates.

**Important note**

URL filtering and DNS security do not have update packages because URLs are looked up against the cloud service and then stored in the local cache.

You can now start building schedules by clicking on the blue **None (Manual)** option after **Schedule**:

![](Images/B31589_02_19.png)

Figure 2.19: The antivirus and WildFire schedules

The antivirus and WildFire schedules look very similar. They both include the** Recurrence** parameter, which tells the firewall how regularly it needs to check for updates. The options differ for both schedules, though:

*   The update interval options for **Antivirus** are **Weekly**, **Daily**, **Hourly**, or **Manual**.
*   The update interval options for **WildFire** are **Real-time**, **Every minute**, **15 minutes**, **30 minutes**, **1 hour**, or **Never**.

When **Recurrence** is set to any value higher than 1 minute (15 or 30 minutes, or hourly), you can additionally set at which minute within the frame the actual check should take place. (e.g., 13 minutes past the hour, 4 minutes into the quarter hour, etc.) This helps prevent conflicting update connections to the update server in cases where the outgoing internet bandwidth is restricted.

The **Action** can be set to simply **download** or **download-and-install**. If the action is set to **download**, manual installation is required.

**Threshold** is a feature that the antivirus update shares with **Applications and Threats**:

![](Images/B31589_02_20.png)

Figure 2.20: Antivirus and WildFire schedules

**Threshold** is a setting that delays the installation of a content package for a set number of hours. At the time that this threshold expires, the firewall checks for a new update package. If a new package is found, the new package is downloaded and **Threshold** is reset for one more attempt. If yet another update package is found after the first reset, the schedule will reset until the next full occurrence. If no new packages are detected, the package will be installed as defined by **Threshold**.

The threshold delay is a mechanism to prevent installing faulty packages; if the vendor provides poorly crafted content, the delay is set in hours, which should allow other accounts to experience the flaws and report the content issue back to the support teams. So, the content is rolled back via the administrators or the vendor. This thresholding option correlates with a company’s tolerance for the risk of vendor errors and the balance of new and emerging threats to the organization.

**PCNSE tip**

According to Palo Alto Networks’ best practices, a security-first approach is to set the threshold between 6 to 12 hours; however, for a critical environment, the threshold should be 24 hours.

The **Application** content package also has an option to completely **Disable new apps in content update** or enable a separate threshold on the app IDs only **to Allow Extra Time to Review New App-IDs**. The reasoning here is that what is identified as web browsing today may change into a unique application after installing the **Application** content package tomorrow. If the security policy has been set up to only allow previously known applications, this could potentially cause issues with users who suddenly can’t access that specific application.

The **New App-ID Threshold** setting allows you to schedule a review period to see whether any applications need to be accounted for in the security policy before they become active. If no action is needed, the applications will become active automatically. The **Disable new apps in content update** option will not activate any new applications until you manually review and activate all new applications.

**Important note**

At the time of writing, the release schedule for new applications is the third Tuesday of each month. Regular threat package updates happen on Tuesdays, but urgent updates are sent out immediately.

**Dynamic updates cheat sheet**

Here is a quick set of recommendations for scheduling dynamic updates:

1.  Click on **Check Now**.
2.  Download and install the latest `panupv2-all-contents` or `panupv2-all-apps` package: 
    *   `panupv2-all-content` includes all app ID, spyware, and vulnerability updates. This package requires an active Threat Prevention license to be installed successfully.
    *   `panupv2-all-apps` only includes app ID updates and is used when a Threat Prevention license is not active on the device.
3.  Click **Check Now**, which will make the antivirus packages visible.
4.  Download and install the latest `panup-all-antivirus` package.
5.  Set an **Antivirus** update schedule: 
    *   Hourly recurrence
    *   15 minutes after the hour
    *   Download and install
    *   6-hour threshold
6.  If you have a WildFire subscription license, set a **WildFire** update schedule to **Real-time** (firewalls on high-latency internet links can be set to 1-minute or 15-minute update intervals).

1.  Set an **Applications and Threats** update schedule: 
    *   Every 30 minutes
    *   7 minutes past the half-hour
    *   Download and install
    *   6-hour threshold
    *   Leave the **Allow Extra Time to Review New App-IDs** field blank if new app IDs can be added immediately; set to a 48-hour threshold  (or more) if the security team wishes to review new applications before they are activated

The settings in this section are considered best practices as they ensure dynamic updates are scheduled so as not to interfere with other scheduled tasks, like report generation. The threshold ensures an update is not applied until some time has passed by postponing the actual installation and then rechecking the available content package at the threshold time. If a new package is available due to an error or an urgent update, the new package is downloaded and the threshold timer is refreshed. The process repeats once the threshold has been reached again and either the latest package is installed or the update is postponed to the next scheduled occurrence. Not only do content packages need to be updated frequently, but new software versions are made available at regular times to address bugs or introduce new features. Let’s now have a look at the steps needed to upgrade your firewall.

# Upgrading the firewall

In this section, you will learn how to upgrade your firewall and what steps need to be taken to ensure a smooth process. We will review important information to keep in mind when preparing your maintenance window and providing a contingency plan. In-depth upgrade procedures are provided in *Chapter 8*, *Upgrading Firewalls and Panorama*.

## Understanding the partitions

Before we start the upgrade procedure, there’s an important bit of information you need to know. Like most Linux systems, the hard disk has been partitioned into specific segments. These segments serve a specific purpose.

A few important ones are as follows:

*   `/` is the root partition, which is where the operating system is installed
*   `/opt/pancfg` is where the configuration files and dynamic update files are kept
*   `/opt/panrepo` is the repository for downloaded operating system (PAN-OS) images
*   `/opt/panlogs` is the partition where `logdatabase` is stored

There is a hidden `maint` partition that can only be accessed during bootup. This partition is used for disaster recovery, factory resetting the device, and disk issues.

The disk space usage can be viewed with the following command:

```
admin@PA-440> show system disk-space
Filesystem      Size  Used Avail Use% Mounted on
/dev/mmcblk0p2   21G  5.9G   14G  31% /
none            7.8G   64K  7.8G   1% /dev
/dev/mmcblk0p5   32G   13G   18G  42% /opt/pancfg
/dev/mmcblk0p6   18G  9.6G  7.0G  58% /opt/panrepo
tmpfs           7.8G  3.8G  4.1G  48% /dev/shm
cgroup_root     7.8G     0  7.8G   0% /cgroup
/dev/mmcblk0p8   22G   11G  9.6G  53% /opt/panlogs
None
```

The cool thing about the `/` root partition is that it is actually one of two `sysroot` partitions. The system has been partitioned with two operating system-specific partitions, of which just one is mounted at a time. The upgrade procedure installs the new PAN-OS onto the inactive partition.

This allows inline upgrades without interrupting the active partition. Once the new operating system has been installed, the GRUB bootloader is configured to load the other `sysroot` partition at the next boot, causing the new PAN-OS to become active:

```
admin@PA-440> debug swm status
Partition         State             Version
--------------------------------------------------------------------------------
sysroot0          RUNNING-ACTIVE    11.2.2-h1
sysroot1          REVERTABLE        11.2.0
maint             READY             11.2.2-h1
```

This mechanism also allows a smooth rollback in case an upgrade fails and it is decided you need to go back to the previous situation. You can trigger the `> debug swm revert` debug command to tell the bootloader to switch the toggle again to the previous `sysroot` partition and reboot the system via `> request restart system`. After the device has rebooted, you are back on the previous PAN-OS with the pre-upgrade configuration loaded.

## Upgrade considerations

When upgrading, you will need to map out where you are, where you need to go, and how you need to get there. You can find where you are by looking at the dashboard’s **General Information** section and looking for the software version. Deciding where you need to go may require some research and consideration:

*   Which features are required? Are there new features in a release you need, or are you running smoothly with the features you have?
*   Is the code train “mature”? Is a new major release brand new, and do the new features weigh up against the risk of being an early adopter?
*   Are there outstanding advisories that trump the required features? Was a critical vulnerability found that has not been addressed in the version you plan to go to?
*   When is an upgrade required and when is it optional? Is there a direct need to upgrade due to a vulnerability or a bug, or are there features in a newer release that will be helpful?
*   Will your current OS code train reach its end of life soon? (You can review the end-of-life dates here: [https://www.paloaltonetworks.com/services/support/end-of-life-announcements/end-of-life-summary](https://www.paloaltonetworks.com/services/support/end-of-life-announcements/end-of-life-summary)).

These questions will help you determine which upgrades need to take place immediately and which ones can be scheduled ahead of time, possibly with a more relaxed testing process before deploying to the production environment or even postponed until a more mature maintenance release version is available. Let’s now try to answer some of these questions.

#### Which features are required?

Determining which features are contained in each PAN-OS version requires the most research. You can open [https://docs.paloaltonetworks.com](https://docs.paloaltonetworks.com) and search for `Feature Guide`, which will return all the new feature guides for the major PAN-OS versions.

#### Is the code train “mature”?

Maturity can be estimated by looking at the maintenance release version. All PAN-OS versions are made up of three numbers – PAN-OS X.Y.Z (for example, 10.1.5):

*   X is the number of the major software release
*   Y is the number of the feature version release
*   Z is the number of the maintenance release

X will change when a new major software version is released containing new functionality and usually containing some changes in its expected behavior and possibly a new look and feel.

Each new software release is usually followed by a new feature version around 6 to 9 months after its release, mostly containing some new features. Maintenance release versions are released for all code trains anywhere between 5 and 9 weeks and mostly contain bug fixes.

There will occasionally be PAN-OS version names that end in `-hx`, which denotes a hotfix. This is a maintenance release that was published ahead of schedule and usually only contains one or a few critical hotfixes (for example, `11.2.2-h1`).

A code train will usually reach a reliable maturity around the `x.y.4` or `x.y.5` maintenance release version when it is somewhat safe to assume most critical bugs have been found and addressed.

Check the release notes for any known issues so that you can appropriately prepare if there are any caveats: [https://docs.paloaltonetworks.com/pan-os/11-2/pan-os-release-notes.html](https://docs.paloaltonetworks.com/pan-os/11-2/pan-os-release-notes.html).

#### Are there outstanding advisories that trump the required features?

Advisories regarding which maintenance release versions to choose or avoid can be found at [https://securityadvisories.paloaltonetworks.com/](https://securityadvisories.paloaltonetworks.com/) and [https://live.paloaltonetworks.com/t5/Customer-Resources/](https://live.paloaltonetworks.com/t5/Customer-Resources/).

#### When is an upgrade required and when is it optional?

Each major version has a base image, usually the `x.y.0` version, which contains all the vital parts of the PAN-OS image.

This allows the following maintenance versions to be smaller in size, containing only critical updates. The base image needs to be downloaded onto the system before a maintenance version can be installed. It is not required for the base image to be installed in order to be able to install the maintenance version when upgrading from a lower major version. It is also not required to install any intermediate maintenance versions unless the release notes explicitly mention that there is an issue that requires a step in between.

Say, for example, that your firewall is currently on PAN-OS 11.0.4 and you need to get to PAN-OS 11.1.5. You can download a PAN-OS 11.1.0 base image, followed by a PAN-OS 11.1.5 maintenance image, and then directly install and reboot PAN-OS 11.1.5. Your system will be directly upgraded from 11.0.4 to 11.1.5.

If, for example, your firewall is currently on PAN-OS 10.0.10 and you want to go to PAN-OS 11.1.5, you do need to download a PAN-OS 10.1.0 base image and download, install, and reboot the latest maintenance release (e.g., 10.1.14) before you can install PAN-OS 11.1.5.

**Important note**

In PAN-OS 10.1, a new feature was introduced that allows you to skip up to three PAN-OS versions when upgrading. When upgrading PAN-OS 9.1 to 10.1, you are required to install and reboot into PAN-OS 10.0. When upgrading PAN-OS 10.1 to 11.1, you can immediately go to the latest 11.1 maintenance version without needing to install 10.2 or 11.0.

You will now have a good understanding of when and why you would need to upgrade and how to decide which version you need to upgrade to. In the following section, we’ll briefly go over the upgrade process via different methods; see *Chapter 8*, *Upgrading Firewalls and Panorama*, for a more thorough upgrade process.

## Upgrading via the CLI

Using the CLI, commands can be quickly executed to perform tasks. To upgrade via the CLI, you can follow these steps:

1.  You first need to retrieve the available software images that can be installed on your system. You won’t be able to download any images before the list is retrieved: 
    
    ```
    admin@PA-440> request system software check
    Version                          Size    Released on          Downloaded
    -------------------------------------------------------------------------
    11.2.2-h1                        749MB   2024/08/02 15:58:14  no      
    11.2.1                           502MB   2024/07/08 12:04:42  no       
    11.2.0-b3                        1228MB   2024/03/14 12:31:10  no       
    11.2.0-b1                        1258MB   2024/02/06 14:20:41  no       
    11.2.0                           1239MB   2024/05/02 10:15:34  no      
    11.1.4-h1                        1015MB   2024/08/08 02:18:35  yes       
    11.1.4                           946MB   2024/06/27 08:17:35  no       
    11.1.3                           893MB   2024/05/14 15:14:14  no       
    ```
    
2.  Next, you can download the desired PAN-OS version. Remember to first download the base image if you’re upgrading to a new major release: 
    
    ```
    admin@PA-440> request system software download version 11.2.0
    Download job enqueued with jobid 31
    ```
    
3.  You can track the download status with the following command: 
    
    ```
    admin@PA-440> show jobs id 31
    Enqueued            Dequeued ID Type   Status Result Completed
    --------------------------------------------------------------
    2024/08/22 23:24:15 23:24:15 31 Downld FIN    OK     23:25:31 
    Warnings:
    Details:Successfully downloaded
    Preloading into software manager
    Successfully loaded into software manager  
    ```
    
4.  Follow up with the maintenance release: 
    
    ```
    admin@PA-440> request system software download version 11.2.2-h1
    Download job enqueued with jobid 32
    ```
    
5.  When the software is successfully downloaded, you can commence installing it onto the system. You will be prompted that a reboot is required to complete the installation and to confirm whether you are sure that you want to continue. Type `Y` to proceed with the installation: 
    
    ```
    admin@PA-440> request system software install version 12.2.2-h1
    Executing this command will install a new version of software. It will not take effect until system is restarted. Do you want to continue? (y or n)
    Software install job enqueued with jobid 32. Run 'show jobs id 32' to monitor its status. Please reboot the device after the installation is done.
    ```
    
6.  You can track the installation progress through the `show jobs` command: 
    
    ```
    admin@PA-440> show jobs id 32
    Enqueued            Dequeued ID Type   Status Result Completed
    --------------------------------------------------------------
    2024/08/22 23:35:28 23:35:28 32 SWInstall FIN OK     23:38:59 
    Warnings:
    Details:Software installation successfully completed. Please reboot to switch to the new version.
    ```
    
7.  To complete the installation, reboot the firewall. Type `Y` into the dialog if you are certain that you want to go ahead with the reboot. Rebooting will cause all sessions to be interrupted and no new sessions to be accepted until the firewall has completed the `autocommit` job: 
    
    ```
    admin@PA-440> request restart system
    Executing this command will disconnect the current session. Do you want to continue? (y or n)
    ```
    
8.  The `autocommit` job runs right after a reboot and serves to load the configuration onto the data plane. After a software upgrade, this process can take a while: 
    
    ```
    admin@PA-440> show jobs all
    Enqueued            Dequeued ID Type   Status Result Completed
    --------------------------------------------------------------
    2021/12/31 23:44:27 1523:44:27 1  AutoCom FIN   OK      23:44:36 
    ```
    

If you prefer to upgrade the firewall via the web interface, follow the procedure outlined in the next section.

## Upgrading via the web interface

Software images can be downloaded and installed from the **Device** | **Software** menu. The first time you access this page, you will be presented with an error message because no repository has been loaded yet:

![](Images/B31589_02_21.png)

Figure 2.21: Error message on the first visit to the software page

You can ignore this warning; click **Close** and then click **Check Now**. Once the repository has loaded, you will see all the available software images. If you want to install a certain maintenance release version and you want to make sure you meet all the requirements, starting from PAN-OS 10.2, you can click the **Validate** button and the system will check which requirements there may be for this version and whether you have already downloaded them. This will show you the following:

![](Images/B31589_02_22.png)

Figure 2.22: Validate action

Click the **Download** link next to the PAN-OS version you want to upgrade to and wait for the download dialog to complete:

![](Images/B31589_02_23.png)

Figure 2.23: Software management page

Once the new PAN-OS package is downloaded, it will be listed as such on the **Software** page, as shown. Click the **Install** link next to the image to start the installation:

![](Images/B31589_02_24.png)

Figure 2.24: Image downloaded and ready to install

At the end of the installation, you will be prompted to reboot. You can skip the reboot if you want to postpone the actual upgrade to a later time. Otherwise, click **Yes**, as shown:

![](Images/B31589_02_25.png)

Figure 2.25: Post-installation reboot dialog

If you return to the **Software** page after the upgrade, you may notice that not all of the PAN-OS options are listed anymore; this is due to a new feature that can filter all the **Preferred Releases** and **Base Releases**, which are enabled by default. These reflect Palo Alto’s recommended software versions or base images for easier access. Disabling both checkboxes will show all available OS releases again.

![](Images/B31589_02_26.png)

Figure 2.26: Preferred and Base Releases filter

**Upgrade cheat sheet**

This is an upgrade cheat sheet that will help you prepare and plan your upgrade. These steps outline a solid methodology to get to a stable PAN-OS version before placing the firewall in production:

1.  Go to [https://live.paloaltonetworks.com/t5/Customer-Resources/](https://live.paloaltonetworks.com/t5/Customer-Resources/) for release recommendations.
2.  In **Device** | **Software***,* click on **Check Now** to load the latest list of available PAN-OS images.
3.  Download and install the recommended image of your current release.
4.  When the installation is completed, a dialog window will ask if you want to reboot the device. Click **Yes**.
5.  Wait for the unit to boot up again and download the base image for the next major version.
6.  Download and install the recommended maintenance release for the next major version.
7.  When the dialog asks you to reboot the device, click **Yes**.
8.  Repeat steps `5` through `7` until you’re on the version you need to reach or until you reach PAN-OS 10.1, at which time, you can skip ahead up to three OS versions.

Remember that for an HA cluster or Panorama environment, you need to do the following:

*   Disable preemption in the **High Availability** configuration before you start, and re-enable it after the upgrade is completed on both members
*   Check both members for functionality before you start. The upgraded device will become non-functional until the lowest member has caught up (the cluster favors the lowest software member)
*   Upgrade the Panorama centralized management first before upgrading any of your devices

You have now made sure the firewall is fully set up for success by ensuring the content packages are automatically downloaded and installed, and the appropriate PAN-OS firmware has been installed. Next, we will take a look at ensuring the management interface configuration is set up securely.

ardening the management interface

It is paramount that the management interface is kept secure, and access is limited to only those administrators that need access. It is recommended to place the physical management interface in an **Out-of-Band** (**OoB**) network, which limits exposure to the broader network. If access to the management server is needed from a different network, it is best to set up a dual-homes bastion host that mediates the connection, either by only allowing admins to log in to it and use services from there, or having it set as a (transparent) proxy with a log of all sessions and limiting the source users and IP subnets as much as possible. Admin accounts also need to be set so they only have access to the sections of the configuration they need to access and use external authentication. Let’s look at these different scenarios in detail.

## Limiting access via an access list

The management interface local access list can be edited by navigating to **Device** | **Setup** | **Interfaces** and clicking on the **Management Interface**:

![](Images/B31589_02_27.png)

Figure 2.27: Management Interface access list

The associated CLI `configure` mode command is as follows:

```
admin@PA-440>configure
admin@PA-440#set deviceconfig system permitted-ip 192.168.0.0/24 description "mgmt subnet"
```

You can also attach an interface management profile (shown in the following screenshot) to an interface, which enables the selected services (SSH and HTTPS, usually) on the IP address of the assigned data plane interface. This is not recommended as it introduces significant risk if not implemented properly:

![](Images/B31589_02_28.png)

Figure 2.28: Interface Management Profile

If you must enable this profile, make sure it is on a sufficiently shielded interface, preferably a loopback interface, that has security policies associated with gaining access to the management services. As a secondary measure, also enable an access list on the profile.

These profiles can be configured in **Network** | **Network Profiles** | **Interface Management Profile** and then attached to an interface in **Network** | **Interfaces** under the **Advanced** tab of the selected interface:

![](Images/B31589_02_29.png)

Figure 2.29: An interface management profile attached to an interface

The CLI command to create an interface management profile, set its services to HTTPS and SSH, and add an Access Control List (ACL) is as follows:

```
# set network profiles interface-management-profile mgmt https yes ssh yes permitted-ip 10.15.15.37
```

The subsequent ACL items can be set via the following command:

```
# set network profiles interface-management-profile mgmt permitted-ip 192.168.0.5
```

As a best practice, never configure a management profile on an internet-facing interface.

Now that access to the management interface has been set, let’s look at access from the management interface.

## Accessing internet resources from offline management

If the management interface does not have access to the internet, this can create interesting challenges as it will not be able to retrieve updates or perform cloud lookups. A workaround is to enable service routes that route specific applications, services, or protocols via the backplane onto a designated data plane interface, allowing the management plane to reach out to the internet while its physical interface does not have access outside of its management LAN.

Service routes can be configured from the **Device** | **Setup** | **Services** menu, where you can click on **Service Route Configuration** to get the following dialog:

![](Images/B31589_02_30.png)

Figure 2.30: Service Route Configuration

Here’s a quick overview of what the predefined service routes are used for:

*   **AutoFocus**: A direct connection to the Autofocus subscription service so you can consume it directly from the firewall web interface
*   **CRL Status**: Fetching **Certificate Revocation List** (**CRL**) information when processing certificates (e.g., during TLS handshakes)
*   **Data Services**: Used for data uplinks from the data plane to Palo Alto cloud services, used for IoT, DLP, and SaaS subscription services
*   **DDNS**: Updates the interface IP with the **Dynamic Domain Name System** (**DDNS**)
*   **Panorama pushed updates**: Content and software updates deployed from Panorama (these are triggered from Panorama but initiated from the firewall)
*   **DNS**: System DNS lookups
*   **External Dynamic Lists**: Fetching updates for configured **external dynamic lists** (**EDLs**)
*   **Email**: Email sent out by the system
*   **Gp IP Mgmt**: External DHCP for GlobalProtect IP assignment
*   **HSM**: **Hardware Security Module** is a remote system that maintains the private key for the certificate used for SSL/TLS decryption
*   **HTTP**: System HTTP forwarding
*   **Kerberos**: Kerberos authentication
*   **LDAP**: **Lightweight Directory Access Protocol** connections
*   **MDM**: **Mobile Device Manager** connections
*   **Multi-Factor Authentication**: **MFA** authentication server
*   **NetFlow**: NetFlow collector for network statistics
*   **NTP**: **Network Time Protocol** updates
*   **Palo Alto Networks Services**: Updates from Palo Alto Networks and WildFire sandbox connections
*   **Panorama**: Connection to the Panorama management server
*   **Panorama Log Forwarding**: Logs sent to a log collector (used by the PA\_5200 chassis only)
*   **Proxy**: Connections to the system proxy
*   **RADIUS**: **Remote Authentication Dial-in User Service** connections
*   **SCEP**: **Simple Certificate Enrollment Protocol** connections for requesting and distributing client certificates
*   **SNMP Trap**: Sending SNMP statistics to an SNMP server
*   **Syslog**: Syslog messaging server
*   **TACACS+**: **Terminal Access Controller Access Control System Plus** authentication
*   **UID Agent**: User ID agent connections
*   **URL Updates**: URL filtering live lookup service
*   **VM Monitor**: Virtual machine monitoring
*   **WildFire Private**: Connection to the private WildFire server
*   **Ztp**: Connection to the ZTP service

Once you set the radio button from **Use Management Interface for all** to **Customize**, you will be able to select which source interface will be used for each service. From the **Destination** tab, you can also add specific IP addresses or entire subnets that need to be routed out of a specific interface. The routing table used by the target interface will determine how the session is routed to the destination, so service routes can only be attached to OSI Layer 3 interfaces.

The associated CLI configuration command to set a service route is as follows:

```
#set deviceconfig system route service dns source interface ethernet1/8 address 192.168.0.15
```

If you want to see a full list of all the available services, hit the `Tab` key after typing `service`:

```
#set deviceconfig system route service <Tab>
  autofocus                    AutoFocus Cloud
  crl-status                   CRL servers
  ddns                         DDNS server(s)
  ...
```

This will enable access to resources that are normally not accessible through the management network. In the next section, we’ll prepare administrator accounts and provide access as needed.

## Admin accounts

The “admin” account is probably one of the most abused accounts in internet history, so your next task is to get rid of it and replace it with named accounts. Instead of the default “admin” account, it is best practice to use named accounts so changes can be tracked by the user and personalized access can be granted easily.

Navigate to the **Device** | **Administrators **tab to view local administrators. When creating new administrator accounts there are two types of accounts available, **Dynamic** and **Role Based**, which you can select by setting the **Administrator Type** toggle:

![](Images/B31589_02_31.png)

Figure 2.31: Creating a new admin account

First, we’ll take a look at dynamic account profiles and their benefits.

### Dynamic accounts

Dynamic accounts may be of the following types:

*   **Superusers** can do everything. They are the only users that are allowed to perform certain tasks like creating other administrator accounts or creating a tech support file.
*   **Device administrators** can do everything besides create new users or virtual systems.
*   Virtual system-capable devices also have **virtual system administrators**, who are also device administrators and are restricted to one or several specific virtual systems.
*   There are also read-only flavors that can view everything but not make changes.

Your first account will need to be a new superuser to replace the default admin account.

### Role-based administrators

Once all the required superusers and device administrators are created, additional role-based administrators can be added for teams that only require limited functionality. Role-based administrators can be customized down to individual menu items so that they can do anything or have read-only or no access.

The roles can be configured through the **Device** | **Admin Roles** menu:

![](Images/B31589_02_32.png)

Figure 2.32: Admin Role Profile

Set each topic to one of the following options by clicking the icon to cycle to the option you need:

*   A red cross indicates that these administrators will not see the menu item
*   A lock indicates that the admin will be able to see objects or menu items, but not make any changes
*   A green checkmark indicates that the admin has full access to this menu item and can make changes to objects or configurations within it

In the **XML API**/**REST API** tabs, each role can be granted or denied access to certain API calls:

![](Images/B31589_02_33.png)

Figure 2.33: XML API/REST API

In the **Command Line** tab, each role can be granted a certain level of access or denied access altogether:

![](Images/B31589_02_34.png)

Figure 2.34: The Command Line permissions

Now that we can set up administrator accounts, we should also create a password security profile to prevent weak password discipline.

### Password security

You will need to add a password profile by going to **Device** | **Password Profiles** to ensure that the password is changed on a regular basis:

![](Images/B31589_02_35.png)

Figure 2.35: Password Profiles

These are the configurable settings in **Password Profiles**:

*   The change period indicates how long a password is valid
*   The expiration warning pops up a warning when an admin logs on if their password is about to expire
*   The post-expiration login feature allows the admin to log on a certain number of times, even after their password has expired
*   The post-expiration grace period indicates how long an admin will be able to log on after their account has expired before it is locked permanently and will require intervention from a different admin

Additionally, you should enforce a minimum password complexity for local accounts to ensure no weak passwords are used by administrators via **Device** | **Setup** | **Management** | **Minimum Password Complexity**:

![](Images/B31589_02_36.png)

Figure 2.36: Minimum Password Complexity

NIST has an extensive guideline on authentication and life cycle management that can be found at [https://pages.nist.gov/800-63-3/sp800-63b.html](https://pages.nist.gov/800-63-3/sp800-63b.html).

Let’s now look at the external authentication factors.

## External authentication

It is best to use external authentication factors, such as Kerberos, LDAP, RADIUS, or SAML, to keep control over credentials in a centralized system, which enables admins to only change passwords once for multiple devices or to be locked out of all critical infrastructure at once if they leave the organization.

It is a good safeguard to keep one “break-glass” local account in case the management interface loses all access to the external authentication servers. This account should only be used in case of emergency and an alert should be sent when the account is used to log on. See *Chapter 9*, *Logging and Reporting*, on how to set up a profile that can send out an alert if the account is used.

You first need to create a server profile from the **Device** | **Server Profiles** menu. Each server type has its own configuration parameters. The following profiles are available:

*   **TACACS+** (**Terminal Access Control Access Control System Plus**) is an authentication protocol developed by Cisco that is still used in many environments for terminal access authentication.
*   **LDAP** (**Lightweight Directory Access Protocol**) is probably one of the most commonly available protocols to authenticate against directory services and will work with Microsoft Active Directory, eDirectory, and custom LDAP servers.
*   **RADIUS** (**Remote Authentication Dial-in User Service**) is an open standard authentication protocol that is widely used in remote access authentication.
*   **Kerberos** is an authentication protocol that is mostly used for single sign-on and relies on a negotiation that does not require the exchange of passwords.
*   **SAML** (**Security Assertion Markup Language**) uses an XML framework to exchange security information and is mostly used with cloud-based **identity providers** (**IdPs**). It integrates with **multi-factor authentication** (**MFA**) very easily.
*   Multi-factor profiles allow for several built-in MFA providers to be configured and added separately to an authentication profile.

Let’s take a look at each of these profiles and how to set their basic configuration.

### The TACACS+ server profile

TACACS+ requires you to choose between **Password Authentication Protocol** (**PAP**) and **Challenge-Handshake Authentication Protocol** (**CHAP**) and set the secret associated with connecting to the TACACS+ authentication server.

Optionally, you can set the profile so that it can only be used for administrator authentication, and set the profile to use a single session for all authentication events, rather than a new session per authentication event:

![](Images/B31589_02_37.png)

Figure 2.37: TACACS+ Server Profile

While TACACS+ is somewhat rare, LDAP authentication is very common.

### The LDAP server profile

For an LDAP profile, you need to provide the type of the LDAP server, which can be **active-directory**, **E-directory**, **sun**, or **other**.

You need to provide a **Base DN** value, which is the domain name (or the distinguished name) of the LDAP tree. The **Bind DN** field is for the user account that will be used to connect to the LDAP server and perform the request and its password. **Bind DN** can be fully qualified, as shown in the following screenshot, or be a **user principal name** (**UPN**) formatted as `user@domain`:

![](Images/B31589_02_38.png)

Figure 2.38: LDAP Server Profile

One thing to remember is that when you configure the server IPs and you have **Require SSL/TLS secured connection** enabled, the default port for LDAPS is `636`, rather than `389`.

If your LDAP server has an externally signed certificate, enable **Verify Server Certificate for SSL sessions** to ensure the authenticity of your server. For the certificate check to work, the LDAP server root and intermediary certificates need to be in the device certificate store in **Device** | **Certificate Management** | **Certificates** | **Device Certificates**. The server name in the profile must match the **fully qualified domain name** (**FQDN**) certificate and `Subject AltName` attribute for this check to pass.

### The RADIUS server profile

RADIUS is one of the most popular authentication methods and supports the following authentication protocols:

*   PEAP-MSCHAPv2: **Protected Extensible Authentication Protocol** (**PEAP**) with Microsoft CHAP v2 provides improved security over PAP or CHAP by transmitting both the username and password in an encrypted tunnel.
*   PEAP with **Generic Token Card** (**GTC**): PEAP with GTC enables the use of one-time tokens in an encrypted tunnel.
*   EAP-TTLS with PAP: **Extensible Authentication Protocol** (**EAP**) with **Tunneled Transport Layer Security** (**TTLS**) and PAP is used to transport plain text credentials for PAP in an encrypted tunnel. EAP-TTLS uses certificates to secure the connection and should be the preferred protocol as it is the most secure.
*   CHAP: Used if the RADIUS server does not support EAP or PAP or is not configured for it.
*   PAP: Used if the RADIUS server does not support EAP or CHAP or is not configured for it.

Palo Alto Networks uses vendor code `25461`.

Like the other profiles, RADIUS can be set so that it is only used for administrator authentication. The **Allow users to change passwords after expiry** option is limited to GlobalProtect users if the profile is also used to authenticate GlobalProtect inbound connections.

The **Make Outer Identity Anonymous** option ensures the admin username is not visible for anyone sniffing the authentication sessions if PEAP-MSCHAPv2, PEAP with GTC, or EAP-TTLS are used and the server supports this:

![](Images/B31589_02_39.png)

Figure 2.39: RADIUS Server Profile

The certificate verification for RADIUS server profiles requires a certificate profile that allows more checks to be performed than just having the root certificate in the trusted certificate store compared to TACACS+. Several mechanisms can be used to verify server validity and actions can be taken if particular conditions are met with the certificate check, such as opting to allow or block a session if the certificate is valid but has expired:

![](Images/B31589_02_40.png)

Figure 2.40: Certificate Profile

### The Kerberos server profile

The Kerberos server profile is very simple to configure as it only requires an IP or FQDN and a port number, but it does require a few specific configuration settings:

*   The firewall needs to have the domain set in **Device** | **Setup** | **Management** | **General Settings**
*   The firewall is synced to a **Network Time Protocol** (**NTP**) server from **Device** | **Setup** | **Services** so that its clock is in sync with the local `ActiveDirectory` server
*   Its DNS servers need to be set to internal DNS servers that are joined to the domain, rather than external DNS servers

This is shown in the following screenshot:

![](Images/B31589_02_41.png)

Figure 2.41: Kerberos requirements

With single sign-on adoption on the rise and many authentication services making a move to the cloud, the popularity of SAML authentication is also increasing.

### The SAML server profile

The SAML profile enables authentication against an external **single sign-on** (**SSO**) provider (such as PingID and Okta).

Your IdP should provide you with the following:

*   An identifier so that it can certify whether the authentication session originates from you
*   The root and intermediary certificates, which you can load to **Device** | **Certificate Management** | **Device Certificates** to verify the identity of the SSO and **single log-out** (**SLO**) sites
*   An SSO URL
*   An SLO URL

SAML HTTP binding provides an option to **post**, which sends a Base64-encoded HTML form to the IdP, or **redirect**, in which case the firewall will send Base64-encoded and URL-encoded SSO messages within the URL parameters.

You can sign SAML messages to the IdP with a certificate for signing requests, which can be configured in the authentication profile:

![](Images/B31589_02_42.png)

Figure 2.42: SAML Identity Provider Server Profile

### The MFA profile

It is highly recommended to add MFA as regular passwords require good hygiene from each administrator and could be inadvertently shared, stolen, or guessed. If your regular authentication does not support integrating an additional factor like SMS, push messages, tokens, etc., you can add an MFA profile and add the extra profile to the authentication profile.

Currently, four MFA providers are available as standalone profiles: Duo, Okta, RSA, and PingID. To configure this profile, you will need some parameters from the provider.

Settings such as the API host may depend on your geolocation, and keys and secrets will be unique identifiers to your account:

![](Images/B31589_02_43.png)

Figure 2.43: Multi Factor Authentication Server Profile

Once the appropriate server profiles have been set up, they need to be added to an authentication profile.

### Setting up the authentication profile

Now that the appropriate server profile has been configured for your environment, you can go ahead and set up an authentication profile, which will set the stage for the administrators to sign in. Go to **Device** | **Authentication Profile** and create a new authentication profile.

The **Authentication** tab lets you choose the type of authentication you want to use for this profile; this will match the server profile you configured in one of the previous steps. You can then add additional parameters, such as setting `sAMAccountName` or `userPrincipalName` for the LDAP login attribute. **Username Modifier** lets you control how the username that the end user enters is translated and sent to the authentication server. This allows you to simply forward what the user inputs or add the user domain in UPN format (`user@domain`) or traditional `domain\user` backslash format:

*   `%USERINPUT%`
*   `%USERDOMAIN%\%USERINPUT%`
*   `%USERINPUT%@%USERDOMAIN%`

This may be necessary in a multi-domain forest environment:

![](Images/B31589_02_44.png)

Figure 2.44: Authentication Profile LDAP example

In the **Factors** tab, you can add a profile for an MFA policy that will trigger the secondary authentication once a user logs in:

![](Images/B31589_02_45.png)

Figure 2.45: Authentication Profile MFA

The **Advanced** tab creates a bit of a chicken-and-egg situation as it requires you to tell the firewall which usernames or user groups are allowed to attempt authentication, but the list of users is only populated after you have properly set up the user ID. If you have not set up a user ID group mapping yet, set the user to **all** until you can return and narrow down the list to the actual admin user groups or usernames.

For security purposes, you should configure a lockout policy that prevents logins for an amount of time after several failed attempts to log in:

![](Images/B31589_02_46.png)

Figure 2.46: Authentication Profile allowed users

When the profile is created, you can use it instead of a static password when creating administrator accounts. The Authentication Profile can be set in the administrator account to replace the static password:

![](Images/B31589_02_47.png)

Figure 2.47: Admin account with an authentication profile

With the topics we covered in this last section, you are now able to set up admin accounts that are not only restricted to the access they need (complying with RBAC requirements) but can also leverage external authentication mechanisms and add MFA to strengthen administrator access and prevent unauthorized access. In the next section, we will learn about the different types of interfaces.

# Understanding the interface types

When you open the **Network** | **Interfaces** menu, you will see an assortment of physical interfaces. There are several different interface types that will cause an interface to behave in a specific way. We will first cover the four basic interface types and continue with the more specialist ones after:

*   Virtual Wire (VWire) is a “Layer 1” passthrough set of two interfaces
*   Layer 3 is routed interfaces
*   Layer 2 is “switched” interfaces that rely on VLAN tags rather than routing tables
*   Tap is a “sniffing” interface

Let’s discuss them in more detail.

## VWire

Just as the name suggests, VWire is intended to be a “bump in the wire.” VWire always consists of two physical interfaces—no more and no less. There is no low-level interference with VLAN tags and there are no routing options; packets are inspected in flow.

Using a VWire interface can be an easy way to “drop in a firewall” without needing to interfere with an existing routing or switching environment. It easily plugs in in front of an ISP router or can be placed in between a honeypot and the network to add a layer of detection.

Out of the box, a firewall will have a default VWire set up on `ethernet1/1` as the outside interface and `ethernet1/2` as the inside interface with a security rule to allow all outbound traffic.

Before you can create a VWire interface, you first need to set two interfaces to the **Virtual Wire** type and assign each of them to a different zone:

![](Images/B31589_02_48.png)

Figure 2.48: VWire interface

You can now connect both interfaces to a VWire profile by going to **Network** | **Virtual Wires** and creating a new VWire profile.

As illustrated in the following screenshot, you will need to select the two interfaces with which you will form a VWire connection. If the VWire interface is placed over a trunked link (one that contains the `VLAN/802.1Q` tags), you need to indicate which ones are allowed. If you want to allow all tags, set `0-4094`. If you want to add single tags or ranges, you can add integers or ranges, separated by commas (for example, `5,15,30-70,100-110,4000`). **Multicast firewalling** needs to be checked if you want to be able to block or otherwise apply security policies to multicast traffic. If unchecked, multicast is forwarded across VWire without checking the security rules. Enabling **Link State Pass Through** brings the opposite interface down if one side loses its connection. 

This ensures that both the client and server sides see the link go down and respond accordingly:

![](Images/B31589_02_49.png)

Figure 2.49: VWire configuration

## The Layer 3 interface

A Layer 3 interface is a routed interface. This means it has an IP address and can be used as a default gateway for clients on the inside connected to it or a next hop for a routing device. On the outside, it can communicate with ISP routers and forward traffic out to the internet. We’ll first look at the interface.

### Exploring the interface

In the **Config** tab of the interface, you need to assign a **Virtual Router** (**VR**) and a security zone (we’ll learn more about the VR later in this chapter, so don’t worry about it just yet). This zone will represent the subnet(s) connected to it when traffic needs to flow from one interface to another:

![](Images/B31589_02_50.png)

Figure 2.50: Layer 3 interface configuration

The IP configuration can be statically configured as an IP/subnet. If needed, multiple IP/subnets can be added to represent additional networks that are directly connected to the interface:

![](Images/B31589_02_51.png)

Figure 2.51: Layer 3 interface IP

A Layer 3 interface can also be set as a **Point-to-Point Protocol over Ethernet** (**PPPoE**) client if the upstream connection is provided by a broadband ISP over cable or DSL. In this case, in the **General** tab, the ISP authentication username and password can be configured:

![](Images/B31589_02_52.png)

Figure 2.52: Layer 3 PPPoE

In the **PPPoE** | **Advanced** tab, you set the authentication protocol to **PAP**, **CHAP**, **auto**, or **none**. If the ISP has provided you with a static IP, you can configure it here and you can add an access concentrator and service string if the ISP requires them to be able to connect. If required, you can disable adding the default route received by the ISP to the routing table. Some ISPs require PPPoE clients to be in a passive state as they initiate the connection. You can enable this here:

![](Images/B31589_02_53.png)

Figure 2.53: Layer 3 PPPoE Advanced options

Once you’ve configured the interface, click **OK** and commit the change. Once the commit has been completed, you can click on **Show PPPoE Client Runtime Info** from the **Network** | **Interfaces** | **Ethernet** page to receive information on the connection. From the CLI, you can issue the following command to see the same output:

```
admin@PA-440> show pppoe interface <interface>
```

The **DHCP Client** tab allows you to configure the interface as a DHCP client, receiving its IP information from an upstream DHCP server:

![](Images/B31589_02_54.png)

Figure 2.54: Layer 3 DHCP Client advanced options

You can choose to automatically accept the default route or disable this functionality and manually define the default route in the VR. If needed by the ISP, you can send the system or custom hostname, and you can determine the metric for the default route.

In the **Layer3** | **Advanced** tab, we gain access to several advanced features:

![](Images/B31589_02_55.png)

Figure 2.55: Layer 3 advanced options

Here we can set the link speed, duplex, and state of the interface. In the **Other Info** tab, we can add the management profile we saw earlier, change the **maximum transmission units** (**MTUs**), and enable **Adjust TCP MSS** with custom values to change the MSS header value in TCP packets flowing through the firewall.

In the other tabs available, we can also set static IPV4 ARP Entries, which can be useful in case an upstream device requires **a static ARP entry**, IPV6 **Neighbor Discovery** (**ND**) **Entries**, or **Neighbor Discovery Protocol** (**NDP**) **Proxy**. A **Link Layer Discovery Protocol** (**LLDP**) profile can also be added to interface with switches.

Lastly, if the external interface has a dynamic IP assigned to it by the ISP, a DDNS hostname can be set up. Currently, four vendors are supported: DuckDNS, afraid.org, no-IP, and Palo Alto:

![](Images/B31589_02_56.png)

Figure 2.56: DDNS configuration

For the Layer 3 subnets and IP addresses to be reachable across interfaces, they need to be added to a routing table; this is accomplished in the VR.

### VR

A VR is the routing element of the firewall, but, as the name suggests, it is not made up of a single engine but rather, a routing set that an interface is subscribed to. Each Layer 3, loopback, and VLAN interface needs to be associated with a VR, but multiple VRs can be used on a system. Not all interfaces need to be associated with the same VR.

You can configure the default VR or add new VRs from the **Network** | **Virtual Routers** menu. In the **Router Settings** tab of a VR, you can see and add interfaces associated with this VR and adjust the administrative distances if needed. An administrative distance associates a priority with a routing protocol. By default, static routes have a higher priority (lower administrative distance) than **Open Shortest Path First **(**OSPF**), but you can change this priority if you want OSPF routes to have priority and only use static routes if OSPF becomes unavailable. Routes within the same routing protocol can be assigned a metric to give them a higher (lower metric) or lower (higher metric) priority. Routes inside the same type (static, OSPF, etc.) are first prioritized based on the size of their subnet. A smaller subnet (for example, `/32`) will have priority over a larger subnet (for example, `/16`). Next, the metric will be used to determine priority for networks with the same subnet mask with the lower metric having priority (for example, `10.0.0.0/24 metric 10` will have priority over `10.0.0.0/24 metric 20`):

![](Images/B31589_02_57.png)

Figure 2.57: VR settings

In the **Static Routes** tab, you can add destination routes as needed. By default, the firewall loads all the connected (configured on a Layer 3, loopback, or VLAN interface) networks in the routing table; adding static routes makes remote networks available from a routing perspective.

One of the first routes you may need to configure is the “default route,” which allows clients to connect to the internet. If a Layer 3 interface is configured as a PPPoE or DHCP Client, an **Automatically Create Default Route** option can be enabled, which will automatically add the default route based on the upstream ISP.

The **Destination** for the default route is `0.0.0.0/0`. A regular route could have a smaller subnet, such as `172.16.0.0/24`.

The **Interface** option indicates what the egress interface will be. If the route is pointing to the internet, the interface will be the one where the ISP router is connected.

**Next Hop** has several options:

*   **IP Address**: The IP of the upstream router to forward packets to.
*   **Next VR**: Whether the packet needs to be handed over to a different VR on the same device.
*   **FQDN**: If the upstream router has a dynamic IP, it could be useful to use an FQDN that is dynamically updated by a DNS record.
*   **Discard**: Routes can be set to “black hole” certain subnets. This can be used to prevent any packets from reaching a connected out-of-band network, even if a security policy were to allow this.
*   **None**: Routes may not have a next hop, such as packets routed into a VPN tunnel.

The **Admin Distance** and **Metric** settings can be changed for each route if necessary.

**Route Table** is used to add routes to regular unicast routing, multicast routing, or both.

You can, if you have redundancy available, use **Path Monitoring** to send a heartbeat ping over the route. If the ping fails a configured number of times, the route will be disabled. The routing table will be re-evaluated for matching packets and the next best match will be used to route packets (that is, a route with a higher metric or larger subnet):

![](Images/B31589_02_58.png)

Figure 2.58: VR default route

Any subnets that are configured on a Layer 3 interface are added to the routing table as a connected network and do not need a static route to be added.

## The Layer 2 interface and VLANs

Setting interfaces to the Layer 2 type enables the firewall to function in a similar way to placing a switch in the network. Each interface acts as the equivalent of an access port (if you need trunk functionality, refer to the *Subinterfaces* topic) on a switch, and you can add as many interfaces as you need.

Each interface should use a different zone so that a security policy can be leveraged to control traffic between the interfaces. Interfaces set to the same zone will, by default, exchange traffic without inspection and require a catch-all security policy to enable inspection.

To group the interfaces into a logical “switch,” you need to create a VLAN object by going to **Networks** | **VLANs** and adding the interfaces you previously set to Layer 2 and want to be connected:

![](Images/B31589_02_59.png)

Figure 2.59: VLAN group

The **VLAN Interface** option adds routing functionality to the group as a logical Layer 3 interface. This can be useful if you have an upstream ISP router or a different subnet connected to a Layer 3 interface that you need to interact with.

You can configure the VLAN interface by going to **Network** | **Interfaces** | **VLAN**. Assign it to the VLAN group you created, fill in the **Virtual Router** field, and assign it a zone. This zone will represent Layer 2 interfaces when interacting with Layer 3 interfaces for security policies:

![](Images/B31589_02_60.png)

Figure 2.60: VLAN Interface configuration

You will also need to assign the VLAN interface an IP address that the clients on Layer 2 interfaces can use as a default gateway or routing next hop. Make sure it is in the same subnet as your clients on the Layer 2 interfaces:

![](Images/B31589_02_61.png)

Figure 2.61: VLAN Interface IP address

## Tap interfaces

Tap interfaces can be used as a passive sniffing port. If a different network device is set up with port mirroring, its egress port can be connected to the tap interface to intercept all packets and apply the app ID and content ID. As long as the Tap interface is sent all packets of a session, it will be able to inspect the traffic as if it is flowing through the firewall. There are, however, a few limitations:

*   As the firewall is not actively participating in the processing of packets, it cannot take action if it detects a threat; it can only report it.
*   SSL decryption can only be applied to inbound connections if the server certificate can be loaded onto the firewall with its private key.

The Tap interface only needs to be configured with a security zone:

![](Images/B31589_02_62.png)

Figure 2.62: The Tap interface

To optimally benefit from the Tap functionality, a security rule will need to be created that allows all operations, or a specific subset, if you want to limit the scope. The firewall will discard all packets in the background, but setting the security rule to drop would discard the packets before inspection:

![](Images/B31589_02_63.png)

Figure 2.63: The Tap security rule

Similar to listening in on a port mirror, the firewall can send all unencrypted session data to a third-party **DLP** or threat intelligence device. It can do so via a Decryption Port Mirror interface.

## The Decryption Port Mirror interface

The Decryption Port Mirror interface allows the forwarding of decrypted packets to an external device for further inspection: the firewall will receive a TLS packet that is set to be decrypted, so takes all the steps to decrypt the session, and then forwards the clear text payload to a third party device for a “second opinion” using the Decryption Port Mirror interface as an egress interface. This can be useful for DLP, for example. The license can be activated for free via the support portal by browsing [https://support.paloaltonetworks.com](https://support.paloaltonetworks.com) and then going to **Assets** | **Devices**.

There, you can find your firewall and click the **Actions** button. If you choose to activate a feature license, you will be able to activate **Decryption Port Mirror**:

![](Images/B31589_02_64.png)

Figure 2.64: Activating a Decryption Port Mirror license

To activate the license on the firewall, follow these steps:

1.  From **Device** | **Licenses**, select **Retrieve license keys from license server**.
2.  In **Device** | **Setup** | **Content ID** | **Content-ID settings**, enable **Allow forwarding of decrypted content**.
3.  In **Network** | **Interfaces** | **Ethernet**, set an interface to the **Decrypt Mirror** type.
4.  In **Objects** | **Decryption** | **Decryption Profiles**, open the decryption profile and add the interface to **Decryption Mirroring**.
5.  In **Policies** | **Decryption**, create decryption rules that use the decryption profile.
6.  Save the changes and connect the Decryption Port Mirror interface.

Besides Ethernet interfaces, there are also three different logical interfaces:

*   Loopback
*   Tunnel
*   VLAN interface

Let’s now take a look at the Swiss army knife of interfaces, the loopback.

## The loopback interface

A loopback interface is a logical Layer 3 interface that can serve many purposes. One common use case includes adding an additional public IP to its own interface so VPN configuration can be added to it. Another use case is to add a management profile to a loopback, and then leverage security rules to allow administrators to manage the firewall from exotic networks. It needs to be configured with an IP address (only a single IP per loopback interface is supported) and a security zone and it needs to be associated with a VR.

It can be set to a new IP address in the same subnet and zone as one of the Layer 3 interfaces, so services such as **Management Profile**, **Captive Portal**, and **GlobalProtect** can be hosted on a different IP than the main IP of the physical interface.

To add extra security, it can also be set to a different zone so that a matching security rule is needed for clients to be able to connect to the loopback interface:

![](Images/B31589_02_65.png)

Figure 2.65: Loopback Interface

The number next to **Interface Name** is an identification number for the logical interface. A number from 1 to 9999 can be assigned to the interface and will only serve as an identification number for the interface.

## The tunnel interface

Tunnel interfaces are logical interfaces that serve as the ingress and egress point of tunneled traffic, both site-to-site VPN and GlobalProtect SSL and IPSec. The physical tunnel is terminated on a Layer 3 or loopback interface, but the packets that need to be encrypted should be routed to the tunnel interface:

![](Images/B31589_02_66.png)

Figure 2.66: Static route for a VPN tunnel

This interface needs to be associated with a VR and a security zone, as you can see in the following screenshot:

![](Images/B31589_02_67.png)

Figure 2.67: Tunnel Interface

The number next to **Interface Name** is an identification number for the logical interface.

**Important note**

For a strong security posture, set a separate zone for individual VPN connections, even for known locations. Treating each connection and remote network as an individual zone ensures adequate visibility and control. A remote office could be exposed to malware (think WannaCry) and infect other offices if the VPN tunnel is set to the same zone for all remote offices. The default `intrazone` security rule allows all sessions to run and does not apply scanning.

There are also several “special” interface types that provide a specific functionality; we’ll cover the special use case interfaces in the following sections.

When a switch uplink needs to contain multiple 802.1q VLAN tags, it can be configured as a trunk, and, on the firewall, subinterfaces can be created to correspond to each VLAN tag.

## Subinterfaces

All physical (that is, Layer 2, Layer 3, VWire, and Aggregate—we’ll cover this type of interface a little further in this chapter) interfaces can have subinterfaces. A subinterface is used when the physical interface is connected to a trunked link containing VLAN (802.1Q) tagged packets. The physical interface is not able to interpret the tags, but subinterfaces are. For each VLAN carried by the trunk, you can create a subinterface to represent the virtual network coming from the switch. The advantage of using subinterfaces is that each VLAN can be associated with its own security zone.

The subinterface will mimic all the configuration specifics of its parent physical interface, but interface types cannot be different from the physical interface type (for example, a Layer 3 physical interface cannot host a Layer 2 subinterface).

You can create these by selecting the desired physical interface and clicking on **Add Subinterface** at the bottom left of **Network | Interfaces**:

![](Images/B31589_02_68.png)

Figure 2.68: Creating a subinterface

## HA interfaces

**High Availability** (**HA**) interfaces are required when setting up a cluster of two firewalls. Some chassis will have built-in dedicated HA interfaces, in which case you may not need to create any HA interfaces yourself. If no onboard HA interfaces are available, or additional interfaces are required to serve as backup HA links, data plane interfaces can be selected to fulfill this role and are connected to the HA peer.

## AE interfaces

To increase available bandwidth above the physical limitations of the interfaces, interfaces can be bundled into an **Aggregated Ethernet** (**AE**) group using the 802.1AX protocol. Up to eight interfaces can be combined into a logical bundle.

A new group can be created by clicking on **Add Aggregate Group** under **Network** | **Interfaces** | **Ethernet**.

You first need to set the type to Layer 2, Layer 3, VWire, or HA, which will require the same configuration as the physical interface equivalent (that is, security zone, VR, VLAN, or VWire).

Additionally, you can configure the **Link Aggregation Control Protocol** (**LACP**) to improve interface failure detection. LACP enables link failure detection on the physical and data link layer, while the default protocol only detects physical link failure.

You can set whether the firewall is in **Active** or **Passive** mode. This configuration setting needs to be reviewed with the LACP peer (typically the switch) as only one peer can be configured as **Active**, but LACP will not work if both are set to **Passive**.

The value of **Transmission Rate** will have an impact on the responsiveness of link failure detection, but it will also have an overhead. Slow transmission means every 30 seconds, while fast transmission means every second.

**Fast Failover** will fail to an operational interface within 1 second when an interface goes down. Traditional failover happens after 3 seconds.

**System Priority** determines which peer determines port priorities.

**Maximum Interfaces** determines how many interfaces can be active at the same time within the aggregate group. This number should not exceed the number of physical interfaces you assign to the group, but can be leveraged to limit total available bandwidth while keeping hot interfaces in reserve in case of failure. For example, if a total bandwidth of 4 gigabits is needed for an aggregate group but you also do not want to exceed this bandwidth to preserve system resources, you can assign five or more interfaces to the aggregate group, and set **Maximum Interfaces** to `4`. Only when an interface fails will another one be activated to pick up the work. In an HA configuration where two firewalls form a cluster, LACP can be enabled on the passive peer so the link aggregation group is pre-negotiated before the passive peer needs to assume an active role, which cuts down on the time needed to failover.

This is achieved by checking **Enable in HA Passive State**. The same system MAC can be used on both cluster members, but this may not be supported by the connected switches. This is shown in the following screenshot:

![](Images/B31589_02_69.png)

Figure 2.69: LACP

When the aggregate group is created, you can add the interfaces by setting **Interface Type** to **Aggregate Ethernet** and selecting the desired **Aggregate Group**:

![](Images/B31589_02_70.png)

Figure 2.70: A physical interface in an aggregate group

In some cases, you may need to be able to connect to a port mirror on a switch and just listen without participating. For such instances, you can configure a Tap interface.

With the information covered in the last sections, you are now able to select the appropriate interface for each network design you may come across. VWire helps you add a firewall in an environment where you can’t interfere with existing routing, Layer 3 interfaces put the firewall in the middle of routing decisions, Layer 2 interfaces make the firewall act in a similar way a switch would, and subinterfaces can be added to all of these to account for VLAN tags. You are able to configure link aggregation and can leverage tunnel interfaces to set up IPSec tunnels.

# Summary

In this chapter, you learned how to create a support account, register a new device, and add licenses. You are now able to identify all the different support licenses and select the appropriate subscription licenses to address your needs. You can upgrade and update a device so that its firmware is up to date and the latest application and threat signatures are loaded to protect the network. You learned how to protect the management interface so that only legitimate users can connect, and you can assign different accesses and privileges to administrators. You are able to configure all the physical interfaces, like Layer 3 and VWire, and know when each is most appropriate. You can also leverage logical interfaces like tunnel interfaces and loopback interfaces when they are needed.

If you’re preparing for the PCNSE, you should take note that upgrading requires the base image to be downloaded before you can move forward to a maintenance release. The recommended threshold for dynamic updates is 6 to 12 hours (unless the device is located in a critical environment, where the threshold should be 24 hours) and you should be able to identify the difference between all the interface types.

In the next chapter, we will start building robust security policies and learn how to set a strong security posture for network traffic.

# 3

# Building Strong Policies

In this chapter, you will get comfortable with configuring security profiles, building rule bases for security, and **network address translation** (**NAT**). We will learn what each setting does, what its expected behavior is, and how it can be leveraged to lead to the desired outcome. Taking full control over all of the features available in the different rule bases will enable you to adopt a strong security stance.

In this chapter, we’re going to cover the following main topics:

*   Understanding and preparing security profiles
*   Understanding and building security rules
*   Setting up NAT in all possible directions

By the end of this chapter, you will be able to set up a complete ruleset that will ensure your users are able to reach the applications and resources they need, internally hosted servers can be reached from the internet, and any threats trying to slip through can be stopped in their tracks.

# Technical requirements

Before you get started, your firewall must have connectivity between at least two networks, with one preferably being your **internet service provider** (**ISP**), to fully benefit from the information provided in this chapter.

# Understanding and preparing security profiles

There are several types of security profiles, like Antivirus, Vulnerability Protection, URL Filtering, and Anti-Spyware (we’ll cover all of these different profiles in the following sections). Before you can start building a solid security rule base, you need to create at least one custom security profile of each type to use in all of your security rules. There are pre-configured profiles available, but they are set to read-only, which means that if you start using them in security rules, you’ll need to replace them one by one if you create custom ones later.

**Pro Tip**

As a best practice, add security profiles to all of the rules, including drop rules. On drop rules, security profiles do not consume any resources but they will help form a habit of adding profiles to all rules, and if the rule ever changes to **Allow**, the profiles will already be assigned.

Security profiles are evaluated by the first security rule that a session is matched against. If a six-tuple is matched against a security rule with no or limited security profiles, no scanning can take place until there is an application shift and the security policy is re-evaluated. *All* security rules need to have security profiles.

## The Antivirus profile

The Antivirus profile has three sections that depend on different licenses and dynamic update settings:

*   The actions under **ACTION** rely on the threat prevention license and antivirus updates
*   **Wildfire Action** relies on the WildFire license and the WildFire updates that are set to periodical updates (1-minute or longer intervals)
*   **Wildfire Inline Ml Action **relies on WildFire set to real time

If any of these licenses are missing from your system, the actions listed in their columns will not be applied.

**Application Exception** allows you to change the action associated with a decoder for individual applications as needed. The actions that can be set for both threat prevention and WildFire antivirus actions are as follows:

*   **allow**: Allows matching signatures *without* logging.
*   **drop**: Drops matching signatures and writes an entry in the threat log.
*   **alert**: Allows matching signatures to pass but writes an entry in the threat log.
*   **reset-client**: Drops matching packets, sends a TCP RST to the client (session initiator), and writes an entry in the threat log. The server (session responder) is not notified.
*   **reset-server**: Drops matching packets, sends a TCP RST to the server (session responder), and writes an entry in the threat log. The client (session initiator) is not notified.
*   **reset-both**: Drops matching packets, sends a TCP RST to both the client and the server, and writes an entry in the threat log.

Packet captures can be enabled for further analysis by the security team or as forensic evidence. They are attached to the threat log and are limited to packets containing matched signatures.

You can create a new Antivirus profile by going to **Objects** | **Security Profiles** | **Antivirus**. As the following screenshot shows, we will use Palo Alto Networks’ recommended best practices settings:

![](Images/B31589_03_01.png)

Figure 3.1: Creating an Antivirus profile

For the Antivirus profile, the setting in the default profile will have **imap **and** pop3** set to **alert** only. Since these protocols do not respond too well to reset, this may be a good setting in cases where they are actively being used in the organization. In most cases, email has been replaced by a web-based (TLS/SSL) alternative and it would actually be more secure to reset legacy protocols.

Exceptions can be added in the **Signature Exceptions** tab for false positives or known true positives. Exceptions are added by typing (or pasting) the **Threat ID** as found in the threat log and clicking **Add**:

![](Images/B31589_03_02_new.png)

Figure 3.2: AntiVirus exceptions

Additional machine learning actions can be set on the **Wildfire Inline ML** tab for **Windows Executables**, **PowerShell Script 1** and **PowerShell Script 2**, **Executable Linked Format, MSOffice, Shell, OOXML, **and **MachO**. Each model can be set to one of three actions:

*   **enable (inherit per-protocol actions)**, which enables additional machine learning scanning, and if a virus is detected, the matching protocol (**smtp**, **http**, and so on) action that was set in the main **Action** tab is applied
*   **alert-only (override more strict actions to alert)** will enable additional machine learning scanning but positive matches will only be reported in logging
*   **disable (for all protocols)**, which is the default setting and does not apply machine learning to the selected model

The following screenshot illustrates the available actions per model:

![](Images/B31589_03_03.png)

Figure 3.3: WildFire Inline ML

WildFire inline ML can dynamically analyze certain file types for various file details and PowerShell scripts for malicious characteristics.

## The Anti-Spyware profile

The Anti-Spyware profile is extremely customizable and is built by a set of rules within the profile. These rules serve to change the default actions associated with each threat; so, if no rules are created at all, the profile will simply apply the default action for a specific signature when it is detected.

Anti-Spyware supports the same actions as Antivirus (**allow**, **drop**, **alert**, **reset-client**, **reset-server**, and **reset-both**), with the addition of **block-ip**, which can track by source or source-destination pair and will block the offending IP for a duration of `1`\-`3600` seconds. Tracking by source will block all connections from the client for the duration of the block, while tracking by source-destination pair will only block connections from the client to the target destination and will not block the same client from connecting to other destinations.

The **Packet Capture** options include:

*   **none**,
*   **single-packet**, which only captures the packet containing the payload matching a signature.
*   **extended-capture** enables the capture of multiple packets to help analyze a threat. The number of packets captured by **extended-capture** can be configured via **Device**  | **Setup**  | **Content-ID**. The default is `5`.

**Important note**

Enabling packet capture on all threats does require some CPU cycles. The impact will not be very large, but if the system is already very taxed, some caution is advised.

**Severity** indicates the severity level of the threat that applies to this rule.

Create a new Anti-Spyware profile and add the following rules:

*   **Policy Name**: **Block-Critical-High-Medium** 
    *   **Severity**: **critical, high, medium**
    *   **Action**: **reset-both**
    *   **Packet Capture**: **single-packet**
*   **Policy Name**: **Default-Low-Info** 
    *   **Severity**: **low**, **informational**
    *   **Action**: **default**
    *   **Packet Capture**: **disable** 
        
        **Important note**
        
        The official best practice profiles may look a little different than they have been laid out here. This is because Palo Alto splits up every severity (but with exactly the same settings). So, the preceding example is a little more efficient!
        

Your profile will now look like this:

![](Images/B31589_03_04.png)

Figure 3.4: Anti-Spyware Profile

The critical, high, and medium severity threats have an overall high ratio of being genuinely malicious, so resetting the connection is considered best practice. Collecting a packet capture for these threats can help us find out whether something that was blocked is false. Low and informational severity threats have a high likelihood of being purely informational and can be left as the default action.

As you can see in the following screenshot, we need to make sure we review **Category** as this allows a fine-grained approach to each type of threat if granularity and individualized actions are needed at a later stage:

![](Images/B31589_03_05.png)

Figure 3.5: Anti-Spyware categories

The Anti-Spyware profile also contains DNS signatures, which are split into two databases for the subscription services. The content DNS signatures are downloaded with the threat prevention dynamic updates. The DNS Security database uses dynamic cloud lookups.

The elements in each database can be set to **Alert**, **Allow**, **Block**, or **Sinkhole**. **Sinkhole** uses a DNS poisoning technique that replaces the IP in the DNS reply packet, so the client does get a valid DNS reply, but with an altered destination IP. This ensures that infected endpoints can easily be found by filtering traffic logs for sessions going to the sinkhole IP. You can keep using the Palo Alto Networks default sinkhole, `sinkhole.paloaltonetworks.com`, or use your preferred IP.

The way that the DNS sinkhole works is illustrated by the following steps and diagram:

1.  The client sends a DNS query to resolve a malicious domain to the internal DNS server.
2.  The internal DNS relays the DNS lookup to an internet DNS server.
3.  The firewall inserts a poisoned reply to the DNS query to the internal DNS server with a record pointing to the sinkhole IP.
4.  The DNS reply is forwarded to the client.
5.  The client makes an outbound connection to the sinkhole IP, instead of the malicious server. The admin immediately knows which host is potentially infected and is trying to set up **command and control** (**C2**) connections:

![](Images/B31589_03_06.png)

Figure 3.6: How a DNS sinkhole works

Blocking instead of sinkholing these DNS queries would implicate the internal DNS server as requests are relayed through it.

The default action for the **Command and Control** and **Malware** domains is to block the DNS reply, as sinkholing could trigger more evasive behavior. Therefore, it is recommended to leave these categories at their default settings. Select an appropriate action for all the other categories. For research purposes, you can enable packet capture.

In PAN-OS 11.2 and later, in the **DNS Zone Misconfiguration** field, you can add public-facing *parent* domains within your network. Advanced DNS security can monitor these domains for misconfigurations such as abandoned subdomains that could be abused by malicious actors.

At the bottom of this tab, we can select SVCB (type 64), HTTPS (type 65), and ANY (type 255), which are DNS resource record types used by encrypted DNS to encrypt the client hello during the DNS resolution process. Blocking these will prevent encrypted DNS sessions from being established, ensuring you can keep applying DNS security:

![](Images/B31589_03_07.png)

Figure 3.7: Anti-Spyware DNS signatures

Any false positives or trusted sites that somehow are malicious can be added to **DNS Exceptions **so they will no longer be blocked or sinkholed.

On the **Inline Cloud Analysis** tab, we can enable inline cloud analysis and define actions to be taken for each protocol where C2 traffic can be detected by local deep learning analysis engines. If certain domains or IP addresses should be excluded from this inspection, you can add an **external dynamic list** (**EDL**; we’ll learn about these later) or IP in the fields shown in the following screenshot:

![](Images/B31589_03_08.png)

Figure 3.8: Inline Cloud Analysis

Let’s now look at the Vulnerability Protection profile.

## The Vulnerability Protection profile

The Vulnerability Protection profile also uses rules to control how certain network-based attacks are handled. **Action** contains the same options as Anti-Spyware: **allow**, **drop**, **alert**, **reset-client**, **reset-server**, **reset-both**, and **block-ip**.

The reset actions send TCP RST packets. **block-ip** blocks all packets coming from a source and can be set to **monitor source** to block everything, or a source destination to only block packets to a specific destination for an amount of time.

**Host Type** determines whether the rule applies to a threat directed at a client (download: for example, a browser vulnerability), server (upload: for example, SQL injection), or any.

Make sure to review the available **Categories** because they, just like we saw in *Figure 3**.4* in Anti-Spyware, can allow far more granularity if needed:

![](Images/B31589_03_09.png)

Figure 3.9: The Vulnerability Protection profile categories

Create the following rules:

*   **Rule Name:** **Block-Critical-High-Medium** 
    *   **Host Type:** **any**
    *   **Severity:** **critical, high, medium**
    *   **Action:** **reset-both**
    *   **Packet Capture:** **single-packet**
*   **Rule Name:** **Default-low-info** 
    *   **Host Type:** **any**
    *   **Severity:** **low**, **informational**
    *   **Action:** **default**
    *   **Packet Capture:** **disable**

Your profile should now look like this:

![](Images/B31589_03_10.png)

Figure 3.10: Vulnerability Protection Profile

The profile will reset any connection matching critical, high, or medium severity threats and will collect a packet sample of the threat for research or forensics. Low and informational threats can be left to the default settings.

While these rules will provide good coverage for all signatures that match a certain severity, you may need to add a rule for a specific CVE at some point. If you create a new rule, you can tailor it to a specific CVE or even a specific vendor.

CVE codes are maintained by MITRE at https://cve.mitre.org

The vendor IDs will usually be posted by the vendor and will have a similar format to, for example, Palo Alto’s PAN-SA-yyyy-nnnn or Microsoft’s MSyy-nnn. If you want to, for example, filter out a specific year, you could add PAN-SA-2024 to the vendor ID. Remember to put the more specific rules matching CVEs or Vendor IDs at the top of the rules.

On the **Inline Cloud Analysis** tab, you can enable inline machine-learning-powered detection for SQL injection attacks or command injection attacks. You can add a URL-based or IP-based EDL in the **Exclude from Inline Cloud Analysis** section so that they are not inspected, which can be useful for development environments where you could encounter many false positives:

![](Images/B31589_03_11_new.png)

Figure 3.11: Vulnerability Protection Inline Cloud Analysis

In the next subsection, we will learn about the categories of the URL Filtering profile.

## URL Filtering profile

URL Filtering leverages URL categories to determine what action to take for each category.

There are two groups of categories: custom URL categories and the dynamic categories provided by the URL Filtering license.

### Custom URL categories

Custom URL categories do not require a license, so you can create these objects and apply URL Filtering even without access to the URL Filtering license.

Go to **Objects** | **Custom Objects** | **URL Category** to create a new custom category and add websites. There are two types of custom categories:

*   **A URL list**: The URL list allows manual entry of URLs, one per line. It takes a light form of **regular expression** (**RegEx**) matched against the address, so neither `http://` nor `https://` are required to match.
*   **A category match**: The category match allows you to combine any of the predefined categories in a custom category. This could come in handy when applying security rules or a decryption policy to a group of predefined categories.

The string used in a custom URL category is divided up into substrings, or tokens, by separators. The `./?&=;+` characters are considered separators, so `www.example.com` has three tokens and two separators. Each token can be replaced by a wildcard (**\***) to match subdomains or entire **top-level domains** (**TLDs**). Wildcards cannot be used as part of a token; for example, `www.ex*.com` is an illegal wildcard. Each string can be closed by a forward slash (**/**) or be left open by not adding an end slash. Not ending a string could have consequences if the string is very short or very common as it could match unintended longer addresses. For example, the `*.com` string could match `www.communicationexample.org`, so adding an ending slash would prevent this.

There is also an **Append Trailing Slash** function in **Device** | **Setup** | **Content-ID** | **URL Filtering** that is enabled by default. It will automatically add a trailing slash to any EDLs or custom URL categories where a slash was not added. If you do need open-ended entries, disable this function.

Having multiple wildcards would require the use of a subdomain, so `*.*.com` would match anything.anything.com, but not anything.com.

### Configuring the URL Filtering profile

When configuring the URL Filtering profile, you need to select which action to apply in the **Site Access** column for each URL Filtering category within the **Categories** tab, as you can see in the following screenshot:

![](Images/B31589_03_12.png)

Figure 3.12: URL Filtering Profile

The available actions are as follows:

*   **none:** Exclusively used for custom URL categories. This excludes the URLs contained in the custom category from being used in this URL Filtering profile.
*   **allow:** Allows a category without logging.
*   **alert:** Allows a category and logs the access in the URL Filtering log.
*   **block**: Blocks the request, injecting an HTTP 503 error and a redirect to a page hosted on the firewall explaining to the user that their access was declined and the action was logged.
*   **continue:** Injects an interactive web page informing the user that they are about to access a restricted website and provides a **Continue** button for them to acknowledge the risk associated with accessing the site.
*   **override**: Injects an interactive web page that allows the user to continue if they are able to provide a password to continue. This password can be set in **Device | Setup | Content-ID | URL Admin Override**.

Additionally, to be able to use the **override** option, an Interface Management profile (**Network | Network Profiles | Interface Mgmt**) needs to be created. This profile should have the **Response Pages** service enabled and added to the interface where users connect to for this page to work, as illustrated in the following screenshot:

![](Images/B31589_03_13.png)

Figure 3.13: Interface Management Profile

As you saw in *Figure 3**.10*, the URL Filtering profile requires each **Category** field to be set to an action individually for site access, and if **User Credential Submission** is enabled, additional filtering can be applied to decide whether a user is allowed to submit corporate credentials to a certain category. This helps prevent phishing attacks.

As you can see in the following screenshot, if you want to change a lot (or all) of the actions at once, there’s a shortcut to help you. If you hover your mouse over **Site Access** or **User Credential Submission,** there will be a little arrow that lets you select **Set All Actions** or **Set Selected Actions**:

![](Images/B31589_03_14.png)

Figure 3.14: Set All Actions in the URL Filtering profile

A good baseline URL Filtering policy can be set up as follows:

1.  Set all of the categories to **alert**. This will ensure that all of the URL categories are logged.
2.  Set **Adult**, **Command and Control**, **Copyright Infringement**, **Extremism**, **Malware**, **Peer-to-Peer**, **Scanning-activity**, **Real-time-detection**, and **Phishing and Proxy Avoidance and Anonymizers** to **Block**.
3.  Set **Dating**, **Gambling**, **Games**, **Hacking**, **Insufficient Content**, **Not-Resolved**, **Parked**, **Questionable**, **Unknown**, and **Web Advertisements** to **Continue**.
4.  Tweak the settings in accordance with your company policy or local laws and regulations (some URL categories cannot be logged by law, for example). The **Categories** set to **Continue** are commonly on the fringes of acceptance, but may still need to be accessed for legitimate purposes. The **Continue** action gives the user the opportunity to ensure that they intend to go to this URL before actually opening the web page.

The **URL Filtering Settings** tab contains several logging options that may come in handy, depending on your needs:

*   **Log container page only:** This setting only logs the actual access a user is requesting and will suppress related web links, such as embedded advertisements and content links on the page that the user is visiting, thereby reducing the log volume.
*   **Safe Search Enforcement:** This blocks access to search providers if strict safe search is not enabled on the client side. Currently, Google, Bing, Yahoo, Yandex, and YouTube are supported.

Additional logging can also be enabled:

*   **User-Agent**: This is the web browser that the user is using to access a web page.
*   **Referer**: This is the web page that links to the resource that is being accessed (for example, Google or CNN linking to a resource page).
*   **x-forward-for**: If a downstream proxy is being used by users, this masks their original source. If the downstream proxy supports enabling the **x-forward-for** feature, it will add the client’s original IP in the **c** header, allowing the identification of the original user.

The following steps and screenshot show you how to enable these settings in your URL Filtering profile:

1.  Enable **Log container page only** to give your users some privacy and prevent the logging of embedded ad pages.
2.  Enable **Safe Search Enforcement**.
3.  Enable additional logging for **User-Agent** and **Referer**.

![](Images/B31589_03_15.png)

Figure 3.15: URL Filtering settings

The **User Credential Detection** tab allows you to enable credential detection (see *Chapter 6*, *Identifying Users and Controlling Access*, for more details).

**HTTP Header Insertion** lets you control web application access by inserting HTTP headers into the HTTP/1.x requests to application providers. As you can see in the following example, this can help you control which team IDs can be accessed in Dropbox, and which tenants and content can be accessed in Office 365 and Google app-allowed domains. You can create any URL that needs to have a certain header inserted to ensure users are accessing the appropriate instance:

![](Images/B31589_03_16.png)

Figure 3.16: HTTP Header Insertion

The **Inline Categorization** tab lets you enable additional scanning to help identify phishing sites or malicious JavaScript and requires the Advanced URL subscription license:

![](Images/B31589_03_17.png)

Figure 3.17: Inline Categorization

You can enable local and cloud categorization individually, in case you prefer not to perform cloud lookups or not put additional workload on the firewall.

### URL Filtering priorities

Some sites may fall into multiple categories and, on top of this, may be listed in a custom category or external dynamic URL list. The way URL Filtering decides which action to apply is based on the severity of the action that is applied to it, and whether it is in a custom category, external dynamic URL list, or pre-defined category. The actions based on the severity in descending order are as follows:

1.  Block
2.  Override
3.  Continue
4.  Alert
5.  Allow

The order of categories is as follows:

1.  Custom URL categories
2.  External dynamic URL lists
3.  Pre-defined categories

So as an example – if a URL is present in all categories, the action of the custom URL category will be applied. If a URL is present in multiple custom URL profiles, the most severe action will be applied.

Now, let’s look at the File Blocking profile.

## The File Blocking profile

The default **strict file blocking** profile contains all the file types that are commonly blocked and serves as a good template to start from. Select the strict profile and click on the **clone** action, as in the following screenshot, to create a new profile based on this one.

If any file types do need to be allowed in your organization, remove them from the block action:

![](Images/B31589_03_18.png)

Figure 3.18: File Blocking profile clone

The direction lets you determine whether you want to only block uploads or downloads or both directions for a specific file type, as well as groups of file types. File Blocking profiles also use rules so that file types can be grouped with their own directions and actions. The default action is **Allow**, so any file type not included will be allowed to pass through (but will be scanned if an appropriate security profile is attached to the security policy). The available actions are **Alert**, **Block**, and **Continue**, which works similarly to the URL Filtering **Continue**  option if the file is being downloaded from a web page that supports the HTTP redirect to serve the user a warning page before continuing with the download or upload.

Review all the file types and set the ones you want to block. Any file types that you are not sure about and would like to get a chance to review first can be set to the **Alert**  action so that you can keep track of occurrences at **Monitor**  | **Logs**  | **Data Filtering.**

As you can see in the following screenshot, we can create sets of file types by clicking on the **Add**  button, selecting the file type, and then setting the action:

![](Images/B31589_03_19.png)

Figure 3.19: File Blocking Profile

We will now have a look at the WildFire Analysis profile.

## The WildFire Analysis profile

The WildFire Analysis profile controls which files are uploaded to WildFire for analysis in a sandbox and which ones are sent to a private instance of WildFire (for example, the WF-500 appliance). Clone the default profile to upload all files to WildFire, or create a new profile if you want to limit which files are forwarded or need to redirect files to a private cloud. If no WildFire license is available, only **Portable Executables** (**PEs**) are forwarded to WildFire.

If all file types can be uploaded for inspection, simply set a rule for any application and any file type. If exceptions exist, either create a rule to divert specific files to a private cloud (if you have a WildFire appliance in your data center) or specify which files *can* be uploaded, as shown:

![](Images/B31589_03_20.png)

Figure 3.20: WildFire Analysis Profile

In addition to uploading files to a cloud or private sandbox, you can also send files to the cloud for inline scanning. Uploading files to the WildFire sandbox will take a little more time but will execute a file in a virtual sandbox and let it “explode” for full analysis. The inline scanning is limited to PE files but can leverage machine-learning-powered detection engines to detect advanced threats. You can create rules to determine which files for which application can be uploaded, and which action to take when malware is detected:

![](Images/B31589_03_21.png)

Figure 3.21: Inline Cloud Analysis

We’ve now covered all the security profiles, which you can use in security rules later on, but you can also create custom objects to more finely tune into specific threats or data patterns that you want to be able to identify and take action on. In the next section, we’ll learn how to create these objects and what is required to be able to define the data that needs to be added for the custom object to work as expected.

## Custom objects

We have already looked at custom URL categories, but, as we will discuss in this section, you can also create custom spyware and vulnerability signatures combining strings of data and RegEx to match a certain signature and take action through a security profile. This can come in handy if you are aware of a resource that could be vulnerable to a specific string of code, are subscribed to a threat feed that provides you with signatures you can add yourself, or want to control what happens when a pattern is detected.

### The Custom Spyware/Vulnerability objects

You can create your own signatures using RegEx to detect spyware phone-home/C2 or network vulnerabilities. The **Configuration** page, as shown in the following screenshots, requires basic information, such as an ID number that is between `15.000-18.000` and `6900001-7000000` for spyware and `41.000-45.000` and `6800001-6900000` for vulnerabilities, a name, a severity value, a direction, and any additional information that may be useful later on. The direction and affected client help the Content-ID engine identify which direction packets that match this signature can be expected:

![](Images/B31589_03_22.png)

Figure 3.22: The Custom Spyware and Vulnerability objects

In the **Signatures** tab, you have two main modes of adding signatures, as you can see in the following screenshot:

*   **Standard**: This adds one or more signatures, combined through logical AND or OR statements.
*   **Combination**: This combines predefined (dynamic update) signatures with a timing component requiring *n* number of hits over *x* amount of time, aggregated for source, destination, or source-destination.

![](Images/B31589_03_23.png)

Figure 3.23: The Standard or Combination signatures

Combined signatures allow you to pick predefined signatures and add a timing attribute, so action is only taken after the signature has been detected a number of times within a certain timeframe. So, let’s focus on standard signatures as this is where we can build our own signatures.

From the main screen, you can add sets of signatures, which are all separated by a logical OR statement.

Once you start building a set, you need to decide on the scope. The transaction matches a signature in a single packet and the session spans all the packets in the session. If the signature you are adding to identify a threat always occurs in a single packet’s payload, you should set a transaction. This will allow the Content-ID engine to stop scanning at once. If you are adding multiple strings, you can enable **Ordered Condition Match**, which requires the signatures to match from top to bottom in an ordered way. If this option is turned off, the last signature may be detected before the first. If you add multiple strings, you can link them by adding an AND condition.

A signature consists of the following:

*   An **operator** is either a pattern, or a greater, equal, or smaller operator. Greater, equal, and smaller operators allow you to target a header, payload, payload lengths, and more. A pattern lets you match an exact string found anywhere in a packet or a series of packets.
*   A **context** is where, in any of the available protocols, the signature may be found (for example, if you look for a string in `http-req-host-header`, that same string will not be matched if it is seen in the payload).

For a full list, there’s a good online resource describing all the contexts at [https://knowledgebase.paloaltonetworks.com/KCSArticleDetail?id=kA10g000000ClOFCA0](https://knowledgebase.paloaltonetworks.com/KCSArticleDetail?id=kA10g000000ClOFCA0). However, many contexts will be self-explanatory, as you can see in the following screenshot:

![](Images/B31589_03_24.png)

Figure 3.24: Creating signatures

*   A **pattern** or **value**: If you want to, for example, match a hostname in an HTTP request header, you would use the `domain\.tld` RegEx, where the backslash indicates that the dot following it is an exact match for a dot and not a RegEx wildcard.

The available RegEx wildcard characters include the following:

![](Images/B31589_03_25.png)

Figure 3.25: Supported RegEx wildcard characters

*   A **qualifier** can further limit at which stage of a transaction a pattern can be matched, either in method or type. Using a qualifier is optional:

![](Images/B31589_03_26.png)

Figure 3.26: Host header pattern

A good tool to capture usable payload information from a session is Wireshark, which you can download from [https://wireshark.org](https://wireshark.org).

With the above custom objects, you are able to identify sessions behaving in a specific way, but this process can also be applied to identify information and keywords inside a session.

### The custom data pattern

In the custom data pattern, you can add strings of sensitive information or indicators of sensitive information being transmitted. There is a set of predefined patterns, including social security numbers, credit card numbers, and several other identification numbers. You can use RegEx to match exact strings in documents or leverage file properties. Once the appropriate parameters have been chosen, you can add these custom data patterns to a Data Filtering profile and, as you can see in the following screenshot, assign weights. These weights determine how many times a certain marker can be hit in a session before an alert is generated in the form of a log entry and when a session should be blocked for suspicious behavior (for example, it might be acceptable for an email to go out containing one social security number, but not multiple):

![](Images/B31589_03_27.png)

Figure 3.27: Data Filtering Profile

Now that you’ve had a chance to review and configure all the available security profiles, the easiest way to apply them to security rules is by using security profile groups.

## Security profile groups

Now that you’ve prepared all of these security profiles, create a new security profile group, as shown in the following screenshot, and call it **default**. This will ensure that the group will automatically be added to every new security rule you create (it is not added to existing rules, so make sure to review them):

![](Images/B31589_03_28.png)

Figure 3.28: The default security profile group

It is not harmful to add *all* of the security policies to a security rule as Content-ID will intelligently only apply appropriate signatures and heuristics to applications detected in the session (for example, HTTP signatures will not be matched with FTP sessions).

Also, create a Log Forwarding profile in **Objects | Log Forwarding** called **default**, but you can leave the actual profile empty for now. This serves the same purpose as the default security profile group in that it automatically populates the log-forwarding action of each new security rule. It is easier to update the profile than to have to add a profile to each rule later on.

You are now able to build your own security profiles and add custom signatures where needed. With the information you have learned, you will be able to ensure the security rules we will be creating in the next section are set to block threats and scan content.

# Understanding and building security rules

We now need to build some security rules to allow or deny traffic in and out of the network. The default rules will only allow intrazone traffic and will block everything else, as you can see here:

![](Images/B31589_03_29.png)

Figure 3.29: Default security rules

In the following sections, we will start to build a rule base, making sure we first introduce some rules to drop undesirable sources and destinations, followed by adding permissive policies focused on allowing applications required by the users in a secure way, leveraging App-ID to ensure only the intended applications are let through. Finally, we’ll look in more detail at which objects make up rule bases and how the rules can be kept tidy after having been in use for a while.

We will first make sure “bad” traffic is dropped by creating two new rules—one for inbound and one for outbound traffic.

## Dropping “bad” traffic

The inbound rule will have the external zone as a source and the four EDLs containing known malicious addresses. These lists are updated via the threat prevention dynamic updates, so if you do not see them, make sure you have the Threat Prevention license and have downloaded and installed the latest content packages. Add a descriptive name and any relevant tags on the **General** tab. The **Source** tab should look similar to the following:

![](Images/B31589_03_30_new.png)

Figure 3.30: Reconfigured external dynamic lists

In the **Destination** tab, set the destination zones to both the external zone and any zone where you intend to host internal servers to which you will allow inbound NAT (for example, corporate mail or web servers) and set the destination addresses to **Any**, as in the following screenshot:

![](Images/B31589_03_31.png)

Figure 3.31: Security rule destination zones

We’ll leave the **Application** and **Service/URL Category** tabs default for now. In the **Actions** tab, set the action to **Drop**. This will silently discard any inbound packets:

![](Images/B31589_03_32.png)

Figure 3.32: Security rule actions

Follow the next steps to create the above inbound drop rule:

1.  Create a new security rule and give it a descriptive name, like Malicious EDL Inbound Drop.
2.  Additional information can be added in the Description field.
3.  Set the source zone to any zone that is connected to the internet (for example, Untrust).
4.  Set the source addresses to the four (or a subset) predefined EDLs.
5.  Set the destination zones to your internal zones that will accept inbound connections from the internet (for example, DMZ), also including the external zones, or simply use the any option.
6.  Set the action to Drop.

**Important note**

You may have noticed that the **Profile Setting** fields and **Log Forwarding** are filled out with the **default**  profiles that you created in the previous step. In all rules where sessions are blocked, content scanning will not take place, so having these profiles will not cause overhead.

Click **OK**, and then make the reverse rule, as in the following screenshot, setting the source zones to your internal zones, the destination to the external zone, and the predefined EDL as addresses. If you changed the DNS sinkhole IP address to one of your choosing, add this IP here as well:

![](Images/B31589_03_33.png)

Figure 3.33: Outbound drop rules

Follow these steps to create the above outbound drop rule:

1.  Create a new security rule and give it a descriptive name, like `Malicious EDL Outbound Drop`.
2.  Set the source zone to your internal zones (for example, **Trust**, **DMZ**).
3.  Set the destination zone to any zone leading out to the internet (for example, **Untrust**).
4.  Set three destination addresses and, for each one, select one of the predefined EDLs.
5.  Set **Action** to **Drop**.

A good practice is to add some **catch all** rules to the end of your rule base, as in the following screenshot, once all the required policies have been added to **catch any** connections that are not allowed. From suspicious zones, one **catch all** rule should be set to **application-default** and one to **any**; this will help identify standard applications that are not hitting a security policy and (more suspicious) non-standard applications that are not using a normal port (see the *Allowing applications* section to learn about the **application-default**  service):

![](Images/B31589_03_34.png)

Figure 3.34: The catch-all rules at the end of the rule base

Adding an **any**\-**any** drop rule as the last catch all will override the intrazone default allow rule and may block some services, so ensure you account for these if you want to discard all unaccounted-for connections. Commonly overlooked connections are DHCP relay, IPSec, and GlobalProtect as intrazone.

You now have some rules actively dropping connections you do not want to get past the firewall, but there are more options available than just silently discarding packets. We’ll review the other options next.

### Action options

Several different actions are available to handle new connections, some of which are stealthy and some of which are noisy and informative. Choose which one to use depending on your needs:

*   **Deny**  will drop the session and enforce the default **Deny**  action associated with an application. Some applications may silently drop while others send an RST packet.
*   **Allow** allows the session to go through.
*   **Drop** silently discards packets.
*   **Reset Client** sends a TCP RST to the client.
*   **Reset Server** sends a TCP RST to the server.
*   **Reset Both** sends a TCP RST to both the client and the server.

If you check the **Send ICMP Unreachable** checkbox and the ingress interface is Layer 3, an **ICMP Unreachable** packet is sent to the client for all of the dropped TCP or UDP sessions. This can be useful to gracefully end UDP sessions as UDP does not support RST packets. It will also help the remote host to find an alternate destination or give up, whereas without the ICMP message, it may keep spamming the firewall with UDP packets.

## Allowing applications

There are generally two ways to determine which applications you want to allow:

*   Creating a group of known applications
*   Creating an application filter to sort applications by their behavior

From **Objects**  | **Application Groups**, you can create groups of known applications that can be used in security policies, as shown:

![](Images/B31589_03_35.png)

Figure 3.35: Application Group

**Important note**

The security rule base is evaluated from top to bottom and the evaluation is stopped once a match is found, and then the matching security rule is enforced. This means blocking rules need to be placed *above* the allowing rule if there could be an overlap.

With the widespread adoption of cloud-based hosting and cheap SaaS solutions, more traditional programs are turning into web-based applications that are accessible over a web browser. This makes it harder for an administrator to easily determine which applications need to be allowed as the needs of the business change quickly. Application filters created in **Objects** | **Application Filters** let you create a dynamic application group that adds applications by their attributes, rather than adding them one by one. These attributes can be selected for “good” properties to be added to allow rules (as you can see in the following screenshot) or “bad” properties to drop rules:

![](Images/B31589_03_36.png)

Figure 3.36: Application Filter with basic attributes

Alternatively, the filter can be based on the predefined and custom tags assigned to applications, as follows:

![](Images/B31589_03_37.png)

Figure 3.37: Application Filter with tags

You can mix and match application groups and filters to build further security rules by adding them to the **Applications** tab, as you can see here:

![](Images/B31589_03_38.png)

Figure 3.38: The APPLICATIONS tab in a security rule

To create a new **Allow** rule using an application filter, do the following:

1.  Create a new security rule and add a descriptive name.
2.  Set the source zone to the **internal zone** that will connect to the internet.
3.  Set the destination zone to **external zone**.
4.  In **Applications**, add a new line and select **Application Filter**.
5.  Click on all of the desired attributes and review some of the applications at the bottom. Add a descriptive name and click **OK** on the filter, and again on the security rule.

You now have an **allow**  rule based on an application filter!

### Application dependencies

You may notice that when you start adding applications to a security rule, some applications have dependencies. These dependencies will appear in the **Depends On** field when you add your applications. These applications rely on an underlying protocol or build on an existing, more basic application that needs to be added and allowed in the security rule base for this sub-application to work.

They do not necessarily need to be added to the same security policy.

Starting from PAN-OS 9.1, these dependencies are displayed in the security rule. As you can see in the following screenshot, they appear when you are adding new applications and can immediately be added to the same security rule or to a different one in the security rule base.

Since the firewall will go over the security rules each time a session changes to a new App-ID, you may already have the dependency satisfied in a different rule and as soon as one of the App-IDs you set in this new rule is matched, the firewall will change the session to this new rule. In older PAN-OS versions, users will only be warned about these dependencies once the configuration is committed. You can review application dependencies for individual applications via **Objects**  | **Applications** too:

![](Images/B31589_03_39.png)

Figure 3.39: Application dependencies

Now that the applications have been set, let’s look at how service ports are controlled.

### Application-default versus manual service ports

Each application will use a certain service port to establish a connection. By default, each service is set to **application-default**, which forces each application to use its default ports (for example, web browsing uses ports `80` (unsecured) and `443` (SSL) secured, while FTP uses ports `21` (unsecured plaintext) and `990` (secured)).

**Important note**

Protocols that use pinholing, such as FTP, are automatically taken care of via the **Application Layer Gateway** (**ALG**), which is a part of the content decoder that is specific to this protocol. The ALG ensures “child” sessions are accepted as part of the same security rule even though the port may be different from the default port.

If an application needs a custom port, you can add a manually created service object, but this would prevent the use of **application-default**. So, any exceptions should preferably be made in individual rules to prevent applications from “escaping” via an unusual port:

![](Images/B31589_03_40.png)

Figure 3.40: Service ports

Adding a URL category can be used to allow or block URL categories at the TCP layer:

![](Images/B31589_03_41.png)

Figure 3.41: URL Category in the security rule

Rather than applying URL Filtering in (ISO) Layer 7, a URL category added directly to the security rule simplifies allowing or denying access to a specific category.

When an outbound connection is identified as web browsing (or SSL), the content decoder will identify the URL that is being accessed and a category lookup will take place. If a URL category is set in the security rule, as illustrated in *Figure 3**.38*, a Layer 3 action can immediately be applied to allow or deny a session from proceeding.

This is different from URL Filtering via a security profile in that URL Filtering will compare the URL found in a session against all predefined URL categories and custom URL categories. This could introduce complications with overlapping categories or may make a rule too wide.

For example, if only a specific category needs to be allowed or denied, a traditional URL Filtering security profile would need to be set to allow the desired categories and block all the other ones, which may disrupt other more generic web browsing security rules. A typical example is a printer or other IoT device that needs access to a preset list of URLs to receive updates or report metrics, but no other URL categories. Using a custom URL category directly in the security rule limits that device’s access to only these FQDNs.

## Controlling logging and schedules

By default, each security rule is set to **Log at Session End**. This means that a log is only written to the traffic log once a session is broken down. For some sessions, it may be interesting to log more interactions, and so **Log at Session Start**  could be enabled. This does cause quite a lot of overhead, however, as there will be a log for each new stage of a session when the SYN packet is received and for every application switch. So, there could be two to five additional log entries for a single session.

Other applications that are very chatty or less relevant may not need to be logged at all, such as DNS.

**Important note**

Even with both the start and end logs disabled in the security rule action tab, any threats detected in a session will still be logged to the threat log.

Log forwarding can be used to forward logs to Panorama, Strata Logging Service, or a syslog server or to send out an email. If you name one of the log forwarding profiles default, it will automatically be added to every new security rule that is created:

![](Images/B31589_03_42.png)

Figure 3.42: Log options and schedules

**Schedule** can be used to create timeframes when this security rule will be active if certain applications are only allowed at specific times of the day (for example, Facebook can be allowed during lunch and after hours):

![](Images/B31589_03_43.png)

Figure 3.43: Schedules

Before you continue putting this new knowledge to work and start creating more rules, let’s review how you can prepare address objects so the rule base becomes more readable and you can reuse similar objects in multiple rules.

## Address objects

To make managing destinations in your security and NAT policy a little easier, you can create address objects by going to **Objects**  | **Addresses**. When you create a new object here, you can reuse the same object in different rules, and if something changes, you only need to change the address object for all the security and NAT rules to be automatically updated:

1.  Click on **Add** and provide a descriptive name for the address. It is good practice to set up a naming convention so that you can repeat this process for all other address objects. A good example is to prefix all server names with `S_` and all networks with `N_` so that they’re easily identifiable.
2.  Set a description if needed.
3.  Select the type of object that this will be: 
    *   **IP Netmask** lets you set an IP with a subnet mask down to `/32` or `/64` for a single IPv4 or IPv6 address (no need to add `/32`).
    *   **IP Range** lets you define a range that includes all the IP addresses between the first and last IP set in the range, separated by a dash (**\-**).
    *   **IP Wildcard Mask** lets you set a subnet masking that covers binary matches, where a zero bit requires an exact match in the IP bit, and **1** is a wildcard. So, for example, a wildcard subnet of `0.0.0.254` translates to `000000000.0000000.0000000.11111110`. The first three bytes are set, and in the last byte, all but the first bit are wildcards. This means that if the associated IP address is set to `10.0.0.2` (`00001010.0000000.000000.00000010`), all of the IPs in the subnet that end with 0 will be matched (that is, all of the even IP addresses). If the IP is set to `10.0.0.1`, all of the odd IPs would match. This type of object can only be used in security rules.
    *   **FQDN** lets you set a domain name that the firewall will periodically resolve according to the **time to live** (**TTL**) and cache. Up to `10 A` or `AAAA` records are supported for each FQDN object. Use the **Resolve**  link to verify that the domain can be resolved.
4.  Add a tag to easily identify and filter policies for this object.
5.  Click **OK**.

Once you have sets of objects that are similar, you can also create groups by going to **Objects**  | **Address Groups**. These groups can be used to bundle objects for use in security or other policies.

## Tags

Tags can be leveraged to group, filter, or easily identify many other objects. Security zones, policy rules, or address objects can all be tagged with up to 64 tags per object. By going to **Objects**  | **Tags**, you can create new tags:

1.  Click on **Add** and create a descriptive and preferably short name for the tag (up to `127` characters). You can also use the dropdown to select one of the already-created security zones, which will cause the tags to be automatically assigned to this zone.
2.  Select a color or leave it as **None**.
3.  Add a comment.
4.  Click **OK**.

As you can see in the following screenshot, tags can then be used to visually enhance your rule base or to filter for specific types of rules:

![](Images/B31589_03_44.png)

Figure 3.44: Tags in the security policy

While building security rules, objects (such as addresses, applications, and services) can be *clicked and dragged* from the object browser on the left into any rule, and from one rule to another. There is no need to open a rule and navigate to the appropriate tab to add objects.

After a while, you will want to review the security rule base you’ve built to make sure you haven’t missed any applications, left rules too open, or have any duplicates that leave rules unused. Let’s take a look at how this can be done.

## Policy Optimizer

While you’re on the **Security **or **NAT policy** tab, there’s a tool called **Policy Optimizer** in the bottom left-hand corner that can help improve your security rules by keeping track of rule usage. Policy Optimizer records statistics relating to your rules and can report the following:

*   Rules that have been unused for 30 days, 90 days, or for all time so that you can delete them
*   Rules that are set up with no applications defined and the applications that were accepted by those rules
*   Rules that have applications that are not being used so that you can remove these excess applications

After the rule base has been in production for a while, the output of Policy Optimizer will start to resemble the following screenshot:

![](Images/B31589_03_45.png)

Figure 3.45: Policy Optimizer

In tandem with Policy Optimizer, each rule also has a column called **Apps Seen**.

### The Apps Seen column

Each security rule will also gather information about the applications passing through it. This information can be accessed from the **Apps Seen** column in the security policy:

![](Images/B31589_03_46.png)

Figure 3.46: Apps Seen

All detected applications can be added to the current rule, an existing rule, or a cloned rule from the current rule.

# Creating NAT rules

Unless you are one of the lucky few organizations that were able to get their very own `A`  (`/8`) or `B` (`/16`) class subnets, your internal network segments will most likely be made up of one or more of the well-known RFC1918 private IP address allocations: `10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16`. NAT is needed in order for your hosts to be able to reach the internet and your customers and partners to reach publicly available resources hosted in your data center. NAT rules can be configured through **Policies** | **NAT**.

For this section, keep the following interface setup in mind:

![](Images/B31589_03_47.png)

Figure 3.47: Interface zone and IP configuration

Address translation comes in different flavors depending on the direction and purpose, each with its own nuances. Let’s first review inbound NAT.

## Inbound NAT

For inbound NAT, it is important to remember that the firewall is zone-based and that the source and destination zones are determined *before* the NAT policy is evaluated:

![](Images/B31589_03_48.png)

Figure 3.48: Packet flow stages

This means that for inbound NAT, the source and destination zones will be identical. The routing table will determine the source zone based on the default route and the destination zone based on the connected network, which is configured on the external interface.

For example, if the `203.0.113.1` internet IP is connecting to the `198.51.100.2` firewall IP to reach the `10.0.0.5` server, the firewall will look up `203.0.113.5` in its routing table. It will find that it only matches the default route, `0.0.0.0/0`, which points from the **ethernet1/1** interface, which is in the **Untrust-L3** zone. It will then look up `198.51.100.2` (the original destination IP in the packet header) and find it in the `198.51.100.0/24` connected network on the **ethernet1/1** interface, which is in the **Untrust-L3** zone.

The **Original Packet** tab needs to have the following:

*   The same source and destination zones.
*   **Source Address** can be **Any** for generic internet sources, specific IP addresses, or subnets if the source is known.
*   **Destination Interface** indicates which interface the packet is headed to. This can be important in cases where there are multiple interfaces with overlapping routes.
*   **Service** can be used to restrict which destination port is allowed in the received packets. This will help in cases where the IP space is restricted and **port address translation** (**PAT**) is required to host different services on the same external IP and will prevent over-exposing an internal host.
*   **DESTINATION ADDRESS** needs to be a single IP for a one-to-one destination NAT (don’t add a subnet). Having a subnet-based destination NAT is possible, but only for **Session Distribution**.

The **Original Packet** tab of an incoming NAT rule will look similar to the following screenshot:

![](Images/B31589_03_49_new.png)

Figure 3.49: Original Packet NAT translation

In the **Translated Packet** tab, you can set what needs to be changed for the external client to be able to reach the internal server:

*   **Source Address Translation**  will usually be set to **None**, but it can be set to match an internal interface subnet or loopback interface if required. This would let the server receive a packet sourced from an internal IP, rather than the original internet IP.
*   **Destination Address Translation** with **Translation Type** set to **Static IP**, also known as one-to-one NAT, changes the destination IP to a single internal server.
*   **Translated Port**  can be used if the internal service runs on a different port than the externally advertised one. For example, externally, a web server could be reachable on default SSL port `443`, while on the server itself, the service is enabled on **8443**.

In the **Translated Packet** tab of an inbound NAT rule, we will add the private IP of the destination server, as illustrated here:

![](Images/B31589_03_50.png)

Figure 3.50: Translated Packet NAT translation

Next, let’s take a look at address translation in the opposite direction.

## Outbound NAT

Outbound NAT rewrites the source IP addresses of internal clients to the interface associated with a different zone. This could be an internet-facing zone or one connecting to a partner, VPN, or WAN, as in the following screenshot:

*   The source zone will reflect the interface that the clients are connected to.
*   The destination zone and destination interface will reflect the egress interface that a routing lookup determines based on the original packet.

![](Images/B31589_03_51.png)

Figure 3.51: Outbound NAT Original Packet

When using an IP pool for source translation, the firewall will use proxy ARP to gain ownership of IP addresses. This means that you don’t need to physically configure all of the IP addresses on an interface, but it is recommended that you have at least the subnet configured on an interface so that the firewall knows which interface is used to broadcast the proxy ARP packets. If the subnet does not exist on an interface, proxy ARP will be broadcast out of all the interfaces.

Important note

Starting from PAN-OS 10.2.8, 11.0.5 and 11.1.0 this behavior was changed; the firewall will now apply strict Proxy ARP enforcement for source NAT, which means it will only send out proxy ARP for IP addresses in the same subnet as the ingress interface.

For destination NAT, the behavior remains the same.

Let’s take a look at some of the common, and a few special, use cases – NAT rule configurations.

### Hide NAT or one-to-many NAT

The most common implementation of outbound NAT is the infamous *hide NAT*, or many-to-one, which changes the source IP addresses of all internal clients to the external IP(s) of the firewall. It is best to place this rule near the bottom of the rule base as it will catch any non-specific sessions and rewrite the source IP to that of the firewall.

The best option for this type of NAT is **Dynamic IP and Port** (**DIPP**). DIPP rewrites the source IP to that of a selected interface or a manually entered IP, IP-range, or subnet, and assigns a random source port to the session on egress, as you can see here:

![](Images/B31589_03_52.png)

Figure 3.52: DIPP to an interface IP or manual selection

DIPP supports around 64,000 concurrent sessions per available source IP, multiplied by the oversubscription factor supported by the platform you are deploying these rules on. As a rule of thumb, small platforms commonly support 2x oversubscription, large platforms support 4x, and extra-large platforms up to 8x. When multiple IPs are available, DIPP assigns a rewrite IP based on a hash of the source IP so that the same source always gets the same translation address. Once the concurrent allowance for a given translation address is depleted, new sessions will be blocked until existing sessions are freed up.

You can check the current oversubscription ratio by using the following command:

```
admin@PA-220> show running nat-rule-ippool rule <rule name>
VSYS 1 Rule <rule name>:
Rule: <rule name>, Pool index: 1, memory usage: 20344
-----------------------------------------
Oversubscription Ratio:                2
Number of Allocates:                   0
Last Allocated Index:                  0
```

Starting from PAN-OS 11.1.1 and later, **Persistent Dynamic IP and Port** (**PDIPP**) allows you to bind the private source IPs to a specific translated IP so all sessions from the client are translated behind the same post-NAT IP. This helps tremendously with applications like STUN that open multiple subsequent connections and require all the connections to be sourced from the same IP.

If more than 64,000x oversubscription ratio concurrent sessions per source are needed, or source ports need to be maintained, you can opt to use Dynamic IP instead of DIPP. Dynamic IP will simply “hop” to the next available IP in its assigned translation addresses for a given source IP while maintaining the source port.

As a fallback, if the available IP pool does get depleted because Dynamic IP does not support oversubscription, you can enable DIPP. The IP used in the fallback should not overlap with any of the main IP pools:

![](Images/B31589_03_53.png)

Figure 3.53: Dynamic IP with two subnets and DIPP fallback

In some cases, a server or host on the network will need to “own” its own IP address, which can be achieved with one-to-one NAT rules.

### One-to-one NAT

Static IP will always translate a source into the same translation IP and maintain the source port. An IP range can be set, in which case the source IPs will be sequentially matched to the translated IPs, but it is important that the source range and translation range are identical in size; for example, `10.0.0.5-10.0.0.15` translates to `203.0.113.5-203.0.113.115`.

The **Bi-directional** option creates an *implied* inbound NAT rule to allow inbound translation for the same source/translated source pairs. This implied rule reuses the destination zone set in the rule and **any** as the new source zone. It will set the translated address as the new destination of the original source to the new translated destination.

For the outbound rule, as you can see in *Figure 3**.54* you have the following:

*   **Source**: **Trust-L3**
*   **Destination**: **Untrust-L3**
*   **Original source**: **serverfarm**
*   **Translated source**: **serverfarm-public**

For rules that have a bi-directional set, the following implied NAT rule will be created. This rule will *not* be visible in the rule base so be very attentive to any rules that are set to bi-directional and what this implies: the source is set to **any** so the implied rule may catch unintended sessions from other zones than the ones you configured:

*   **Source**: **any**
*   **Destination**: **Untrust-L3**
*   **Original destination**: **serverfarm-public**
*   **Translated destination**: **serverfarm**

![](Images/B31589_03_54.png)

Figure 3.54: Static IP NAT with the bi-directional option

In some cases, “double NAT” needs to be applied to sessions that need to take an unusual route due to NAT. These types of NAT rules are called U-turn or hairpin NAT rules.

### U-turn or hairpin NAT

If an internal host needs to connect to another internal host on the same subnet, by using its public IP address, a unique problem presents itself. For each session, only one NAT rule can be matched. When the client connects to the public IP, the routing table will want to send the packet out to the internet, which will trigger the hide NAT rule, which translates the source IP. (It will not trigger the normal destination NAT rule, as this will have **Untrust** as the source zone.)

The packet should then go back inside as the destination public IP is also owned by the firewall, but a second NAT action can’t be triggered, so the packet is discarded.

If the hide-NAT IP is identical to the destination IP, which is common in environments with few public IP addresses, the packet will be registered as a land attack:

```
admin@PA-220> show counter global | match land
Global counters:
Elapsed time since last sampling: 26.05  seconds
name     value  rate severity category  aspect  description
-------------------------------------------------------------
Flow_parse_land 1 1  drop     flow      parse    Packets dropped: land attack
```

A workaround to this problem, if changing the internal DNS record so the FQDN resolves to an internal IP while on the local network or adding an entry to the host file of the client is not possible, is to configure a U-turn or hairpin NAT.

**Important note**

If you are using PAN-OS 9.0.2 or later, take a look at an alternative solution in the *Enable DNS Rewrite* section.

This type of NAT combines the destination and source NAT and must be placed at the top of the rule base to prevent the hide NAT rule from catching these outbound sessions. The reason the source NAT is required is to make the session stick to the firewall so that no asymmetric routes are created.

If you were to configure the destination NAT to rewrite the public IP for the internal IP without translating the source, the server would receive a packet with the original source IP intact and reply directly to the client, bypassing the firewall. The next packet from the client would be sent to the firewall, which would try to perform TCP session sanity checks and determine whether the TCP session was broken, discarding the client packet.

Adding source translation would force the server to reply to the firewall, which would then forward the translated packet back to the client:

![](Images/B31589_03_55.png)

Figure 3.55: U-turn NAT

This type of complication can also be addressed by changing the DNS query to the internal IP of the final destination.

### Enable DNS Rewrite

The **Enable DNS Rewrite** option  was introduced in PAN-OS 9.0.2 and later, and enables the NAT policy to be applied inside DNS response packets:

*   It reverse translates the DNS response that matches the *translated* destination address in the rule. If the NAT rule rewrites `198.51.100.2` to `10.0.0.5`, the reverse rewrite will change the DNS response of `10.0.0.5` to `198.51.100.2.`
*   It forward translates the DNS response that matches the *original* destination address in the rule. The forward DNS rewrite changes the DNS response of `198.51.100.2` to `10.0.0.5`.

This could be useful in a scenario where internal hosts need to query a DNS server in the DMZ for an FQDN of a server also hosted in a DMZ where they receive the external IP in the DNS response. This could lead to odd routing issues (see the *U-turn or hairpin NAT* section) as the destination IP will match the external zone, but both the client and server are on internal zones:

![](Images/B31589_03_56.png)

Figure 3.56: Enable DNS Rewrite

If a service is hosted on several physical servers (the original destination is an FQDN that returns several IP addresses), the destination translation settings can be set to **Dynamic IP**  (**with session distribution**). The firewall will rewrite the destination IP according to the chosen method:

![](Images/B31589_03_57.png)

Figure 3.57: Dynamic IP (with session distribution)

This is how these session redistribution methods work:

*   **Round Robin** assigns a new session an IP in rotating order.
*   **Source IP Hash** assigns new sessions an IP based on a hash of the source IP. The same source IP will always connect to the same destination, so if there’s a proxy, for example, or a NAT gateway, between the clients and the firewall, do not use this method.
*   **IP Modulo** assigns new sessions an IP based on the source and destination IP, performing an XOR and a modulo operation.
*   **IP Hash** assigns new sessions using a hash of the source and destination IP.
*   **Least Sessions** assigns new sessions to the IP addresses that have the least number of active concurrent sessions in the session table.

With this information, you will now be able to resolve any NAT challenges you may face.

# Summary

In this chapter, you learned how to create security profiles and how to build a set of profiles that influence how your firewall processes threats. You learned how to create security profiles that leverage best practices and can add these to a default security profile group so that your security rule base starts off with a strong protection stance. You are also able to create complete security rules that leverage reusable objects, have easy-to-identify tags, and are set to allow all desirable access based on application identification rather than ports. You can now make complex NAT policies that cater to the needs of your inbound and outbound connections.

If you’re studying for the PCNSE, take specific note of how the best practice security profiles are set with reset-both and single-packet packet capture for critical, high, and medium severity, while low and informational are set to default with no packet capture. Remember how zones play an important role in the original packet tab of NAT rules. Remember the importance and implications of App-ID with application-default and how application filters can be used to define behavior rather than needing to account for all applications manually.

In the next chapter, we will see how to take even more control of your sessions by leveraging policy-based routing to segregate business-critical sessions from the general internet, limit bandwidth-hogging applications with quality of service, and look inside encrypted sessions with SSL decryption.