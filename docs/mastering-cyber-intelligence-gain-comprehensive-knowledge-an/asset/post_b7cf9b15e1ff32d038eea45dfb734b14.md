
## Naming adversary campaigns

No universal standard or regulation controls the naming of adversary campaigns or threat actor groups. Organizations name groups based on their conventions and criteria. As mentioned earlier, the Russian group APT29 has different names in different organizations. Threat intelligence expert organizations, such as CrowdStrike, FireEye, Microsoft, and more, use certain conventions to name threat actors. However, there are best practices that a CTI analyst can build upon. Some of the DON'Ts in naming adversaries include the following:

- **The use of tools**: It is not recommended that you use tools, techniques, or procedures to name a threat actor campaign or group, as these can change or be shared by different groups.
- **The use of a nation name**: It is not recommended that you use the nation where the group is based because that can also change.
- **The use of a major external incident**: This is also not recommended. An organization can be tempted to assign a breach name to a campaign or threat group. But what happens if that group succeeds in pulling another breach? Hence, an incident name should not be used as a campaign or group name.

It is highly recommended that you do not use names that can change with time when naming campaigns or groups. Instead, it is advised that you use names that are easy to track and remember but that are unique. Names should not be too revealing either.

FireEye's Mandiant (https://bit.ly/317dnrM) uses APT with numbers as names for threat groups, and it is one of the most popular namings. Examples include the Russian groups APT29 and APT28, the Iranian group APT39, and the Chinese groups APT1, APT2, APT3, and APT19.

However, you do not have to copy those names. The CTI team can use their inspiration to find suitable names for adversary campaigns as long as they are easy to track and correlate with external intelligence namings.

## Advanced persistent threats (APTs) – a quick overview

When we talk about **advanced persistent threats (APTs)**, we refer to the sophistication of cyberattacks. APTs employ advanced techniques to compromise even the most secure IT infrastructures. In the past, they were mostly related to state-sponsored threat actors. However, now, threat actors with the relevant resources can orchestrate stealth attacks. Some of the characteristics of APTs are as follows:

- APT targets are primarily *high-value targets* and *large corporates*, including but not limited to *governments* and *major business organizations* in diverse fields such as finance, health care, telecommunication, technology, transport, manufacturing, and more. However, small and medium businesses should not take APTs lightly.
- Their motivation includes and is not limited to *data theft*, *espionage*, and *operations disruption*. In most cases, APTs steal sensitive information, classified information, or disrupt the system to feed some agendas.
- APT attacks are always persistent – *over a long period*. The actors intend to steal data and remain in the system for a long time, allowing them to initiate other attacks – ensuring continuity in their operations.
- Some of their widespread attack methods include the use of *social engineering* through phishing campaigns, *infiltration* (insiders), and *human intelligence* (for example, an employee who gives out critical system information). With resources at their disposal, APTs can also leverage *system vulnerabilities* and *zero-day exploits*.

Organizations must always be on alert when it comes to APTs. An advanced persistent attack is achieved through a set of steps, as shown in the following diagram:

![](img/d47f26598025b2da017dd41fcbb6e602_444_0.png)

Figure 14.10 – Standard APT attack phases

The preceding steps are explained in the following points:

- 1. **Gaining initial access**: APT actors will try to infiltrate your organization by employing various techniques such as spear-phishing, phishing, and exploiting zero-day vulnerabilities. This step is the optimal point of stopping the attack. Past this stage, stopping APTs becomes tricky.
- 2. **Establishing a foothold**: Once in your system, APT actors install backdoors (or a network of backdoors) and create tunnels to quietly interact with the compromised system. They use advanced methods such as polymorphism and metamorphism to make malware dynamic and cover tracks.
- 3. **Escalating privileges**: After establishing a foothold, the APT actors use several methods to acquire the administrator’s rights. They use exploits, process injections, modifications, and many more techniques, to escalate privileges.
- 4. **Performing lateral movement**: Once the APT actors have administrator access, they can extend the attack to other systems (such as servers, workstations, and third-party networks).
- 5. **Learning the system:** The APT actors perform an internal reconnaissance, learn about the system, and collect any information required to achieve their goal. They pinpoint the vulnerabilities that they can exploit.
- 6. **Achieving persistence:** An APT's particularity (compared to regular black hacks) is the ability to remain inside the compromised system for a long time (until discovered). An APT actor remains in control of the system even after restarting. They can install more than one malware to ensure continuity.
- 7. **Performing the course of action:** Finally, the APT actor can achieve their intended goals: exfiltrate sensitive data.

