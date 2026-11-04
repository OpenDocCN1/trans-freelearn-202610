The advantage of building an in-house storage infrastructure could relate to cost protection. It is likely to cost less to build a big data platform based on open source applications such as Hadoop and Spark than buying an expensive TIP. In terms of efficiency and effort, in-house data storage systems might not be ideal for small and medium businesses.

# Threat intelligence platform for storing data

Probably the most popular choice (especially for level 1 and some level 2 organizations), TIPs can be used as intelligence storing infrastructure. This is justified by the fact that TIPs are built while following CTI standards (for example, they automatically support STIX/TAXII and YARA formats) and are ready for use. They also facilitate information sharing with the rest of the infosec community. Most paid TIPs are likely to provide storage mechanisms. However, open source TIPs such as MISP and Collaborative Research Into Threats (CRITs) also provide storage capabilities.

> **Important Note**
> For organizations that are new to CTI (level 1), I recommend saving your budget by leveraging open source TIPs such as MISP before diving into creating internal infrastructure or expensive TIPs. MISP is widely used because it provides several functionalities and is properly maintained. MISP is used throughout this book for practical use cases.

The CTI team and relevant stakeholders must select the correct strategies for storing intelligence data. When choosing one or both storage infrastructures, you must consider the requirements and how intelligence will be shared – CTI is a field that's growing due to the common cause of the community: fighting against cybercrimes.

Threat intelligence data feeds are passive in their original state. The CTI team or analyst must make them active by integrating them into existing security systems, TIPs, or Security Information and Event Management (SIEM). By doing so, TIPs or SIEMs can perform one of their principal tasks – correlating the external data feeds with the internal system data to provide more insights into threats.