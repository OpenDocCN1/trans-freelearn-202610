
# Creating Your Policy Framework

Creating your policy framework can be intimidating. *Table 10.1* only shows a small snippet of what should be considered for policy documents, not to mention that the framework should include an exhaustive list of controls when starting out, as you may only have a handful of controls.

There are a few lists that can assist you when starting out creating your own framework. The Open Policy Framework is an open source project aimed at assisting policymakers. It lays out several control families that you can use to start your own framework. These include the following:

## Control Families

### Information Security

The information security control family is aimed at data protection and acceptable use. Standards documents could include the acceptable use policy, encryption, data disposal, and **information lifecycle management (ILM)**. The information security control family is meant to control and secure the organization’s sensitive information.

### Asset Management

There is a reason that the Center for Internet Security states that asset management is the most important control. *You cannot protect what you cannot see!* The organization should have sound controls over asset management, including the identification of software and services that also run on the IT resource. If you do not know that the asset or software exists, would you have a game plan to patch or mitigate any risks?

### Human Resources

This control family is meant to explain what to do with an employee’s access and company-owned possessions during their employment. When a new employee is hired, the manager must go through a series of steps to ensure that the new hire has everything they need to do their job functions. This may include receipt of a new computer, a company-owned cell phone, or peripherals.

When an employee is transferred to a different position, you will need to have standards that explain what to do with those possessions or access to IT resources. You want to ensure that access creep is not introduced into the environment by allowing the employee to have access to the same IT resources they previously had if their new job does not require it.

We also want to create standards for what to do when the employee is offboarded. What do you do if an employee is let go or leaves freely on their own? This all gets planned out in the offboarding standard that must be written. These standards could also be written in coordination with Human Resources to ensure that both departments are aligned with what is stated in the standard.

### Physical Security

There are plenty of physical security controls that could be discussed in this section, such as **closed-circuit television (CCTV)**, physical barriers, man traps, key card door access, and many others. These controls shall have their own control family to be tracked. Several frameworks also mention physical security in their documentation as well. These controls should be depicted in the policy documents that are written for your organization.

### Access Control

Least privilege and separation of duties, password standards, use of **multifactor authentication (MFA)**, and identity proofing are controls that fall under access control. Access control is more than just controlling the level of access a user has on a given device. While least privilege and separation of duties are important, there are several other standards that should be developed.

We should look at developing standards around the types of MFA that are allowed in the environment. For example, several MFA vendors provide differing levels of access, such as push notifications, U2F, **one-time passcodes (OTPs)**, to even landline callbacks and SMS. Each of these has differing levels of risk associated with their authentication. Some MFA mechanisms have additional risks involved, such as using SMS when there is a fear of SIM swapping or push fatigue when using a mobile application.

### Incident Management

Incident management can include the Recover and Response functions from the NIST CSF core. Controls should include the creation of your security incident response team, a call tree in the event of an incident, how to perform investigations, documenting the incident, and documenting lessons learned.

This is also where you would align your IR runbooks with the controls. For example, if you have a response plan for how to overcome an account takeover, you should also have a runbook to assist the procedure and visualize the response.

### Business Continuity/Disaster Recovery

We have discussed how to respond to a given incident and how to recover from it. Those policies, standards, and procedures for how to conduct this shall be in this family of controls. Standards that should be included in this family include backup and testing, business continuity planning and testing, transitions to hot or cold sites, and recovery and documentation.

### Security Awareness and Training

You may think that security awareness and training are synonymous with each other; however, they are not. According to NIST, security *awareness* is performed through observation, whereas *training* is more focused on security topics. This is something that is done year-round and not just a project that is set aside for security awareness month.

Security awareness can be done through banner displays or videos set up on TVs. These are meant to be viewed by staff members passing by. Security training is also meant to highlight cybersecurity; however, it is done through more intense and structured means. Security training is focused on the job role or function rather than being just a high-level banner that is viewed. For example, security awareness would highlight the need for extra scrutiny when interacting with a phishing email, whereas training may be required for email administrators who configure the secure email gateway.

### Secure Network Configurations

This control family is meant to highlight the need for secure network configurations. This should include site-to-site VPN connectivity, client-to-site VPNs, and hardening firewalls or other security appliances, such as IDS/IPS. This also highlights the configurations required for VPN setups. For example, many organizations have moved from split-tunneling client-based VPN connections and moved to full tunnels. If you allow one but forbid the other, this should also be in the standard.

Secure network configurations should also include firewall configurations. Do you deny all and allow by exception in the rule base? Should the firewalls require MFA for authentication? What protocols are never allowed to come through a firewall? These should all be discussed.

### Secure Configuration and Management

All IT resources and software should be hardened against a standard. There are several standards that highlight the need for hardening a system; however, the best-known standard is the Center for Internet Security benchmarks documents. Hardening means that you remove or change all default settings and credentials on a system. Remove any unwanted or unused services from the system and ensure that proper security configurations have been performed.

### Risk Management

We have discussed risk quite a bit throughout the book. We now must take everything that we have learned and apply that to policies and standards. NIST has several documents that cover organizational risk: NIST SP 800-30, 800-37, and 800-39. The International Organization of Standards has also developed ISO 27005, which also introduces strategic ways of dealing with risk.
