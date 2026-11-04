POS is an endpoint. Therefore, it has an operating system, memories, processing units, and physical peripherals that facilitate network connection and human interaction. Although all security measures are considered (such as encrypting all communications), malicious software still targets POS terminals. Let's analyze the Home Depot POS data breach of 2014, as follows:

**Victim company:** Home Depot, Inc.

**Country:** United States (US)

**Breach type:** POS attack

**Time taken to discover the breach:** 5 months (April to September 2014)

**Description:** Home Depot was a victim of one of the biggest data breaches. Attackers gained access to the POS network and stole data on 56 million credit cards.

**Attack surface and vector:** (1) The attackers *stole credentials* from Home Depot's third-party vendor. (2) They used the credentials to gain access to the vendor's system or network. (3) They exploited a *Windows zero-day vulnerability* to pivot from the vendor's system to the Home Depot corporate network. (4) They installed *memory-scraping malware* on more than 7,000 POS terminals. (5) The malware stole information relating to 56 million credit cards and around 53 million email addresses.

**Loopholes in Home Depot security:** Home Depot failed to detect the attacker sooner and prevent it from happening in the first place. Although they had Symantec Endpoint Management, we can see that specific security measures and procedures were not in place, as follows: (1) *Vulnerable POS terminals' software and hardware.* (2) *Poor vulnerability management program.* They should have been able to detect the breach if vulnerability assessment had been executed frequently. (3) *Poor access management* between vendors and Home Depot networks. (4) *Poor credential management* from both the vendor and Home Depot. (5) *Poor (or lack of a) data/network monitoring* solution, introducing a delay in the response. (6) *Poor network design.*

**Probable prevention:** Measures that could have prevented the attack include the following: (1) *Efficient POS terminal configuration policies* to enforce secure practices in software and hardware. (2) Use of *point-to-point encryption (P2PE)* to encrypt payment data in POS terminal memory. (3) *Operating system upgrade*, since Home Depot terminals were running Windows XP Embedded **Service Pack 3 (SP3)** at that time. (4) *A good vulnerability management program.* (5) *Segmenting corporate networks* using technologies such as **virtual LANs (VLANs)**. (6) Properly managing third-party access to sensitive data. (7) Having a *sound monitoring system* that could feed to a SIEM or TIP (having a *TI program* that could allow a centralized actionable defense; a TI program could have shown them gaps in the security infrastructure).