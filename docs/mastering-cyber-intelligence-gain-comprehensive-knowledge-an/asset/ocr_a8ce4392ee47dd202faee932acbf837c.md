+   - **TTPs:** TTPs define the adversaries' tactics, techniques, and procedures used to achieve their goals. All steps from reconnaissance to exfiltration (or any action) are considered here. You should identify each TTP used at each stage of the Cyber Kill Chain. For example, using MITRE ATT&CK, we can highlight phishing for reconnaissance and initial access tactics. And many techniques can be used for those tactics, such as emails with malicious attachments, URLs, and phone numbers.

As a CTI analyst, it is critical to know and understand the PoP's IOCs, what they mean, and how to recognize them in a system. Refer to the previous section to read about the key IOCs and how to deal with them. The following subsection explains the PoP and its use in intelligence and incident response.

# Understanding the PoP

The pyramid's colors and width are critical in understanding the impact IOCs have on adversary activities. The top of the pyramid is red, and the bottom is blue, with the biggest part being green (two steps). The IOC at the bottom of the pyramid has little impact on the adversary compared to the one at the top. As we go up the pyramid, the effort required to deny the IOCs becomes huge. The question that will drive the PoP explanation is *How painful would it be for the adversary to find a workaround?*

**Hashes** are used for data integrity and signatures. Every input has its hash; therefore, two different malware files cannot have the same hash. This means that should the adversary use malware whose hashes are registered in the security signature databases, the system will likely stop such malware. However, hashes are very easy to change. An adversary has to change one character only or add a few NULL characters in the malware code, and the hashes change. An example is shown in the following figure.

We can see that "Security" and " Security " (with spaces) have different hashes:

![](bbox=[0.21, 0.65, 0.79, 0.86])

Figure 13.4 – MD5 hash values for a file after content modification