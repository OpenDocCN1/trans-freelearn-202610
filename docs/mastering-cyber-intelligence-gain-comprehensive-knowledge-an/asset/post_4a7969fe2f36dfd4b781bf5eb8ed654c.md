
Threat intelligence goes beyond the collection and processing of internal data sources (*feeds*). We get more insight into IoCs, network and host artifacts, and adversary TTPs by using actionable intelligence. While IoCs and system artifacts may change, adversaries do not frequently modify their modus operandi. Adversaries' TTPs are directly linked to the technology used and its constraints. Therefore, intelligence analysis can help extract TTPs, adversary groups, and infrastructures to be used for threat hunting. TTPs sit at the top of the **Pyramid of Pain** (https://www.sans.org/tools/the-pyramid-of-pain/), meaning that if we can detect and respond to threats on the TTP level, the adversary needs to rebuild their entire attack strategy from scratch.

The MITRE ATT&CK framework provides a practical TTPs reference that can be used for TTP-based threat hunting. The Diamond Model provides an effective way to uncover capabilities, infrastructures, adversary profiles, and victims, which can be used for IoCs hunting, network artifacts hunting, and host artifacts hunting. The Cyber Kill Chain provides an efficient approach to understanding the entire attack structure. It is an *offensive* intelligence framework that can be used to identify and anticipate cyber threats.

CTI provides a good baseline for threat hunting as it helps (1) identify the gaps in the security infrastructure, (2) formulate hypotheses on adversary intents, capabilities, and opportunities (hypothesis formulation is essential in threat hunting methodology), and (3) collect and process data through the use of **threat intelligence platforms (TIPs)** and SIEM tools. TIPs and SIEM tools are designed (in most cases) to handle big data and perform advanced analytics and visualization. Threat alert and prioritization are essential drivers of threat hunting because they support scope definition. Based on the organization's business and CTI requirements, the threat modeling output can highlight what threats are likely to affect the organization and guide threat hunters in chasing the correct adversaries, TTPs, and threats.

