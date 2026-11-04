**Scenario:** You are a CTI analyst in ABCompany, a credit record company that works with several partners, including banks, insurance companies, retail, and manufacturing. Your company also deals with credit disputes through an online web portal where users can log credit record issues with any organization. Due to the recent cyberattacks on the financial and insurance industries and the exposure of personal data in the sector, you have been tasked to research the 2017 Equifax data breach and extract valuable information for other security professionals operating in the design, passive defense, and active defense areas. You must also develop a threat model to identify the resources (and assets) that may be the targets, as well as identify possible TTPs that may be linked to threat actors.

# Equifax data breach summary

In this section, we will summarize the data breach by providing an executive summary, state the kinds of threat vector(s) used, determine the timeline, map the attack to a threat intelligence framework, summarize the vulnerabilities, identify the consequences of the breach, and look at what was done to strengthen the system.

**Executive summary:** Between May and July 2017, Equifax, a major credit record company, reported a major data breach that saw nearly 150 million people's personal and financial information being stolen. The attacker leveraged a vulnerability on an unpatched Apache Struts framework (web server) in one of the database servers that's used for online disputes. The attack went undetected for over 2 months.

**Threat vectors:** Unpatched vulnerabilities, poor or non-existing encryption, and misconfiguration (SQL code execution and easy system pivoting).

**Timeline:** The Equifax attack timeline is as follows:

+   1. The attacker ran a vulnerability scanner in March 2017 and discovered a known vulnerability in the web server software, Apache Struts, running on the online portal.
+   2. The attacker created an exploit, used several techniques to cloak it from Equifax systems, and executed SQL queries on the database (around 9,000 queries).
+   3. The attacker exfiltrated personal and financial data.
+   4. The attacker located additional servers, accessed unencrypted login credentials, and issued additional system commands to query and exfiltrate more PIIs from other databases.
+   5. The attacker went 2 months undetected and exfiltrated data during that period (May 13, 2017 to July 29, 2017).
+   6. On July 29, 2017, Equifax officially announced the attack. The organization determined the extent of the breach on September 7, 2017.