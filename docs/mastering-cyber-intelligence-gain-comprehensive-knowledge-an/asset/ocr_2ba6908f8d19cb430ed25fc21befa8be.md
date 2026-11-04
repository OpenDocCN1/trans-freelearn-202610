+   - **Executive summary**: This is an executive summary summarizing the threats and their business impact. This section is about the summary of the analysis – the highlights of the threat intelligence digest. A simple example is shown in the following diagram:

# 4. Executive Summary

*(Insert a summary of the threat intelligence after conducting analysis)*

```
As requested by the Department of Energy, a bi-weekly Cyber Threat Intelligence program was initiated to evaluate the organization’s security posture considering all web-related assets. The output has shown that the Department of Energy is vulnerable to a recent threat paused by the Russian group NOBELIUM, also known as APT29. This threat targets government organizations and results in sensitive data theft, trade secrets exfiltration, and espionage operations. It uses a variant of a well-known malware, the Cobal Strike. This threat should be considered a high priority ...
```

Figure 14.4 – Section 4 of the threat intelligence report – Executive Summary

An executive summary can also contain bullet points for quick scanning and understanding.

+   - **Traffic Light Protocol (TLP)**: TLP is used to share information. It uses a four-color scheme to highlight expected sharing boundaries that apply to the recipient’s test (please refer to https://www.cisa.gov/tlp and https://www.first.org/tlp/). It is a set of designations used to ensure that the appropriate audience receives sensitive information, such as the intelligence report. Additionally, it facilitates collaboration between the sharing parties. TLP can contain information about the threats, their risks, and their impact within the scope of application of threat intelligence. A TLP-designated threat intelligence report should highlight the TLP color of each threat or intrusion analysis included. The TLP color definitions are listed as follows:

+   - **TLP:RED** means that the intelligence is not to be disclosed and is restricted to participants or a group of people. The CTI analyst can use the **TLP:RED** indication when an intelligence report or analysis can lead to a violation of privacy, a bad reputation, and more. One example of this is a CTI report that contains private information about users, operations, and corporate data. It could be kept or shared between the CTI analyst and the executive team only.

+   - **TLP:AMBER** means the sharing is restricted to participants' organizations. A CTI analyst can use the **TLP:AMBER** indication when the intelligence report needs to be used by other teams of those organizations (such as forensics and threat hunting) or any trusted third party. The CTI analyst should specify the limit of sensitive information sharing.