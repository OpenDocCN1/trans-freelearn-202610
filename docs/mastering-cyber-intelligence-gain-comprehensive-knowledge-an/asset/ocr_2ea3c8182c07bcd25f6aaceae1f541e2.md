Let's look at the process in detail in the following points. The process needs to be executed by skilled professionals. We explain the process using the following scenario.

You are a threat-informed pen tester and a CTI analyst by default. You have been requested to perform intelligence-led pen testing for the ABC company. ABC is a high-tech company based in South Africa. They supply software and hardware solutions to the government. You need to perform the following tasks:

+   1. **Planning and preparation:** This step aims to gather the global and CTI requirements for pen testing. The intelligence-led pen tester sets objectives and defines the end game. All decisions are discussed with the organization's strategic team. Because it is an intelligence-led offensive test, the scope of the attack is not defined at this level. Planning and preparation are essential to the success of the testing.
+   2. **Threat intelligence collection:** Collect threat intelligence reports, analysis, and data from the CTI team. Threat intelligence helps define the target specifics (assets, applications, industry, systems that threat actors may target) and understand adversaries' behaviors. Its output shapes the real-world attack scenarios that the tester must mimic. The end game is to define *real-world test cases and assessment setups.*

An adjustment of the planning phase must follow intelligence collection. After defining the test cases, the threat-informed pen tester analyzes the adversaries' TTPs, selects the appropriate tools and methods, and defines the rules of engagement to prevent potential damages during the test.

# Reviewing the Collected Intelligence

During the intelligence review, you identified *APT12 group or Panda* as the adversary. APT12 (https://cybermaterial.com/apt12/) has targeted high-tech companies, media outlets, and governments (which fits your organization profile), and their primary intents are *data and information theft* and *espionage*. Their method involves *weaponizing Word* and *PDF documents, and executable files*. Some of their techniques include the following:

+   (1) **Phishing** (initial access tactic)
+   (2) **Client execution** using browser URLs, opening malicious documents, or third-party applications such as Adobe Flash (execution tactic)
+   (3) **Process injection** (persistence tactic)
+   (4) **Dynamic resolution** and **web services** for communication and security evasion (command and control tactic)

Examples of malware associated with APT12 include **Ixeshe**, **RapidStealer**, **Iheate**, and **WaterSpout.**