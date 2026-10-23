# Hosit-based agents

The final method of passively collecting security monitoring data is by means of **host-based agents**. An agent is a small application that can be installed on the endpoint system. Its main function is to (periodically) grab relevant information or monitor the system for relevant actions, changes, or activities, and then send any discoveries to a central server to be processed by the security solution that deployed the agent.

Many security vendors have offerings that include agent technology. The agents range in capabilities, footprint, and size, depending on how much of the security efforts is performed by either the agent or the central solution. For example, an asset management solution such as **SolarWinds Server & Application Monitor (SolarWinds SAM)** can deploy a tiny agent, hardly taking up any resources, as its functions are limited to inventorying and cataloging the endpoint. The rest of the functionality of the asset management solution is done by the central server component. A distributed **antivirus** **(AV)** solution such as **Symantec Endpoint Protection (SEP)**, on the other hand, deploys a much larger agent (the AV component that gets installed on the system) as it needs to do most of the heavy lifting on the box. The central server component of the SEP solution is mostly concerned with management and reporting.

As part of *Exercise 1 – Setting up and configuring Security Onion* later in this chapter, we will be deploying a host-based agent. The agent in question is **Wazuh**. Let’s briefly discuss what Wazuh’s capabilities are.

# Wazuh

This is from the Wazuh site (`https://wazuh.com`):

> "Wazuh is a free, open source and enterprise-ready security monitoring solution that combines threat detection, integrity monitoring, incident response and compliance functionality into a single agent."

> Wazuh addresses the need for continuous monitoring and response to advanced threats. It is focused on providing the right visibility, with the insights to help security analysts discover, investigate and response to threats and attack campaigns across multiple endpoints.

> Wazuh helps detect hidden exploit processes that are more complex than a simple signature pattern, and that can be used to evade traditional antivirus systems. In addition, the Wazuh agent provides active response capabilities that can be used to block a network attack, stop a malicious process or quarantine a malware infected file."