+   - Security Onion 2 upgrades to Elastic Stack 7.x and supports the **Elastic Common Schema (ECS)**.
+   - Security Onion 2 completely replaces the PF_RING with AF_PACKET unsigned kernel module.
+   - In Security Onion 2, Suricata completely replaces Snort.
+   - Security Onion 2 removes Sguil, Squert, capME, and PHP.

Security Onion is a platform that allows you to monitor your network for security incidents and alerts. It can be spun up in minutes to start monitoring the network of small environments and allows advanced users to scale things up and deploy distributed systems that can be used in large-enterprise, network-type environments. Security Onion has full packet capture capability. It includes IDS through Snort or Suricata for rule-driven intrusion detection. It uses Zeek for event-driven network event monitoring and intrusion detection, and uses Wazuh host-based agents for HIDS. All this functionality is ready to roll from the minute you spin up the VM.

Security Onion allows us to combine all the common passive security monitoring tools we talked about in the previous section—and more—into one solution. I deploy a Security Onion VM (or system) as part of all my security architecture engagements.

We will be setting up Security Onion 2.3.21 in Exercise 1 of this chapter and we will be using the tools to perform many functions of the threat-hunting exercises in the third part of this book, Part 3 – Threat Hunting.

# Exercise 1 – Setting up and configuring Security Onion

As the first exercise for this chapter, we are going to deploy and configure a Security Onion VM. We will be extending the functionality of this Security Onion appliance throughout the remainder of this part of the book and will be using its analytics and search capabilities extensively throughout the third part of this book, Part 3 – Threat Hunting.