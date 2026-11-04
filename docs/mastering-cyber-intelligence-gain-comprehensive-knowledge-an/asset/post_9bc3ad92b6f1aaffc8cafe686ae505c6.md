
OTX can work as a STIX/TAXII server, making it usable with any STIX/TAXII client. It can also be integrated into the MISP platform. Some of the IoCs that are generated include IP addresses, domains, hostnames, emails, URLs and URIs, file hashes, CIDR rules, CVE numbers, file paths, and more. Using OTX data can help you address the following points:

- **Organization exposure level:** How badly the organization is exposed to threats.
- **Threat relevance:** By analyzing OTX intelligence, the CTI analyst can determine how relevant a threat is to the organization.
- **Threat actors and groups:** OTX IoCs allow you to identify the adversaries and motives behind threats.
- **Targeted assets:** Based on the data collected, OTX can help you identify the most targeted assets.

> **Important Note**
> AlienVault OTX is one of the intelligence sources and services that organizations can use to start a CTI program. Not only does it provide integration with SIEM and TIP, but it also acts as STIX/TAXII server, which saves and protects costs for commercial STIX servers.

Intelligence sources such as OTX and AIS can be considered standalone intelligence platforms. They extract information from multiple data sources such as search engines, forums, emails, logs, and more to make them available to the public in a format such as STIX/TAXII or database files.

## InfraGard Portal – FBI

InfraGard defines partnerships between the FBI and other sectors (public and private). It is a collaboration where the FBI and organizations share cybersecurity incidents and threat intelligence to fight against cybercrimes in the US. The requirements and nature of information sensitivity make it a US-based membership source – only US companies, academies, and individuals can subscribe to the data source.

InfraGard divides critical infrastructures into 16 sectors that represent the major business areas. Organizations (members) have access to recent cyber threats and crimes being tracked by the FBI. More information can be obtained from the official website (https://www.infragard.org/).

> **Important Note**
> It is essential to vet **Indicators of Compromise (IOCs)** coming from third parties such as law enforcement and government sources. Some of them (such as hashes, IP addresses, and domains) might be legitimate programs or application indicators. Not vetting IOCs can result in business disruption. Refer to the intrusion analysis use case in *Chapter 10, Threat Modeling and Analysis – Practical Use Case*, for more information.

## Internet Storm Center (ISC) and DShield

Internet Storm Center is an open source and accessible platform for cybersecurity events sharing. The SANS Institute powers ISC by bringing security experts and organizations together to analyze internet traffic (on a large scale) for malicious activities. It gathers intrusion detection log entries daily using sensors. It covers over 500,000 IP addresses in over 50 countries (https://www.dshield.org/about.html). ISC commits to identifying sites used by adversaries to commit cybercrimes and availing data on the types of attacks orchestrated in various industries and regions.

Sensors collect information about suspicious traffic on the internet. These sensors that are used by the ISC work with most network security elements (firewalls, IDSes, broadband devices, operating system data) and send the data to the DShield database to be analyzed by volunteers and machines to identify behavior. The result is posted on the ISC site (https://isc.sans.edu/dashboard.html) to be viewed or queried through scripts. The SANS Institute also sponsors the DShield service. CTI analysts can register with DShield to get the benefits of *accessing the logs* collected and submitted to the database. The registration can allow the CTI team to download the logs and integrate them with internal security systems (TIPs and SIEMs).

Many organizations detect cyberattacks long after the system has been infected. ISC encourages organizations to submit their network security logs to Dshield for analysis. Hence, another advantage of registering with DShield is to activate the *fightback capability*, which allows the ISC to notify an organization of an occurring or occurred attack.

> **Important Note**
> Any organization can leverage the SANS Institute DShield service to monitor intrusion attempts and protect the system. While the registration provides some benefits, it is not required to submit network element logs.

For more details about the ISC and DShield, please visit the official website provided in the second paragraph of this section.

## Information Sharing and Analysis Centers (ISACs)

**Information Sharing and Analysis Centers (ISACs)** (https://www.nationalisacs.org/) provide central points for collecting cyber threat information affecting different critical infrastructures. An organization can join an ISAC depending on the sector. ISAC membership may come with a cost, but the intelligence provided is affordable or free to the public. Each sector's ISAC (such as financial services, communication, health, and others) collects and analyzes threat information. The generated intelligence is then shared with the members, along with the tools to mitigate risks and improve security. Some benefits of joining an ISAC include the following:

- 24/7 sector-specific monitoring and alerting of cyber threats through the portals, platforms, and applications
- Hands-on training, exercises, summits, and security events for registered members to ensure that they get the full benefits of the platform
- Reliable support for all the steps of a cyber incident
- Supports the STIX/TAXII standard

Organizations can leverage the benefits of ISAC to integrate intelligence into business operations. Membership fees are linked to the organization's size and infrastructure (critical assets, revenue, customer base, and company size).

## Abuse.ch

Abuse.ch (`https://abuse.ch/`) is an open source platform owned by a Swiss professional for malware tracking and monitoring. The platform tracks indicators such as IP addresses, domains, URLs, distributed sites, payment sites, and C2 servers associated with different malware types. Several security professionals, vendors, researchers, and law enforcement agencies use abuse.ch to understand mechanisms used by malware and how threat actors use those mechanisms to launch cyberattacks. Abuse.ch has main projects including MalwareBazaar (used to share malware information), *Feodo tracker* (used to share botnet C2 servers associated with the Feodo malware), *I got phished* (for acquiring information on phishing victims – domains, email addresses, IP addresses, and others), and *SSL blacklist* (used to identify malicious SSL certificates that can help organizations detect fraudulent SSL connections). SSL blacklist also helps detect and block malware botnet C2 channels on the TCP layer. Additionally, there is *URLhaus*, used for sharing information on URLs mainly used for malware distributions, and *threat fox*, which is used for sharing **Indicators of Compromise (IoCs)** with the cybersecurity arena.

Abuse.ch is a non-profit organization; hence, it allows CTI analysts, security vendors, and IT security professionals to collect information freely and build better cyber defense systems. There are two standard ways to collect data from abuse.ch: use the provided APIs or download the data as a CSV file and load it into the TIP or SIEM.

> **Important Note**
> Abuse.ch's MalwareBazaar provides a Python API (`https://bazaar.abuse.ch/api/`) that the CTI team can use to integrate malware samples (data) into the TIP or SIEM system. The API can also be used to upload malware samples to the MalwareBazaar database. The latter can also be browsed for malware information. Malware samples must be dealt with carefully to avoid infection during analysis.

It is essential for security analysts and researchers to contribute to the platform by uploading malware samples, phishing data, and botnet C2 channel information. Not only does it enrich the platform, but it also helps the security community in fighting against cybercrimes.
