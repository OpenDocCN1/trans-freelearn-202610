
- **Guide data collection**: Threat intelligence feedback identifies any gaps in the data that has been collected. Different teams can indicate whether the data collected was adequate or not.
- **Help rate the program**: Threat intelligence feedback helps the CTI team evaluate the overall program.

Threat intelligence feedback can be added to the intelligence report as a different section. Additionally, it can be used as part of the strategic report – to showcase the actionality and quality of the program to the strategic team. The CTI team should not undermine the consumer's opinions on the program.

## Methods for collecting threat intelligence feedback

There is no standard method in which to collect threat intelligence feedback. The CTI team should understand the audience and put in place the appropriate methodology to collect a consumer's opinions and comments. Some of the methods used by threat intelligence analysts include the following:

- **The URL link in the report**: The CTI team adds a feedback section within the report with a link for comments and opinions. This method works better for electronically generated and distributed reports – the organization creates a platform where consumers can download the latest CTI reports and leave feedback.
- **The security survey after each threat intelligence analysis**: The CTI team can collect feedback about individual threat or intrusion analysis cases and then aggregate them to global feedback. This method is known as *partial feedback collection*. It is of great value because it can help analysts adjust requirements, add more data, and enhance analysis processes on the go.

> **Important Note: Feedback Timing**
> While threat intelligence feedback is critical for the program improvement, its timing (temporal component) must be optimal and pragmatic. If the consumers take longer to provide feedback, the latter might not effectively reshape the program anymore. For example, for a monthly generated threat intelligence strategic report, two weeks should be applied, giving the team enough time to adjust the requirements. However, if the strategic team takes more than a month to provide their views on the report, there will not be feedback for the next report.

Whichever method is used, threat intelligence feedback should facilitate collaboration and interaction between the producer and the consumer. Additionally, the proper timing should be applied to the consumer's responses to keep the CTI program effective.

## The threat intelligence feedback cycle – use case

Let's assume you have generated a threat intelligence report and shared it with the rest of the SOC team through a web link. The feedback cycle will be as follows:

1. The blue team *accesses* the monthly tactical report and intends to use it to update the system defense, including firewall, endpoint, and intrusion defense system rules (making it actionable).
2. The blue team realizes that the produced intelligence is not timely enough as some of the IOCs and adversary campaigns have been retired. They provide this information to the CTI team in an *easy* and *understandable* manner.
3. The blue team *expands on point two* by explaining the *consequences* of a non-updated intelligence output, such as the inability to monitor recent threats and attacks. At this point, the CTI team has enough information to reshape the requirements. For example, they change from monthly intelligence to weekly or bi-weekly reporting.
4. The blue team restarts from point 1 (of the feedback loop).

Here, the cycle described provides a straightforward way for CTI to collect feedback. We can then summarize the feedback process, as shown in the following diagram:

![](img/d47f26598025b2da017dd41fcbb6e602_460_0.png)

Figure 14.13 – The threat intelligence feedback loop cycle

The threat intelligence feedback loop is essential within a CTI program. You need to ensure that the produced CTI is actionable and benefits all consumers. The best way to do that is to ensure that all consumers are satisfied and the program ROI can be justified. The CTI process is a closed-loop. As long as the program is active, consumer feedback must be taken into consideration. Therefore, the feedback process also becomes a closed loop.

## Summary

Threat intelligence reporting is critical to ensure that the CTI program benefits the organization and that all relevant parties are aware of the threats and attacks the organization faces. Reporting must be tailored to the recipient’s needs. As intelligence is created, it needs to be disseminated internally and externally – threat intelligence is a collaborative and interactive subject. Therefore, it is recommended that you join one or more sharing communities. However, you should assess the community you want to join based on factors such as support, maintenance, push and pull requests, cost, terms, and conditions.

This chapter looked at threat intelligence reporting, discussing the types of intelligence reports that a CTI analyst should consider. It discussed how to make reports valuable and create one using a simple template. Then, it detailed how to understand and build adversary campaigns, track threat actors, retire campaigns, and retire CTI courses of action. It offered a global overview of APTs. Finally, it discussed the particulars of threat intelligence dissemination and the importance of a feedback loop in CTI.

Although reporting, dissemination, and feedback are considered the last phase of CTI, it is essential to understand that CTI is a cyclic process. Therefore, the last phase feeds the first phase of the next cycle.

In the next chapter, we will look at threat intelligence sharing and IOC construction using practical use cases.

## 15

## Threat Intelligence Sharing and Cyber Activity Attribution – Practical Use Cases

Sharing threat intelligence has proven beneficial to the security community, helping organizations strategically, tactically, and operationally. Intelligence can be shared in several ways and formats. The **Cyber Threat Intelligence (CTI)** analyst or team must be comfortable drafting threat intelligence reports and preparing output for internal and external dissemination. When conducting threat intelligence, they should understand how to build threat activity groups and campaigns from analyses. Awareness of nation-state or state-sponsored actors' influences and advanced cyber criminals' threats in the cyber arena is critical in tracking some of the most sophisticated cyber activities. Previous attacks and breaches have shown that state-sponsored actors can target private institutions. Therefore, the CTI analyst or team should be able to conduct an **Analysis of Competing Hypotheses (ACH)** and attribution of threats and attacks to state-sponsored actors when necessary.

This chapter focuses on practical threat intelligence-sharing approaches and how to attribute cyber activity to a campaign, threat groups, or threat actors. It equips an analyst with practical skills to develop and share **Indicators of Compromise (IOCs)** for internal security enhancement and external dissemination. It also equips the analyst with knowledge of threat tracking.

By the end of this chapter, you should be able to do the following:

- Develop or uncover IOCs using YARA rules and use them to detect and stop attacks.
- Set up Anomali STAXX, an open-source intelligence dissemination platform based on **Structured Threat Information eXpression/Trusted Automated eXchange of Intelligence Information (STIX/TAXII)**.
- Use a threat intelligence sharing platform for intelligence sharing.
- Build activity groups from threat analyses and associate analyses to each group.
- Conduct ACH to attribute cyber activities to state-sponsored groups.

In this chapter, we are going to cover the following main topics:

- Creating and sharing IOCs
- Understanding and performing threat attribution

## Technical requirements

This use case uses the **SANS Investigative Forensics Toolkit (SIFT) virtual machine (VM)**. It can be installed on **VirtualBox or VMware**. Alternatively, an Ubuntu machine can also be used. SIFT's download and installation steps can be found on the official website: https://www.sans.org/tools/sift-workstation/.

The analysis performed in *Chapter 10, Threat Modeling and Analysis – Practical Use Cases*, with its artifacts is also required.

## Creating and sharing IOCs

Let's consider the intrusion analysis performed in *Chapter 10, Threat Modeling and Analysis - Practical Use Cases*, with the simplified kill chain discovery action shown in the following figure. Some of the other indicators extracted during the analysis included the `executable.1640.exe` hash value, `12cf6583f5a9171a1d621ae02b4eb626`, and the `resoh.ru` domain:
