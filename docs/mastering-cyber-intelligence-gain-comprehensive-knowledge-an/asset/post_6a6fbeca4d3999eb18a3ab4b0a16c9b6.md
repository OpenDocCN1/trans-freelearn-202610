
## Technical requirements

For this chapter, no technical requirements are highlighted. Most of the use cases will make use of web applications.

## Integrating threat intelligence into SIEM tools – Reactive and proactive defense through SIEM tools

In an organization with no CTI program, a SIEM tool collects system logs and internal documentation from all devices (servers, wireless access points, firewalls, IDSs, and IPSs), applications (for example, software applications and protocol applications), and endpoints (for example, PCs, printers, and mobile devices) that connect to the network. It collects data from different system environments and different formats. It then consolidates, correlates, and converts that data to human-readable formats. Systems and people can query the processed data to identify abnormal activities. The SIEM system generates alerts (alarms) that indicate possible threats or attacks. All system events and flows must be logged to ensure proper internal visibility. A SIEM system can help with reactive and proactive system defense depending on the detected activity. A reactive defense is the system protection mechanism when a data breach (or a cyber incident) has already occurred. And a proactive defense refers to the system protection structures that minimize or avoid cyber incidents or breaches in the first place. In the following subsection, we look at high-level SIEM architecture and components. The latter will also help you as a CTI analyst to evaluate which SIEM system to acquire.

## System architecture and components of a SIEM tool

A modern SIEM system consists of several components, some of which remain mandatory. Most SIEM vendors adopt a modular architecture where system components can be purchased based on business agreements and requirements. License models, which can determine the number of components, the number of events to monitor, and the data storage, are critical in selecting SIEM systems. However, the features involved in SIEM systems include security information management and event management, as shown in the following figure:

![](img/d47f26598025b2da017dd41fcbb6e602_376_0.png)

Figure 12.1 – High-level architecture and components of an advanced SIEM system

By looking at the architecture in Figure 12.1, we can see that a SIEM tool is an essential system for SOC operations. The basic SIEM architecture can be divided into four layers: **data source**, **data collection**, **data processing**, and **application**. Let's look at each layer in detail.

### Data source layer

The data source layer is the lowest layer of the SIEM stack. Some analysts do not consider it part of SIEM; however, it is important to understand the organization's data sources available for security monitoring. Internal data such as **logs**, **previous incident analysis**, **forensics reports**, **policies**, and **rules** make the first data source or feed for a newly deployed SIEM system. Hence, an organization should have a good data and traffic monitoring infrastructure.

Organizations with CTI programs can benefit from external data sources and feeds such as **malware data**, **dark web reports**, **security news**, **intelligence analysis**, and **security vendors**. The more data sources or feeds your organization has, the higher the likelihood of better security insight. However, it is essential to look at data quality to avoid noise that can overwhelm the CTI analyst and consider the budget and strategic plan of the organization. Data source acquisition needs to be aligned with the organization's objectives, security, and intelligence requirements – refer to *Chapter 7, Threat Intelligence Data Sources*, to understand what data is required for intelligence and where to obtain the data.

### Data collection layer

A SIEM system must collect data from different sources and store the raw data (locally and in the cloud). The collection layer depends on the **storage capacity**, **memory size**, **processing power**, and **connector types**. The latter can be in the form of direct connections, **application programming interfaces** (**APIs**), or forwarders to fetch data:

