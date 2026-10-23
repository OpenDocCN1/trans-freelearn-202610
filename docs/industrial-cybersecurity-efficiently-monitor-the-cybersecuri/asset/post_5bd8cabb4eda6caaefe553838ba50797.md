
Now that we are connected to the JTAG interface, we can perform chip pin-level debugging, firmware extraction, and other low-level hardware hacking activities with software such as XJAnalyser (https://www.xjtag.com/products/software/xjanalyser/):

![](img/c8ffa9959721097701584fcc95101362_710_0.png)

Figure 17.47 – XJAnalyser in action

This is as far as we will take hardware hacking in this book. For more information, look at these articles:

-   https://hackaday.com/2020/04/08/a-hackers-guide-to-jtag/
-   https://gracefulsecurity.com/extracting-flash-memory-using-jtag/
-   https://gracefulsecurity.com/tag/hardware-hacking/
-   https://www.blackhillsinfosec.com/how-to-hack-hardware-using-uart/
-   https://www.blackhillsinfosec.com/tag/hardware-hacking/
-   https://blog.rapid7.com/2019/02/20/iot-security-introduction-to-embedded-hardware-hacking/

## Pentest engagement step 4 – reaching the objective of the attack

The objective was well chosen for this engagement: *find any avenue of attack, any threat vector or vulnerability we can leverage, to get into their industrial network that can interrupt production or steal sensitive data.* This resembles a real attacker's objectives and covers a broad spectrum of necessary testing.

Throughout the pentest engagement, we have found lots of evidence that can allow an attacker to achieve these objectives. All we need to do now is write a report that reflects our findings and summarizes how the successful attacks can be leveraged by an attacker. This helps them gain a foothold in the industrial network and disrupt production or steal sensitive information.

## Summary

In this chapter, we discussed the many facets of conducting OT-centric penetrating testing engagements. We looked at how the ICS Cyber Kill Chain can help us perform a more well-rounded pentest. We also discussed how making an approximation of the ICS under consideration (a testbed) is the safest way to perform the scanning, probing, and attacking tasks of a pentest. We then engaged with Company Z's ICS to perform some penetration testing activities. You should now be familiar with the terminology, tools, techniques, and procedures for performing your own penetration testing assessments.

In the next chapter, we are going to look at how to prepare for when things go wrong by discussing incident response as it pertains to the ICS environment.

# Section 5:
Industrial Cybersecurity –
Incident Response
for the ICS
Environment

In this part of the book, we look at incident response and incident recovery tools, techniques, activities, and procedures as they relate to the ICS environment.

This section comprises the following chapter:

-   *Chapter 18, Incident Response for the ICS Environment*

# Chapter 18
## Incident Response for the ICS Environment

Our journey through industrial cybersecurity monitoring and validation is almost over. So far, we have mostly concentrated on how to verify the security of our industrial environment. In this final part of this book, we are going to discuss how to prepare for mayhem and how to handle things in case we find ourselves facing security-related incidents, as well as what to do when things go wrong.

In this chapter, we will be discussing the ins and outs of setting up and maintaining an incident response plan. We will outline the phases, activities, and processes of incident response as it relates to the industrial environment. You will learn how by maintaining clear-cut and detailed processes and procedures around the incident response plan, you will set yourself up for success when dealing with security incidents.

We will cover the following topics in this chapter:

-   What is an incident?
-   What is incident response?
-   Incident response processes
-   Incident response procedures

# What is an incident?

To recap what we discussed back in Chapter 5, Introduction to Security Monitoring, an incident can be described as an occurrence of an event. Therefore, a security incident can be described as an occurrence of a security-related event – something that's happening to the security posture of the ICS environment that we are interested in and want to detect. To detect interesting security incidents, we need to be monitoring for them with security monitoring tools and practices.

The following are some examples of security incidents:

-   Executing malicious code on a system
-   Impaired or disrupted availability of ICS systems or equipment (DCS, SIS, PLC, HMI, SCADA, and more)
-   Malicious or damaging interaction with computing or production resources
-   Unauthorized changes to a Programmable Logic Controller (PLC) or Human Machine Interface (HMI) program
-   Unauthorized access to a building or restricted area of a building
-   Unauthorized access to computer systems
-   Unauthorized use or abuse of software or data
-   Unauthorized changes to production systems, software, or data
-   Loss or theft of equipment storing production-related data
-   Distributed Denial of Service (DDoS) attacks
-   Interference with the proper operation of Operational Technology (OT) or production resources
-   Excessive failed login attempts

With that in mind, let’s discuss ways to prepare for and handle incidents by defining incident response processes and procedures.

# What is incident response?

Incident response (IR) is the process by which an organization prepares for and handles a (cyber)security-related incident such as a data breach or cyberattack. This includes efforts taken to manage the fallout/consequences of the incident. The goal of implementing an IR program/process is to be able to effectively manage incidents to the point where the damage and impact of the incident is limited, and both the recovery time and costs, as well as collateral damage, which includes the organization's reputation, are kept to a minimum.

Organizations should invest time and resources in implementing a comprehensive incident response plan that can help them prepare for, and deal with, security incidents. The IR plan should put definitions around what is considered an incident for the organization and provide clear, guided processes and procedures to be followed in case an (OT) security incident occurs.

The following sections detail the two distinct aspects of IR, namely IR preparation and IR handling. The text is derived from the following sources and is meant to be used as a template policy:

-   **Blue Team Handbook: Incident Response Edition version 2.2**: `http://www.blueteamhandbook.com/`
-   **CSS2017 Session 7 SANS Training - Incident Handling Process**: `https://www.youtube.com/watch?v=76fuTjzuiLg`
-   **NIST SP 800-61 revision 2**: `https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final`

With the definition of incident response explained, let's start looking what an IR plan entails.

# Incident response processes

There are two distinct aspects to incident response:

-   Incident response preparation
-   Incident response handling

The incident response preparation process occurs periodically without any identified incident. The incident handling process is triggered when an incident is detected.

# Incident response preparation process

The goal of the preparation phase of incident response is to prepare the **Industrial Control System (ICS)** or OT security team to handle incidents efficiently and effectively. By its nature, the preparation phase occurs separately from any identified incident or event.

Creating and maintaining an incident response policy document (we want to record and track our IR processes and procedures), and the related processes and procedures documents, is at the heart of the preparation phase. The incident response preparation process includes both tasks intended to help prevent incidents and tasks intended to streamline incident detection and response.

It is recommended to follow the incident response preparation process periodically – once a year at a minimum.

# Incident response preparation process flow

The following flowchart describes the major steps of the preparation process. This process and its accompanying procedures assist the organization in preparing to handle future incidents:

![](img/c8ffa9959721097701584fcc95101362_718_0.png)

Figure 18.1 – Incident response preparation process flow
