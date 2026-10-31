
1. [The Apps Seen column](#B31589_03_Epub.xhtml#_idParaDest-84)
4. [Creating NAT rules](#B31589_03_Epub.xhtml#_idParaDest-85)
   1. [Inbound NAT](#B31589_03_Epub.xhtml#_idParaDest-86)
   2. [Outbound NAT](#B31589_03_Epub.xhtml#_idParaDest-87)
      1. [Hide NAT or one-to-many NAT](#B31589_03_Epub.xhtml#_idParaDest-88)
      2. [One-to-one NAT](#B31589_03_Epub.xhtml#_idParaDest-89)
      3. [U-turn or hairpin NAT](#B31589_03_Epub.xhtml#_idParaDest-90)
      4. [Enable DNS Rewrite](#B31589_03_Epub.xhtml#_idParaDest-91)
5. [Summary](#B31589_03_Epub.xhtml#_idParaDest-92)

# Landmarks

1. [Cover](cover.xhtml)

# Mastering Palo Alto Networks: The complete journey to firewall mastery from setup to advanced security

**Welcome to Packt Early Access**. We're giving you an exclusive preview of this book before it goes on sale. It can take many months to write a book, but our authors have cutting-edge information to share with you today. Early Access gives you an insight into the latest developments by making chapter drafts available. The chapters may be a little rough around the edges right now, but our authors will update them over time.You can dip in and out of this book or follow along from start to finish; Early Access is designed to be flexible. We hope you enjoy getting to know more about the process of writing a Packt book.

1. Chapter 1: Understanding the Core Technologies
2. Chapter 2: Setting Up a New Device
3. Chapter 3: Building Strong Policies
4. Chapter 4: Taking Control of Sessions
5. Chapter 5: Services and Operational Modes
6. Chapter 6: Identifying Users and Controlling Access
7. Chapter 7: Managing Firewalls through Panorama
8. Chapter 8: Managing firewalls through Strata Cloud manager
9. Chapter 9: Upgrading Firewalls and Panorama
10. Chapter 10: Logging and Reporting
11. Chapter 11: Virtual Private Networks
12. Chpater 12: Advanced Protection
13. Chapter 13: Troubleshooting common session issues
14. Chapter 14: A Deep Dive into Troubleshooting
15. Chapter 15: Cloud based firewall deployments

# 1 Understanding the Core Technologies

Welcome to the first chapter! In this book, we're going to explore the ins and outs of the Palo Alto Networks Strata suite. We'll start off by learning about all the different features of the firewall and how to configure them before we move on to more complex features and additional services that will help you complete your deployment. On the way, you'll pick up important knowledge nuggets that will help you both understand the technology and pass the `PCNSE` exam.

In this chapter, we're going to examine the core technologies that make up the Palo Alto Networks firewall. We are going to take a closer look at the way in which security zones control how security, `Network Address Translation` (`NAT`), and routing verdicts are made. We will review the mechanics behind `App-ID` and `Content-ID` so you get a deeper understanding of how packets are processed and security decisions are made by the firewall, and we will review how `User-ID` contributes to a more robust security stance by applying group-based or user-based access control.

This chapter will cover the following topics:

* Understanding the zone-based firewall
* Understanding `App-ID` and `Content-ID`
* The management and data planes
* Authenticating users with `User-ID`

By the end of this chapter, you will have a better understanding of how the core technology is built up and will be able to apply these skills when we start building configuration. If you're preparing for the `PCNSE` exam, this chapter will also help you understand the fundamentals required to tackle some of the scenario-based questions.

## Technical requirements

For this chapter, no physical installation is required; technology is only explained. A good understanding of basic networking protocols like `UDP` and `TCP` is necessary to fully benefit from the following materials. It is helpful if you've already worked with Palo Alto Networks firewalls, but it is not required. Some experience with firewalls or web proxies in general is recommended, as this will make the subject matter more tangible.

## Understanding the zone-based firewall

Traditionally, when considering a firewall as an element of your network, most likely you will imagine a network design like the one in *Figure 1.1*, with two to four areas surrounding a box. Most of the time, whatever is placed in the north is considered dangerous as it represents the internet, the east and west are somewhat gray areas as they are the `demilitarized zones` (`DMZs`) that are partly exposed to the internet, and the south is the happy place where users do their daily tasks. The box in the middle is the firewall. All these areas will be defined as zones in the firewall:

![](Images/B31589_01_01.png)

Figure 1.1: Basic network topology

In reality, a network design may look a lot more complex due to network segmentation, remote offices being connected to headquarters via all sorts of different technologies, and the adoption of cloud vendors.

In a `route-based firewall`, zones are simply an architectural or topological concept that helps identify which areas comprise the global network that is used by the company; they are usually represented by tags that can be attached to a subnet object. They hold no bearing in any of the security decisions made by the system when processing security policies.

The `zone-based firewall`, on the other hand, will use zones as a means to internally classify the source and destination in its state table.

The following diagram illustrates the phases of packet processing from the first step when the first packet of a new session enters the firewall to the last step where the packet egresses the firewall:

![](Images/B31589_01_02.png)

Figure 1.2: Phases of packet processing

Let's look at the process workflow for initial packet processing:

1. When a packet is first received, a source zone lookup is performed. If the source zone has a protection profile associated with it, the packet is evaluated against the profile configuration. If the first packet is a `TCP` packet, it will also be evaluated against the `TCP` state where the first packet needs to be a `SYN` packet, and a `SYN` cookie is triggered if the protection profile threshold is reached.
2. Then, a destination zone is determined by checking the `policy-based forwarding` (`PBF`) rules and, if no results are found, the routing table is consulted.
3. Lastly, the `NAT` policy is evaluated as the destination IP may be changed by a `NAT` rule action, thereby changing the destination interface and zone in the routing table. This would require a secondary forwarding lookup to determine the post-`NAT` egress interface and zone.

After these zone lookups have been performed in the `initial packet processing`, the firewall will continue to the `security pre-policy` evaluation.

In the pre-policy evaluation, the "`6-tuple`" is used to match an incoming session against the rule base before establishing or dropping/denying a session. At this stage, the firewall does not consider the application just yet, as this can usually not be determined by the first packet in a session. The six-tuple consists of the following elements:

* Source IP
* Source zone
* Destination IP
* Destination zone
* Destination port
* Protocol

Zones are attached to a physical, virtual, or sub-interface. Each interface can only be part of one single zone. Zones can be created to suit any naming convention and can be very descriptive in their purpose (`untrust`, `DMZ`, `LAN`, and so on), which ensures that, from an administrative standpoint, each area is easily identifiable.
