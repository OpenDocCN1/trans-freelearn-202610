
- **Weaponization:** The second stage in the Cyber Kill Chain is the weaponization stage. After successfully gathering information, adversaries use the collected data to prepare the attack. Depending on the objectives and methods that have been employed, they are likely to weaponize legitimate documents, create malware, and develop undetectable trojans. The *capability* is expected to be there since well-engineered malware, payloads, and exploit creation tools to carry advanced system attacks are available. *Only a few skilled adversaries can program their weapons to compromise systems.* However, with *Threat-as-a-Service*, even less skilled attackers can purchase state-of-the-art weapons and launch devastating attacks. The intelligence team, along with the internal security, must have clear visibility of this phase and implement methodologies to detect weaponized resources (including methods such as malware analysis and scanning, intrusion detection, and so on).

![](img/d47f26598025b2da017dd41fcbb6e602_84_0.png)

Figure 3.3 – Lockheed Martin's Cyber Kill Chain (https://lmt.co/3iHGjLl)

- **Delivery:** The adversary is ready to attack. The delivery stage involves techniques used to convey the malicious file(s) to the target. The framework breaks the delivery stage into two styles: adversary-controlled delivery (where the adversary manages the delivery; for example, using public-facing servers) and adversary-released delivery (which includes practices such as phishing, planting USB sticks, and so on).

The intelligence team develops ways to block intrusions. This is an essential step of the chain. If an adversary passes this stage, they are likely to affect the target system operation, even if they are detected in later stages.

- **Exploitation:** The exploitation phase is where the adversary gains access to the victim. They take advantage of system vulnerabilities to get into the system. The techniques used at this stage include exploiting vulnerabilities in software, hardware, and humans (social engineering). They can also use *zero-day exploits* or leverage *unpatched vulnerabilities* to gain access. Other methods involve opening malicious attachments or navigating malicious web links. The security team must develop countermeasures to prevent or stop the attack.
- **Installation:** Unless the operation is a one-off task, adversaries are likely to install persistent backdoors for future intrusions. This helps them maintain access for as long as they want in the victim system. The intelligence team must develop ways to detect backdoors (using a methodology such as endpoint instrumentation). Some techniques used by the adversaries at this stage include changing the registry, adding environment variables, or modifying legitimate processes to achieve persistence.
- **C2:** Once a backdoor has been planted in the victim, an adversary finds techniques to communicate with it and take control. The Cyber Kill Chain describes techniques that threat actors use to create communication with backdoors. Hence, the defense team can use countermeasures to block the attack. C2 is the last stage where the intelligence team and the defense system can take action – after this, only tears remain.
- **Actions:** At this stage, the adversary can execute any action on the target system, from collecting sensitive information to denying services to the users. The framework enumerates some of the most used adversarial actions, including privilege escalation, lateral movement, internal reconnaissance, collecting and exfiltrating data, system destruction, system modification, and data corruption. At this stage, the defender's objective is to detect the action rapidly to avoid further damage. Forensics methods are used at this stage to gather pieces of evidence on the breach (incident response).

The Cyber Kill Chain model uses a comparative method of the adversary tactic and the possible defensive stand. For each stage, it provides techniques to defend against the corresponding adversarial movement.

## Use case – Lockheed Martin's Cyber Kill Chain model mapping

**Scenario and objective:** A group of hackers has decided to take down the ABC insurance company after voting for and passing policies that customers are unhappy with. Their sole objective is to take down the business for as long as they want in order to push the company to remove the policies.

The group harvests email addresses, domain information, IP ranges, and SSL certificate information from the public internet. They come across the email address of Vanessa, the IT admin, on LinkedIn and her life routine on Instagram. They also discover the company router that faces the internet and the company's website, where users can register for insurance, contact the company, and leave comments.

- Cyber Kill Chain mapping: *Reconnaissance.*
  **Countermeasure:** Continuously collect website access logs for search activities, collect traffic analytics from web administrators, and detect unique and suspicious recurrent browsing behavior.
  The group selects the **Metasploit Framework (MSF)** as their tool to generate a set of malware embedded in a document to send to Vanessa.
- Cyber Kill Chain mapping: *Weaponization.*
  **Countermeasure:** Know the most used malware generation platforms, perform malware analysis, and identify and detect malware payload types and their origins.
  The group is ready to use two techniques to compromise the system. They send an email to Vanessa and also prepare a USB stick with the prepared weapon that will physically be installed on Vanessa's personal computer.
- Cyber Kill Chain mapping: *Delivery.*
  **Countermeasure:** Understand the upstream infrastructure, understand the people and their roles, understand what information can be accessed by which user, and frequently perform forensics reconstruction.
  Paul is a member of the group. He befriended Vanessa a month before and has been able to visit her at her place. One of the ways to deliver malicious code is for Paul to convince Vanessa to open the document on the USB stick. Alternatively, Paul can send an email with the attachment to Vanessa and she will open it.
- Cyber Kill Chain mapping: *Exploitation.*
  **Countermeasure:** Train the employees on social engineering practices, frequently perform vulnerability scanning, perform endpoint hardening testing, and perform forensics endpoint auditing often.
  The malicious file spreads in the company system when Vanessa connects to the corporate network. The weapon installed a backdoor on the system and achieved persistence.
- Cyber Kill Chain mapping: *Installation.*
  **Countermeasure:** Install intrusion prevention devices, frequently audit registries and processes, and audit endpoints for abnormal file generation.
  Paul and his crew have established a reverse connection with the planted backdoor and can control the ABC insurance system.
- Cyber Kill Chain mapping: *Command and Control.*
  **Countermeasure:** Harden network communication (use proxies), scan for C2 networks, and monitor unusual ports.
  The group has collected user credentials, performed pivoting to the database network, and exfiltrated data. Finally, a DDoS attack is launched to take down the company with a message saying, *Remove the new policy, or we will burn this company to the ground.*
- Cyber Kill Chain mapping: *Action.*
  **Countermeasure:** Perform incident response, scan and identify exfiltration tasks, and scan for access to sensitive databases or repositories.

The use case described here provides a global overview of how a cyber event can be mapped to the Cyber Kill Chain as well as the possible countermeasure techniques that can be used to break the attack chain. These countermeasures are not exclusive. There are several methods we can use that are detailed by the framework and the industry. The CTI team must be able to advise on effective industry-leading countermeasures for each stage of the kill chain. Now, let's look at how Lockheed Martin's Cyber Kill Chain can be integrated into the intelligence product or platform.
