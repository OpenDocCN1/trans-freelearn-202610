
- **Support all endpoint types**: Endpoint management must cover all types of devices that can connect to a network (laptops, printers, smartphones, smart devices, tablets, electronic boards, industrial machines, and so on). In addition, it must cover physical and virtual endpoints as well.
- **Support all operating systems**: While Windows remains the most popular operating system, an effective endpoint management system must cover all operating systems (Windows, Linux, macOS, Android, iOS). It must also be tailored to address customized and personalized operating systems or software.
- **Facilitate patch management**: Ensure that software patches can be enforced using the system and that all major patches are covered.
- **Maintain user flexibility**: While the endpoint management system enforces policies and standards on network devices, it must not compromise users' ability to do their work on their devices. While the main objective remains to tighten security at a lower possible cost, operations must remain efficient.
- **Identify and alert on non-compliance**: Ensure that the endpoint system identifies devices that are not compliant with your organization's policies and industry regulations. At the extreme, it must automatically address the compliance problem or alert the security or IT team.
- **Integrate with other platforms**: The idea of developing an effective defense is to avoid different security components working in silos. Hence, the endpoint management system must have the capability to seamlessly integrate with other IT systems such as SIEM, TIP, monitoring systems (using an **application programming interface (API)** or custom connections). In addition, the SIEM or TIP must work as a centralized system for all security platforms.

Investing in an endpoint management system can be costly; therefore, the cost factor must always be counted, especially for organizations with limited budgets. An organization can acquire more than one endpoint management system, depending on the functionalities and requirements. When more platforms are used, ensure that integration and interoperability can be upheld to facilitate maintenance and support. The following section looks at mobile endpoint management and how this differs from and challenges an endpoint management system.

## Mobile endpoint management

Mobile device usage is growing exponentially and has overtaken the usage of static devices such as laptops, printers, and so on. Mobile device architecture, daily usage, and ability make it more challenging for an endpoint system. Imagine an employee who uses their phone to access the corporate network and at the same time downloads games and music, accesses torrent sites, and clicks on random links when browsing the internet. Their device becomes a bridge between the attackers and the corporate network. Let's look at probable mobile device security threats.

## Mobile security threats and countermeasures

Security threats on mobile devices can differ from traditional endpoints due to the architecture adjustment. For example, interaction with the kernel is not direct with mobile devices compared to laptops. Instead, the user needs to go through various applications to interact with the device operating system. Therefore, we can divide mobile security threats into three categories, as follows:

- **Kernel-based threats**: These threats happen when there is an intentional or unintentional change in the operating system's functionalities (permissions, root applications, and so on). For example, rooting mobile endpoints or jailbreaking to access the operating system core makes devices vulnerable to cyber attacks. *Therefore, you must enforce policies that restrict rooted (or jailbroken) mobile devices' access to the network.*
- **Application-based threats**: Phishing applies to mobile devices too, and mobile applications have different levels of access to resources. If you grant sensitive data access to weaponized applications, not only does your device become a victim, but any network you connect to is then at risk. Therefore, you must *enforce policies on how users manage and grant permissions to applications* (this can be through education). The system must identify *mobile devices that pose security risks* (get their **identifiers (IDs)**, **International Mobile Equipment Identity (IMEI)**, model, manufacturer, and so on). The system should be able to *block such devices if the risk is high.*
- **Social engineering and network threats:** Social engineering will always be one of the most effective attacks, even where mobile devices are concerned. Downloading malicious files and entering credentials on cloned sites can give access to attackers. Another way to access devices is through network attacks. An attacker can collect information from endpoints by gaining access to the corporate wireless network (using a **MITM attack**, for example). Therefore, you must *educate employees on security best practices and enforce LAN and WLAN network protection (Wireless Protected Access 2-Enterprise (WPA2-Enterprise) or the latest WPA3 encryption)*. When open access points are used, ensure that employees cannot pivot to access other networks.

Mobile devices are constantly under attack. Organizations must implement policies that support secure endpoint behavior in the system, such as the use of VPNs and the restriction of certain sites while in the organization's network. A reliable management system can create an effective defense. In the next section, we look at a use case where endpoint management failed.

## Endpoint data breach use case – point of sale

**Point of sale (POS)** is used everywhere to make payments (stores, restaurants, hotels, parking, and so on), and it has become the target of many attackers as it directly connects to banking infrastructures. Hence, there is one objective in targeting POS: *credit card data theft*. To ensure that POS is protected, all organizations using online payment must comply with the PCI DSS. A typical POS communication is shown in the following diagram:

![](img/d47f26598025b2da017dd41fcbb6e602_277_0.png)

Figure 8.9 – Basic POS architecture and data flow

POS is an endpoint. Therefore, it has an operating system, memories, processing units, and physical peripherals that facilitate network connection and human interaction. Although all security measures are considered (such as encrypting all communications), malicious software still targets POS terminals. Let's analyze the Home Depot POS data breach of 2014, as follows:

**Victim company:** Home Depot, Inc.

**Country:** United States (US)

**Breach type:** POS attack

**Time taken to discover the breach:** 5 months (April to September 2014)

**Description:** Home Depot was a victim of one of the biggest data breaches. Attackers gained access to the POS network and stole data on 56 million credit cards.

**Attack surface and vector:** (1) The attackers *stole credentials* from Home Depot's third-party vendor. (2) They used the credentials to gain access to the vendor's system or network. (3) They exploited a *Windows zero-day vulnerability* to pivot from the vendor's system to the Home Depot corporate network. (4) They installed *memory-scraping malware* on more than 7,000 POS terminals. (5) The malware stole information relating to 56 million credit cards and around 53 million email addresses.

**Loopholes in Home Depot security:** Home Depot failed to detect the attacker sooner and prevent it from happening in the first place. Although they had Symantec Endpoint Management, we can see that specific security measures and procedures were not in place, as follows: (1) *Vulnerable POS terminals' software and hardware.* (2) *Poor vulnerability management program.* They should have been able to detect the breach if vulnerability assessment had been executed frequently. (3) *Poor access management* between vendors and Home Depot networks. (4) *Poor credential management* from both the vendor and Home Depot. (5) *Poor (or lack of a) data/network monitoring* solution, introducing a delay in the response. (6) *Poor network design.*

**Probable prevention:** Measures that could have prevented the attack include the following: (1) *Efficient POS terminal configuration policies* to enforce secure practices in software and hardware. (2) Use of *point-to-point encryption (P2PE)* to encrypt payment data in POS terminal memory. (3) *Operating system upgrade*, since Home Depot terminals were running Windows XP Embedded **Service Pack 3 (SP3)** at that time. (4) *A good vulnerability management program.* (5) *Segmenting corporate networks* using technologies such as **virtual LANs (VLANs)**. (6) Properly managing third-party access to sensitive data. (7) Having a *sound monitoring system* that could feed to a SIEM or TIP (having a *TI program* that could allow a centralized actionable defense; a TI program could have shown them gaps in the security infrastructure).
