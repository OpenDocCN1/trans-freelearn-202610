
- **Modular integration and price:** An organization can purchase QRadar with specific functions and upgrade as their business needs develop. This capability introduces flexible pricing.
- **Maintenance and support:** The solution is fully supported by IBM with a support portal for your organization to log tickets in case of issues.

QRadar Advisor has shown to have a lot of benefits. We can see that it addresses most of the security challenges raised throughout this book. The IBM Watson app for cybersecurity forms the AI engine for QRadar Advisor, providing powerful cognitive analytics and ML abilities.

### QRadar simplified architecture

QRadar is made of different engines and modules that form the entire stack. The tool collects data (both events and flows), aggregates and correlates it, and then stores it for real-time analytics and other application functionalities. The simplified architecture is shown in the following figure:

![](img/d47f26598025b2da017dd41fcbb6e602_301_0.png)

Figure 9.6 – The QRadar simplified architecture

The architecture layers are as follows:

- **Data collection layer:** This collects flows and events from the system. It can also collect external data to enrich the solution. The layer is made of two main components: events and flows collectors. The data is parsed, preprocessed, and normalized in a structured format usable by the platform's upper layers.
- **Data processing layer:** The data processing layer contains the rule engine, which allows you to create custom rules and use cases. The rules and use cases generate threat information, alerts, and priorities. Events and flows are processed separately. Other QRadar modules use the data processing layer to create functionalities.
- **Data search layer:** This is the application layer providing analytics and visibility of the system security in a human-readable way (for example, graphs, reports, colored alerts, and so on).

As mentioned in one of the previously listed benefits, QRadar supports modular deployment, allowing you to choose or purchase the modules you need. QRadar modules include the following (this list is not exhaustive):

- **QRadar Risk Manager:** For risk analysis, assessment, and control
- **QRadar Vulnerability Manager:** For vulnerability assessment and management
- **QRadar Incident Forensics:** For investigation and incident response
- **QRadar Advisor with Watson:** For AI integration and ML
- **QRadar Security Analytics:** For automated and proactive threat analytics

It is essential to understand what your requirements are in order to acquire the correct modules.

QRadar adopts a centralized system protection mechanism and integrates other existing security tools. In the next section, we will look at how you can deploy QRadar.

### Deploying QRadar

When you purchase QRadar, you have several ways to deploy it, depending on your organization's size, system topology, and IT infrastructure. However, there are two main methods and four models of QRadar deployment. The methods include **single-host deployment** (where the entire solution runs on a single system) and **multi-host deployment** (where the components run on different hosts). QRadar single-host deployment is appropriate for small and medium-sized enterprises (and enterprises with low public exposure). As the enterprise grows and the business needs develop, the solution can be expanded.

The deployment is subject to licensing, measured by *events per second* and *flows per minute*. Another important element to consider when deploying QRadar includes the *storage capacity*, which determines the *retention period*.

The deployment models include (1) on-premises (that is, installing the relevant hardware, software, or virtual machines), (2) managed service (in the sense of Software as a Service (SaaS)), (3) on-cloud (private or public), and finally (4) hybrid deployment (which combines on-premises and SaaS or Infrastructure as a Service (IaaS)).

For more details about IBM QRadar, visit the following links:

- QRadar SIEM, https://ibm.co/3x136bu.
- QRadar for visibility, detection, investigation, and response, https://ibm.co/3x1P5dF.
- QRadar use cases, https://ibm.co/3x3WU2e.

### What's in it for you or your organization?

I am sure you are wondering why we spent so much time on the IBM QRadar solution. As a CTI or security analyst in charge of decisions, you might want to acquire or advise on a SIEM solution. You can use IBM QRadar (as an existing and prominent AI-enabled SIEM solution) as a reference point to deep dive into details. It means that you should know what to expect from an AI-enabled SOC, SIEM tool, IR solution, or threat hunting platform.

Other similar tools include Securonix (https://www.securonix.com/), Splunk (https://splk.it/3A47isT), AlienVault USM (http://soc.att.com/3jlimM5), and many more. In Chapter 12, SIEM Solutions and Intelligence-driven SOCs, we look at SIEM and SOC operations.

### Summary

AI is changing the cybersecurity field, powering both defensive and offensive tactics. Therefore, organizations must adequately build roadmaps for integrating AI into business security operations. AI is powering CTI, threat hunting, SOC, IR, and many services to address three key use cases: system protection, staying ahead of adversaries, and faster threat response. In this chapter, we have introduced threat hunting and its importance in security intelligence. We have also shown that AI is a double-edged sword, as it can serve both organizations and adversaries. As a result, you should be able to highlight the importance of AI in your CTI program to the strategic team, position AI in your organization's security stack, and advise in the acquisition and deployment of an AI-based security tool (whether designed internally or acquired from a third-party vendor). In the next chapter, we look at practical use cases for cyber threat modeling and intrusion analysis.

## 10 Threat Modeling and Analysis – Practical Use Cases

The primary goals of **cyber threat intelligence (CTI)** analytics, at a high level, are *finding the source of a threat (an adversary and their activities)* and *stopping it* (although there are several processes and tasks to reach that point, as detailed in the previous chapters). A **threat intelligence (TI)** analyst must develop strong modeling and analytical skills that must become second nature because time and efficiency are of great importance in modeling and analyzing threats. Hence, the ideal scenario would be to *automate the process* as much as possible. However, leveraging manual processes is essential to ensure that a CTI analyst knows what to do when it comes to threat analysis.

This chapter focuses on practical threat modeling and analytics examples to illustrate how to conduct intrusion analysis. The chapter's endgame is to equip the intelligence analyst with practical knowledge to analyze threats manually and automatically.

At the end of this chapter, you should be able to do the following:

- Understand and master how to perform threat analysis using a real-life scenario
- Use the **SANS Investigative Forensics Toolkit (SIFT)** **virtual machine (VM)** and **Malware Information Sharing Platform (MISP)** for effective CTI analysis
- Understand MISP and leverage the tool to automate CTI collection, analysis, and storing

In this chapter, we are going to cover the following main topics:

- Understanding the analysis process
- Intrusion analysis case—how to proceed
- MISP for automated threat analysis and storing

### Technical requirements

The analysis in this chapter uses the MISP TI platform and the SIFT VM installed on VirtualBox. The download and installation of these tools can be done through the following official websites:

- `https://www.misp-project.org/download/`
- `https://www.sans.org/tools/sift-workstation/`

MISP and the SIFT Workstation can be a good combination for CTI analysis, especially for Level 1 organizations. However, it has to be noted that several tools can be used for CTI analysis, both open source and commercial. The objective of the use case is to provide a logical flow for analyzing CTI matters.

### Understanding the analysis process

One of the most important skills for CTI analysis is **pivoting**. Pivoting allows an analyst to use one or more identified indicator(s) as an anchor point and search through various data sources for events and flows related to those indicators. A simplified threat analysis process is shown in the following diagram, and we use that throughout the chapter to standardize our tasks:
