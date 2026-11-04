
The preceding screenshot shows that one particular IP address has accessed the event page and individual employee pages. After reading the *ABC* website, we assume that the same IP address searched LinkedIn, Google, and Twitter for the four employees' information (essential for monitoring who is visiting the company and employee pages). We also found that the compromised hosts are *researchers* for an external group outside *ABC* company.

The reconnaissance phase is not malicious in nature but can be suspicious as adversaries search for targets. In the case of our analysis, this is likely to be linked to corporate espionage. The people targeted have direct access to the organization's research projects. The searches were done almost 3 months before the intrusion discovery.

In the next section, we fill the Cyber Kill Chain and the Diamond model frameworks to complete our manual analysis and get the scope of the intrusion.

## The Cyber Kill Chain and Diamond model

Now that we have identified most of the TTPs and infrastructure used by the adversary, we can fill the kill chain and Diamond model to identify gaps in the organization's defense. The Diamond model is shown in the following screenshot:

![](img/d47f26598025b2da017dd41fcbb6e602_333_0.png)

## Figure 10.30 – The Diamond model for each Cyber Kill-Chain stage

It is possible to have no supporting evidence for some elements of the Diamond model, especially the adversary information. While it is easier to profile the actor with enough intelligence, it could be an unknown element when no intelligence is available throughout the investigation.

The *discovery* COA for all stages of the kill chain is shown in the following screenshot. It is essential to fill in all findings, as they will be used for CTI reporting:

| Phase | Discovery | Detect | Deny | Disrupt | Degrade | Deceive |
|---|---|---|---|---|---|---|
| Reconnaissance | Website pages: event-mogul/about pages. LinkedIn, Twitter. Email addresses & research affiliations | | | | | |
| Weaponization | research_tech.pdf (Embedded JavaScript) | | | | | |
| Delivery | Phishing (email). 194.150.215.153 | | | | | |
| Exploit | Hook running process (explore.exe) | | | | | |
| Install | read_sl.exe (on explore.exe), executable.1640.exe, ~.exe (outlook temp) | | | | | |
| Command & Control (C2) | 125.19.103.198 Sys/files/ | | | | | |
| Actions | 46.168.5.140, Exfiltration – FTP, HTTP POST (RAR docs project cobra), C0d0s123 (password) | | | | | |

Figure 10.31 – The simplified Cyber Kill Chain with discovery COA

We have managed to analyze the intrusion methodically, pivoting internally from one indicator to another. We can tell the story of what happened. As a CTI analyst, you must provide mitigation plans on stopping such attacks in the future (at any stage of the kill chain). However, the mitigation strategies' implementation will require the strategic team's coordination (management and executive teams) and will involve different security units (risk management, security operation managers, IR managers, and the **chief information security officer (CISO)**).

We have gathered the necessary information for the discovery COA. The next step should involve analyzing the intrusion for the rest of the actions. However, depending on structures and organizations, the rest of the actions might be part of the IR team. CTI works with all the security functions to ensure that information is available for decision-making. The next section looks at CTI storage and automatic data collection using MISP.

## MISP for automated threat analysis and storing

MISP (https://www.misp-project.org/) is an open source TI platform for automation, analysis, and intelligence sharing. I recommend MISP for small and medium enterprises with limited resources. We start with the default MISP configurations, and we can then add extra data feeds later.

The following sections allow us to automate and store the previous analysis.

## MISP feed management

Collecting the correct external data can be challenging. MISP allows integration with several feeds and comes with a set of free feeds (https://www.circl.lu/doc/misp/managing-feeds/). The following points provide steps to enable the feeds:

- 1. Log in to the MISP dashboard using the admin@admin.test username and admin password. You will be requested to change the password on the first login.
- 2. Navigate to **Sync Actions > List Feeds**.
- 3. Click on **Default feeds**, select all, and enable them. The feeds will be enabled for correlation with our analysis. You get the name, format, provider, source, and several other fields for each field.
- 4. You can use **Add Feed** to add a new feed to the current MISP configuration.
- 5. Select the fields you want to use for the analysis, click on **Fetch and store all feed data** to fetch the indicators and events from remote servers.

The feed overlap option of the platform allows analysts to check if data feeds overlap. Once the feeds have been enabled and cached, we move on to the next task.

## MISP event analysis

To perform intrusion analysis and correlate with external sources, we need to create an analytic event. The following steps walk us through the process:

- 1. Create an event by clicking on **Event Actions > Add Event**.

The *date* is the date of the intrusion detection; the *distribution* is set to the **organization only** until the case is ready to be published. The *threat level* depends on the severity of the threat, and the *analysis* is set to **initial** for now. Click on **Submit** to save the event.

- 2. Add the attributes. Use the Cyber Kill Chain output to add attributes, attachments, and the email object that led to the intrusion. The first attribute we add is the reported C2 IP address (125.19.103.198). **Hint:** Use each infrastructure of the Diamond model as an attribute. Use **Add Attribute**, **Add Object**, and **Populate from...** to add indicators for our analysis.

- 3. Enrich the events by going to **Enrich event** and selecting all the enrichment you would like to add. The attributes will be populated with more information.

- 4. Publish the event with the organization or community using the **Publish Event** menu option.

MISP stores the analysis and facilitates the sharing of this with the community. Another advantage of the platform is the correlation with external intelligence data. You can use **View Correlation Graph** to view links between the analysis (event) created and other events.

> **Important Note**
> 
> When dealing with indicators, always aim for stable ones that are not likely to be manipulated. IP addresses can be changed easily or moved dynamically. Going with the domain can be a good option. However, C2 domains can also be dynamically manipulated (there are several free and paid dynamic DNS services). It is, therefore, crucial to understand the different adversary techniques of indicator obfuscation.

The visual link representation of MISP can help link infrastructure, indicators, and threat actors together. MISP also allows generating rules (such as **Yet Another Recursive Acronym (YARA)** rules) to improve security by blocking traffic coming in containing all detected indicators. More on MISP can be found on the official MISP web page (https://www.misp-project.org/), and there are also tutorials on how to leverage MISP for CTI analysis.

## Summary

It is impossible to analyze all network activities one by one. The CTI process is not always straightforward, and you are not guaranteed to find all the elements of the CTI frameworks. However, the logic is that if it happened, then the evidence is there. You just need to search deeper and ensure that the organization has data to help with the investigation. Refer to *Chapter 7, Threat Intelligence Data Sources*, for the data collection and minimum data requirements to start a CTI program (a centralized data system, raw packet data capturing and storing, and so on). Note that *SIEM tools (for example, Splunk, IBM QRadar, AlienVault USM, McAfee ESM, and so on) and TI platforms (TIPs) (for example, IBM X-Force Exchange, MISP, FireEye Mandiant, AlienVault USM, and so on) make threat and intrusion analyses and dissemination simpler than the manual approach.* However, although manual intrusion analysis is complex, it is often a fun investigation and rewarding process. Thus, an analyst should know how to conduct a threat or intrusion analysis—a fundamental skill set. It is, however, recommended for a TI analyst to know one or more SIEM tools for security operations. *It is also recommended to have sound knowledge of log files' format and analysis.*

This chapter has described the basic steps of performing intrusion analysis from a CTI standpoint. It has also shown how to map the threat analysis output to the Cyber Kill Chain by completing the Diamond model elements at the end of each phase. You should be able to perform intrusion analysis as it forms part of an analyst's main job profile.

In the next chapter, we look at making CTI part of the business process by following a usable security approach.