- **Direct connections**: These are used for straight links to the data source repository. For example, an on-premises SIEM collection layer can have WLAN cables connected to the organization's incident database server. In this case, the SIEM collects incident data directly from the server. A cloud SIEM tool can use direct connect services (such as **AWS Direct Connect**) to access data sources in the organization's private WLAN.
- **APIs**: These are used for software-based connections between the SIEM tool and the desired data sources or feeds. APIs are one of the popular methods for intelligence data collection. External malware data sources such as **VirusTotal** (https://bit.ly/3r3zA1g) and **VirusShare** (https://bit.ly/30T3Gx2) provide API support to access and download malware data and metadata for intelligence.
- **Forwarders**: These are *software agents* that SIEM vendors install in the organization's internal repositories to automatically send data (for example, logs, reports, and incidents) to the SIEM system.

> **Security Note**
> Forwarders and APIs are third-party applications. They must be tested for security. SIEM vendors must detail security measures taken to protect data movement from collection to delivery. The organization must have strong third-party application access policies covering all operational and strategic requirements for secure connectivity.

The collected data is stored in a data lake for historical references and archives. Because the raw data cannot be used as it is, it needs to be processed to convert to human-readable format – the data processing layer's task.

### Data processing layer

The data processing layer, also known as the **mediation layer**, is responsible for **pre-processing**, **processing**, and **standardizing** the raw data. A good SIEM system must have a robust mediation layer that supports most data representation standards (STIX, TAXII, CSV, and so on). The following processes are done in the processing layer:

- **Data cleaning**: The data in the data lake is untreated and may contain inaccurate, duplicate, corrupt, and incomplete (missing) records. The SIEM data cleaning function detects and corrects abnormalities in the raw data. The methods used to clean the data depend on the vendors. However, it needs to be understood by the analysts.
- **Data aggregation**: Data feeds (or sources) use different event periods. For consistency, the SIEM system must ensure that data is aggregated uniformly across all sources to facilitate correlation. The data aggregation unit is responsible for combining and grouping data in a concise format. Aggregated data allows analysts to perform statistical and exploratory analyses. For example, proxy log events might be collected every minute, and the access point logs every 30 minutes. The SIEM system must consider the collection interval and aggregate the two datasets appropriately to correlate proxy logs with access point logs.
- **Data normalization**: Data sources must be organized in readable formats. It is advisable to normalize the data into relational structures to make it queryable. The SIEM system assigns keys and variables where necessary, such as IP addresses, domains, and hash values. The data is indexed and cleanly stored for rapid search and ordering.
- **Data correlation:** Data correlation allows the SIEM tool to establish relationships between different (or all) normalized datasets. The correlation process provides a more comprehensive (horizontal) view of events and flows in the system. For example, given a reported C2 domain, malicious.com, by performing data correlation between the proxy logs and intelligence report from an external source, we can identify all the internal IP addresses that have communicated with the external C2 domain. Correlation is important for the application layer because it can also highlight similar events or flows in the system or outside the system.

Data processing is the most critical layer of a SIEM system. If raw data treatment is not performed correctly, the SIEM output will not be helpful for the different security teams. The **garbage in, garbage out (GIGO)** principle of data science applies here. The following subsection discusses the application layer.

### Application layer

The application layer consists of different functions of the security infrastructure and use cases. The application parts stated in the architecture are not exclusive – there may be more or less, depending on the organization structure. SIEM-processed-and-correlated data can help different units as follows:

- **Advanced security analytics:** Most modern SIEM systems provide an advanced analytics platform. The platform analyzes SIEM processed data to *extract patterns* that pinpoint potential threats or breaches. It identifies and flags suspicious indicators, allowing security analysts to investigate. Then, it uses SIEM data to perform *behavior analysis* to identify abnormal activities in the system. **User behavior analysis (UBA)** has become an integral part of most SIEM systems.

For example, a sensitive organization file is only accessed by the executive group (normal behavior). A non-executive user has accessed the file (abnormal behavior). The system flags such behavior. Modern SIEM tools leverage **machine learning** or **statistical methods** (or both) to enhance the analytics capability.

- **Security dashboards:** SIEM systems have dashboard modules to get an insight into the system security graphically. The dashboards are made of one or more reports on the threat landscape, indicators, threats analysis, generated alarms, and many more security use cases. Examples of dashboard cases are the number of threats identified in the past 12 months, the number of abnormal behaviors per asset category, and the most used communication flows in the system. SIEM systems come with pre-defined dashboards. However, it is essential to customize them based on the organization's requirements. Whatever the SIEM dashboard shows must be aligned with the company objectives.
