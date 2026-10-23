+   - **Honeypots**: Honeypots are deliberately enticing (decoy) assets deployed to look like valuable targets and intentionally designed to be easy to breach. Honeypots allow the blue team to detect and analyze attacks and often are the first to identify new exploits. They also help the blue team to better understand how attackers are going about attacking and compromising the honeypot machines, which in turn allows better protection of the real production systems the honeypot was based on.

Blue team assessments will be discussed in detail in *Chapter 16, Red Team/Blue Team Exercises*. The next type of assessment we are going to talk about is penetration testing.

# Penetration testing

Before we start our conversation around penetration testing, to illustrate the differences and similarities as well as overlaps, let's briefly outline some related cybersecurity assessment types. Most of these assessment types have been discussed before but are reiterated here and put things into perspective.

The four main cybersecurity assessment types are as follows:

+   - A **gap analysis** compares the current set of mitigation controls to a list of recommended security controls, provided by a standards body such as NIST. The method looks for deviations or **gaps** between the existing prevention mechanisms for a system and the recommended mechanisms. Activities such as a network architecture drawing review and system configuration review are used to identify the gaps.

+   - A **vulnerability assessment** will unearth vulnerabilities or flaws in an ICS asset or in the system as a whole by comparing the current patch level of devices or application revisions against a list of known vulnerabilities for that patch level or application revision.

+   - A **risk assessment** is an all-inclusive assessment of the risk exposure of a system. The assessment includes gap analysis and vulnerability analysis to create risk scenarios or risk maps, which are strategized scenarios of possible attacks to the assessed system. A risk assessment will calculate the risk score for a system and, when combined with a penetration test, can provide a very accurate, actionable, and relevant insight into the overall risk landscape of the assessed system. With these risk scores, a more targeted and effective risk mitigation plan can be designed, maximizing the return on investment of applied controls.