Organizations must always be on the lookout for APTs. They are not simple to mitigate, especially if the actor has established a foothold. The challenge is justified by the use of multiple backdoors, increasing their chance of regaining access to your system. Protecting against APTs requires more than just monitoring, using antiviruses, intrusion detections, intrusion prevention, and so on because the stealthiness of these attacks is optimal. Threat intelligence is an effective way in which to combat APTs.

## Tracking threat actors and groups

Tracking threat actors (especially APTs) can be challenging, especially for level-1 organizations (entry-level to CTI). Several companies continuously track them and publish their information. Hence, for level-1 and some level-2 organizations, it is recommended that you use public frameworks as a starting point for monitoring adversary groups and campaigns. Some of the popular open-source platforms to track and enhance your threat intelligence with campaigns and groups include the following:

- **The MITRE group tracker:** MITRE tracks adversary activities in groups, threat actors, intrusion sets, and campaigns. It is one of the most used threat actor tracking frameworks. Most well-known threat groups can be found on the MITRE platform (https://attack.mitre.org/groups/). It references most of the well-known naming conventions. Therefore, your organization can map analyzed threats with other names to track adversary campaigns.
- **The CrowdStrike adversary universe:** The CrowdStrike adversary universe provides adversary information grouped by industries and targeted countries. You can monitor the relevant groups based on your business area and country (https://adversary.crowdstrike.com/en-US/).
- **The Mandiant APT group insight:** The Mandiant APT group insight provides a list of APTs and threat actors that can be used to track adversaries. Mandiant uses the popular numbering format of APT1, APT2, APT28, and more (https://www.mandiant.com/resources/apt-groups).

Commercial platforms for tracking adversaries and campaigns also exist. Depending on your organization's CTI level and budget, you can select open source or commercial platforms. You must ensure that the tracker is constantly updated – this is a big part of understanding the enemy. Some modern SIEM tools and TIPs include threat actor tracking.

## Retiring threat intelligence and adversary campaigns

Nothing runs forever, and CTI and adversary campaigns are no exception. There will come a time when the intelligence team will have to evaluate the usefulness of the program and the dissolution of an adversary campaign. Retiring the entire program is different from making a campaign absolute. However, both might have business impact consequences, which must be considered before retiring any of them. The two main questions that analysts ask when it comes to CTI are When do we stop the CTI program? And When do we stop tracking an adversary campaign?

## Rethinking the CTI program

A CTI program is helpful to the organization as long as it strengthens the security posture and optimizes security operations. When the program becomes stagnant, its existence needs to be reevaluated. However, when discussing detasking or rethinking threat intelligence, we usually refer to detasking a specific course of action (one or more of the 7 Ds of the kill chain model). Let's take an example of the discover and detection courses of action.

The CTI program helps detect threats and discover intrusions through IOCs and IOAs. This analysis is then shared with different teams, such as forensics teams, incident response teams, and malware analysis teams. The CTI program and courses of action should be reevaluated when the following occurs:

- **It misleads the SOC:** When the SOC relies on threat intelligence, the latter must be practical and accurate. If its output misleads or is not valuable to the SOC, it should be retired and rethought.
- **It produces a lot of false positives:** False positives are an analyst's worst nightmare as they waste time. Analysts must perform system checks to identify the causes of false alerts. Resources deployed on false alerts are not taken back. Therefore, when the SIEM tool reports a lot of false positives, it is essential to detask the course of action (detection) and reevaluate the program.
