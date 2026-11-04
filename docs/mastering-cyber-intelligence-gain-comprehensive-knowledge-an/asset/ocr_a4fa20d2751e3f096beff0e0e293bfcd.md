# Use case – ATT&CK model mapping

**Scenario and objective:** An adversary has been hired by the competitor to steal business information from the ABC company. Because this is business espionage, the adversary has been given the necessary resources to orchestrate the attack. The adversary passed the information gathering and collection phases without being detected. During the information gathering phase, the adversary came across joe@abc.co.za on LinkedIn. Joe is a fan of soccer:

+   - Tactic mapping: *Initial access.*
+   
+     They initiate an attack against the organization. They choose to access the system using spearphishing (social engineering) by using the system admin's email. They prepare and weaponize (embed malicious code) an excellent sports journal in PDF format. The journal is sent to Joe with a spoofed sender email address.
+   - Technique mapping: *Phishing.*
+   
+     Phishing electronically delivers a malicious trojan, link, or service. In this case, the adversary could use any of the three mentioned phishing methods to access the system. The analyst maps the activity to the phishing technique of the ATT&CK framework.
+   - Subtechnique (method) mapping: *Spearphishing attachment.*
+   
+     Joe receives an email from someone pretending to be Alice with a legit ABC company email domain (alice@abc.co.za). The email contains a PDF that states, "Possible transfer of Kylian Mbappe to Liverpool FC. Hey Joe, did you see the latest rumor about the arrival of Mbappe to Anfield? Check the attached article." The analyst maps the activity to spearphishing using the email attachment. Joe, excited, opens the attachment and reads the article. The system is compromised.

# How to detect such an attack

The ATT&CK framework advises using intrusion detection systems, email gateways, or detonation chambers to protect from and detect phishing programs that use malicious attachments. However, because such protection is based on traffic patterns and packet analysis, adversaries might use defensive evasion techniques to bypass network obstacles. Hence, it strongly recommends using antiviruses (auto-scanning emails with attachments), endpoints, and network sensing (to detect when the PDF document is opened).