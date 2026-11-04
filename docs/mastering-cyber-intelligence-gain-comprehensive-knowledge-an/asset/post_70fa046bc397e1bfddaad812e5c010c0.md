
Nowadays, social media provides a lot more information than just connecting people. With APIs, developers and security analysts can access social networks such as *Twitter and Tweetdeck* (https://tweetdeck.twitter.com/), *Facebook*, *LinkedIn*, and *YouTube* to perform *searches*, *retrieve analytics*, and *harvest archives and documents*. These functionalities help collect social media data for intelligence – following security researchers, outlets, and agencies to find out about exploits, malware samples, and vulnerabilities news. We can retrieve hot security topics about threats, vulnerabilities, exploits, malware, breaches, and more. In the following subsection, we will look at some OSINT or low-cost malware data collection sites.

## Publicly accessible malware portals

Most data breach reports and investigations have shown that a high percentage of cyberattacks involve malware, directly or indirectly. Hence, the CTI team or analyst needs to collect malware-related data for intelligence. Many sites or portals facilitate malware analysis openly. We will not cite all of them, but we will provide some malware sites and links that can freely be used for malware analysis and data collection:

- **VirusTotal**: VirusTotal (https://www.virustotal.com/gui/) is a free online malware analysis platform that allows security professionals to submit malware files and get analysis results. The result is shared with the requester and the organizations that are partners with VirusTotal. Through the VirusTotal API (public or premium), the CTI team can enrich the internal security tools with telemetry data to facilitate alert reports and integrate the data with TIPs and SIEMs.
- **VirusShare**: VirusShare (https://virusshare.com/about) is a malware samples repository that provides organizations and individuals with access to malicious code samples. VirusShare is free, but access is subject to an invitation. Thus, an organization or individual needs to request whether they can be added to the list to access the data. However, it has commercial APIs for data feeds and specialized searches of the data.
- **Any.run community**: Any.run (https://any.run/) is a malware analysis and sandbox platform that allows information security analysts to access live malware data processing, analysis, and IoC data. It can also be used to enrich TIPs and SIEMs. You can register for the community edition of the tool and download samples of malware data.
- **Intezer community edition**: Intezer (https://www.intezer.com/) is a malware analysis, threat hunting, and incident response platform that allows analysts to classify and reverse engineer malware files. Intezer supports multiple data formats, including STIX. Hence, it can be integrated into modern TIP and SIEM.

Many malware sites are offering free analysis and access to data. The CTI team or analyst must search for those sites and understand how they can benefit the program.

In this section, we have looked at some of the publicly available open source and low-cost data sources. OSINT is a broad area of research and application. Hence, it demands a thorough understanding and selection. Most of the sources provide mechanisms to connect and collect data (APIs or database downloads). In the next section, we will look at the basic structure of open source intelligence resources.

## OSINT platform data insights (OSINT framework)

Collecting OSINT data can be challenging (where to fetch the data from, how to collect it, and what to look for in that data). It requires a good knowledge of the underlying data sources. The sources listed in the previous section, *Open source intelligence portals*, are ideal for when you wish to start collecting data. Much more information can be collected as part of the OSINT framework (https://osintframework.com/). This framework summarizes the resources and tools that can be used to collect and gather security information.

![](img/d47f26598025b2da017dd41fcbb6e602_231_0.png)

Figure 7.1 – OSINT framework tools and resources

You can browse each OSINT category and access the tool or resource for an in-depth understanding of them. Tools and resources marked with R require registration, while those marked with T need to be installed locally. Although the tool is still being updated and improved, it can be used to create a list of OSINT data sources. If we click on Exploits & Advisories > MITRE ATT&CK, the site will redirect us to the MITRE official website for more details. In the following subsection, we will discuss the probable limitations of OSINT data.

## OSINT limitations and drawbacks

OSINT sources provide a large amount of data – data that could be structured or unstructured, controlled or uncontrolled (in terms of overhead). Some of the challenges in working with OSINT include the following:

- **Demanding data cleaning:** Most OSINT data or feeds are raw and not ready for consumption in their initial state. Many post-processing operations need to be performed on them to get them into consumable formats (ready to query, correlate, or integrate with other tools).
- **Source validation:** Source validation and evaluation are essential in working with threat intelligence data and feeds. The voluminous nature of OSINT makes it challenging to evaluate, especially manually. Imagine trying to validate 10 sources of data. It is possible but could require time and resources. How do you validate OSINT coming from a source that conducted corporate or government espionage? This ties OSINT to the data accuracy problem.
- **Information filtering:** Finding the required information in data with a lot of noise is a challenge. And because of that, there is a chance of getting misleading information, resulting in many false positive alarms.

Two components can be invoked to address some of the limitations of OSINT: *data collection automation* and *field expertise*. Data collection automation reduces the probability of human errors and improves the data collection cycle's speed. And by having expertise in the collection process, the CTI team can find optimal ways to separate noise from data insight and the generated false positives. Thus, it is recommended to automate the collection process as much as possible.

OSINT is a good source for intelligence project initiation. Therefore, level 1 and 2 organizations can leverage that to start building intelligence. However, it is essential to understand that the objectives and the CTI requirements must be the drivers of the OSINT data collection process. When suitable sources are selected, they can be correlated with internal data to provide reliable intelligence. In the next section, we will look at malware data and its use for threat intelligence.

## Malware data for threat intelligence

Malware is one of the most commonly used words in cybersecurity. Its invocation brings turmoil and torment to organizations and bliss to attackers. Its concept is complex to comprehend as it requires a different set of skills and expertise (architecture, analysis, and design). However, as a CTI analyst, understanding malware data and its collection must become second nature. In this section, we will look at malware data that is fed to the TIP or SIEM for intelligence.

Sites (or sources) such as VirusTotal, VirusShare, and other malware sandboxes are powered by malware analysis engines that allow them to analyze the behavior of files or links that have been uploaded to classify them as malicious or not. Although open source sandboxes can help CTI teams and analysts perform malware analysis automatically, it is essential to understand the basic information about malware data.

> **Important Note**
> Malware analysis is a topic on its own and is not part of this book. We have only provided basic information on malware and the fundamental indicators that CTI analysts can extract from malware data.

Modern malware design techniques can bypass security systems and avoid detection. Advanced analysis is required to detect malware threats reliably. The CTI team or analyst aims to extract IOCs from the data and feed that to the TIP or SIEM by connecting to malware-related data sources. They can then correlate that with internal data to create alerts for future threat detection.

## Benefits of malware data collection

The key benefits of using malware data for threat intelligence include the following:

- **Malware detection**: Identify malware IOCs that need to be blocked. The CTI can implement notification services to report on possible malware threat detection.
- **Threat prioritization**: The CTI team can create high-priority alarms on malware detection where malware threats have high priority over other IOC alerts. Automated threat prioritization makes the triage task simpler.
