# Malware data for threat intelligence

Malware is one of the most commonly used words in cybersecurity. Its invocation brings turmoil and torment to organizations and bliss to attackers. Its concept is complex to comprehend as it requires a different set of skills and expertise (architecture, analysis, and design). However, as a CTI analyst, understanding malware data and its collection must become second nature. In this section, we will look at malware data that is fed to the TIP or SIEM for intelligence.

Sites (or sources) such as VirusTotal, VirusShare, and other malware sandboxes are powered by malware analysis engines that allow them to analyze the behavior of files or links that have been uploaded to classify them as malicious or not. Although open source sandboxes can help CTI teams and analysts perform malware analysis automatically, it is essential to understand the basic information about malware data.

> **Important Note**
> Malware analysis is a topic on its own and is not part of this book. We have only provided basic information on malware and the fundamental indicators that CTI analysts can extract from malware data.

Modern malware design techniques can bypass security systems and avoid detection. Advanced analysis is required to detect malware threats reliably. The CTI team or analyst aims to extract IOCs from the data and feed that to the TIP or SIEM by connecting to malware-related data sources. They can then correlate that with internal data to create alerts for future threat detection.

# Benefits of malware data collection

The key benefits of using malware data for threat intelligence include the following:

+   - **Malware detection**: Identify malware IOCs that need to be blocked. The CTI can implement notification services to report on possible malware threat detection.
+   - **Threat prioritization**: The CTI team can create high-priority alarms on malware detection where malware threats have high priority over other IOC alerts. Automated threat prioritization makes the triage task simpler.