
## Physical security
- **Physical security**: Physical security remains an often forgotten part of ICS (cyber) security. However, the truth is that if an attacker makes it into the ICS environment, there is no end to the harm he or she can do.

## Vulnerabilities in outdated OSs, software, PLC/IO/controller firmware, and the applications that run the ICS processes (think HMI/PLC programs)
- **Vulnerabilities in outdated OSs, software, PLC/IO/controller firmware, and the applications that run the ICS processes (think HMI/PLC programs)**: Because of the several reasons we discussed in the previous section, ICS firmware, software, OSs, and applications are often not updated regularly. This allows an attacker to use exploits that are often completely obsolete on the regular IT network. As an example, the Conficker worm that made the headlines in late 2008 (https://antivirus.comodo.com/blog/comodo-news/conficker-worm/) can still be found on some ICS environments, mainly because the underlying vulnerability (MS08-076) that allowed the worm to propagate was never fixed. And don't get me started on WannaCry (MS17-010).

## Malware
- **Malware**: Along the lines of the previous risk category, malware is devastating to the ICS environment. Not only can malware thrive in the ICS's unpatched network infrastructure, but it will also cause significant damage while running rampant. Ransomware takes the crown in this category, with damages in excess of 10s of millions of dollars starting to be the norm.

## Sniffing, guessing, and cracking of passwords
- **Sniffing, guessing, and cracking of passwords**: With the implementation of lax security policies, outdated software, and a lack of encryption on ICS networks, sniffing, guessing, and cracking password hash dumps is a lucrative business for an attacker.

## Social engineering
- **Social engineering**: As we discussed in the previous section, ICS operators are more susceptible to social engineering attacks, just because their main focus is not cybersecurity but getting their product out the door as efficiently and as cost effectively as possible.

A well-planned penetration testing exercise will cover all these risk categories and will follow a methodical approach, based on real-world attack scenarios. In the next section, we will discuss how we can add real-world attack scenarios to our pentest efforts by modeling them for the ICS Kill Chain methodology.

## Modeling pentests around the ICS Kill Chain

As we discussed in the previous section, ICS environments are ideal targets for attackers with inherent risks associated with the uniqueness of the ICS environment. Because of these unique characteristics and the architecture of ICS environments, serious attackers will follow a unique approach to attack the industrial network (kill chain). It makes sense to model a pentest around this unique attack approach. One of the best writeups detailing this attack approach comes in the form of the industrial control system Cyber Kill Chain, as presented by the SANS institute, which can be found here: https://www.sans.org/reading-room/whitepapers/ICS/industrial-control-system-cyber-kill-chain-36297.

Let's start our discussion by explaining the Cyber Kill Chain.

### The Cyber Kill Chain explained

The concept of the Cyber Kill Chain was first created by analysts of the military defense corporation Lockheed Martin. They published a paper in 2011 describing the concept, which they initially called the **Intrusion Kill Chain**. The purpose of the Intrusion Kill Chain concept is to help with the decision-making process during potential (physical) attacks or intrusions.

In the realm of cybersecurity, the Intrusion Kill Chain is known as the **Cyber Kill Chain**.

### The Intrusion Kill Chain

The Intrusion Kill Chain describes a process that is executed against a target with the intention of producing a desired effect (objective). It is portrayed as a chain of events because it is made up of a series of execution stages, where interrupting or mitigating any execution stage would result in breaking the chain and stopping the attacker from obtaining its objective.

The Intrusion Kill Chain describes each step or stage in an advanced attack by means of a seven-step sequence of procedures. The chain approach makes identifying and understanding each stage of the attack easier. The following diagram shows the seven Intrusion Kill Chain stages:

![](img/c8ffa9959721097701584fcc95101362_660_0.png)

Figure 17.1 – Intrusion Kill Chain stages

The model was designed with corporate environments in mind and as such, it is not totally fitting to apply to the ICS environment, mainly due to the nature of these environments and the attacks targeting them. The following section will describe an adapted model, specifically targeting the ICS environment.

# The ICS Cyber Kill Chain

Due to its unique features and deployment, the ICS requires considerable knowledge about the target's industry – the ICS environment – to be able to carry out a successful attack. These unique challenges of an industrial control system require the attacker to avoid interfering with the multitude of sensors and controls and automation devices while performing the attack, as well as being able to pivot through multiple layers of networks that are usually found in such environments (the internet to enterprise networks, to the industrial network to enclaves, and so on).

To put these unique challenges into perspective, the SANS Institute (https://www.sans.org/about/) published a report in 2015 that adapts the Cyber Kill Chain to industrial control system environments. This report expands upon the original Intrusion Kill Chain stages by, among other things, dividing the stages into two distinct phases, with the purpose of articulating the ICS characteristics. The following diagram shows the first phase of the ICS Kill Chain:

![](img/c8ffa9959721097701584fcc95101362_661_0.png)

Figure 17.2 – Phase 1 of the ICS Cyber Kill Chain

When the attacker has successfully compromised the target, phase 1 of the ICS cyberattack is considered complete. The attack will continue with the second phase. Note that in some cases, phase 1 of an attack is not implemented. This can be the case where access to the industrial network is gained some other way, such as through an internet exposed ICS system or device or if the attacker comes in through a supply chain compromise, such as a breached vendor or engineering/support company.

The following diagram shows the second phase of a typical ICS cyberattack:

![](img/c8ffa9959721097701584fcc95101362_663_0.png)

Figure 17.3 – Phase 2 of the ICS Cyber Kill Chain

In the second phase, knowledge that was collected during the first phase of the attack is used to prepare the attack on the ICS environment. This phase could follow directly after successfully completing phase 1, but a delay between phases is possible.

Typically, the following stages are part of the second phase of the Cyber Kill Chain:

- **Planning**: During the planning stage of the second phase, the attackers will plan the attack path, choose and define any tools and exploits that will be used, and generally try to make sure they develop their capabilities so that they successfully complete the unique challenges for the target’s ICS environment.
- **Validation**: During the validation stage, the attacker will certify the (new) capabilities required for the ICS attacker in an identical or similar environment as the victim’s ICS setup. The idea is that by practicing the attack on a simulation of the victim’s environment, the attackers get the chance to test, plan, prepare, and practice the attack, with the hope to avoid mistakes that can prevent them from reaching their objectives. This is a great challenge for the attackers, since simulating an entire system such as those used in ICS environments is a highly complex and time-consuming task.
- **ICS attack**: This is the final stage of the ICS Cyber Kill Chain. During this stage, the attackers will perform the ultimate objective of the entire cyberattack. This is what the attack is all about. The objective can vary from stealing secret information, disrupting the production process, industrial espionage, hacktivism goals, or even more sinister objective such as causing damage or harm to the environment or employees.

With that, we’ve explained the ICS Cyber Kill Chain, detailing how attackers go through distinct attack phases and stages to reach a certain objective. Next, we will learn how to use this model while performing penetration testing engagements.