The parallelism between CTI and **cyber threat hunting (CTH)** can be established by analyzing the TTP hunting methodology V diagram, published in the MITRE white paper by R. Daszczyszak, D. Ellis, S. Luke, and S. Whitley (https://bit.ly/3GnP9cy). The diagram is shown in the following figure:

![](img/d47f26598025b2da017dd41fcbb6e602_293_0.png)

Figure 9.3 – The TTP threat hunting V diagram by R. Daszczyszak and others

The methodology is based on malicious activity characterization and threat hunting execution, with each occupying one leg of the V diagram. The diagram has three layers: activity, analytic, and data processes. The components of the diagram are detailed in the following sections.

## Characterization

The characterization component involves understanding the adversary behavior and extracting the different TTPs they may put in place. CTH analysts use CTI data collection, reports, analysis, and research to understand and identify adversaries' mode of operation on different attack surfaces. As analysts, we use CTI frameworks such as MITRE ATT&CK or the Diamond Model to identify and prioritize TTPs. This activity model provides analysts with information on TTPs that are likely to change or remain constant.

After identifying the TTP, the analyst formulates hypotheses related to the identified TTPs. These hypotheses must be built on the TTPs that are not likely to change. One popular technique to perform a lateral movement in a system is using file-sharing over the Server Message Block (SMB) protocol or Windows Admin Shares – MITRE ATT&CK ID T1021.002. For example, if an adversary uses file-sharing, a hypothesis would be to look at scheduled tasks linked to file movement.

The hypotheses are then used to collect the correct data. Internal and external data must be collected and contextualized for the task at hand.

The data collected for intelligence is filtered and analyzed for the threat hunt. The scope of the search determines which part of the data remains valuable for the operation. *Time*, *surface*, and *behavior* determine the strategy of the adversary hunting. The *time* component helps identify the initial period of the hunt or the period of activity of a specific adversary. TIPs and SIEM systems have a data retention period that can situate the operation in time. The *surface* component allows the analysts to select the data related to the attack surface. For example, for a **Linux** system, collected Windows intelligence may be irrelevant. The *behavior* component allows the analyst to identify the TTPs likely to affect the system maliciously.

## Hunt execution

Similar to the CTI data collection requirements, the analyst must validate the data, identify the gaps in the data, and determine its usability factor. If the data is incomplete, the analyst must address the gap by finding new data sources or feeds. After the data is validated, the analyst can implement and test analytics. They build analytics queries to detect the mode of operation highlighted in the TTPs (or phase 1 of characterization, as shown in *Figure 9.3*).

> **A Note on Analytics Queries**
> 
> Analytics implementation and testing highly depend on the platform used by the organization or the analyst. For example, clients of QRadar and Splunk might have different ways of implementing analytics. The optimal way to create queries is to be general (that is, look at high-level components such as the protocol and process creation) and not specific (for example, protocol names, process IDs, and so on). A good understanding of the attack surface is essential in implementing effective analytics queries.

The last phase of the execution component is to hunt by detecting the real activities and investigating them. This process can be challenging. However, there are steps for this defined by the MITRE white paper, as shown in the following figure:

![](img/d47f26598025b2da017dd41fcbb6e602_295_0.png)

Figure 9.4 – The threat hunt execution flow

The execution flow is iterative and is explained in the following points:

- **Analytics tuning**: Ensure that the analytics developed for each TTP reflects the malicious activity events. The objective is to reduce the number of events that need to be looked at. This is achieved through correctly writing the queries.
- **Check for outliers**: Check if there is anything abnormal about the data analyzed. If there are too many outliers, the analyst needs to tune the analytics model again. If there are no events after querying, the analyst needs to revise the previous steps – because there will be no hunting if there are no events.
- **Evaluate the events**: The analyst uses skills and expertise to evaluate each event extracted from the analytics process. The aim is to classify each event as malicious or non-malicious. If an event is not malicious, move to the next event. And if an event is malicious, analyze it, then move to the next point.
- **Document the events**: As analysts, we must document malicious security events. An analyst must record all information related to events: timeline, host affected, user and malware information, and activity graphs. The analyst can use the discovered IoCs to pivot through external and internal systems for more information.
- **Contextualize the events**: Event contextualization allows an analyst to understand the context of each event. Identify facts that led to that specific conclusion (event is malicious or not). We must link the event to system components such as files, processes, user information, and communication protocols.
- **Investigate malicious events:** As analysts, we must identify the root causes of security events. Extract different activities that led to such an event. Intrusion analysis is a technique that can be applied at this step (refer to *Chapter 10, Threat Modeling and Analysis – Practical Use Cases*). We use the Diamond Model and the Cyber Kill Chain to work backward and reconstitute the event. The analyst must perform data pivoting to identify similar behavior in the system until all possibilities of similar behavior are exhausted and all analytics are completed.

The analyst must report the result of the process, which the strategic and tactical team will use to make security decisions to strengthen the system. The technical team's task is to respond to events through appropriate actions to ensure that the organization is ahead of the adversaries.

Threat intelligence is essential for threat hunting. A good CTI program facilitates threat hunting by keeping updated data sources (through open, paid, and shared feeds) and updated adversary profiles (through up-to-date TTPs and adversary behavior). It enriches threat hunting by expanding its detection types beyond IoCs or network/host-based hunting techniques. Hunting using threat intelligence can deny adversary access by operating at the top of the Pyramid of Pain, making their commitment to the cause extremely difficult. However, the next section looks at how adversaries can leverage AI to keep up the attack.

## How adversaries can leverage AI

AI is an open arena, which means it has favorable and unfavorable contributions to cybersecurity. Adversaries can leverage AI technologies to enhance TTPs. For example, imagine malware codes that can adapt to a system's defenses in real-time to avoid detection or malware codes that can change their signature to bypass anti-virus tools. Threat actors are continually developing intelligent malicious programs to orchestrate sophisticated and undetectable attacks. The following points highlight a few AI applications in intelligent attacks:

- **Smart malware:** In this instance, attackers use AI to weaponize legitimate files and links to inject malicious code into systems. The injected code learns the system's behavior toward them and initiates other actions, such as collecting information about the system defense or reporting what system feature blocked its operations. By learning about the system, it can impersonate genuine applications and processes to become invisible. It can also *self-evaluate* to understand the portion of the code that alerted the defense system and *self-actuate* to confuse the system defense while maintaining its minimum functionalities.
