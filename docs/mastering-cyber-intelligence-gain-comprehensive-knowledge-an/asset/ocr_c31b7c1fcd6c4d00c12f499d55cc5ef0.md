# Use case – Lockheed Martin's Cyber Kill Chain model mapping

**Scenario and objective:** A group of hackers has decided to take down the ABC insurance company after voting for and passing policies that customers are unhappy with. Their sole objective is to take down the business for as long as they want in order to push the company to remove the policies.

The group harvests email addresses, domain information, IP ranges, and SSL certificate information from the public internet. They come across the email address of Vanessa, the IT admin, on LinkedIn and her life routine on Instagram. They also discover the company router that faces the internet and the company's website, where users can register for insurance, contact the company, and leave comments.

+   - Cyber Kill Chain mapping: *Reconnaissance.*
+     **Countermeasure:** Continuously collect website access logs for search activities, collect traffic analytics from web administrators, and detect unique and suspicious recurrent browsing behavior.
+     The group selects the **Metasploit Framework (MSF)** as their tool to generate a set of malware embedded in a document to send to Vanessa.
+   - Cyber Kill Chain mapping: *Weaponization.*
+     **Countermeasure:** Know the most used malware generation platforms, perform malware analysis, and identify and detect malware payload types and their origins.
+     The group is ready to use two techniques to compromise the system. They send an email to Vanessa and also prepare a USB stick with the prepared weapon that will physically be installed on Vanessa's personal computer.
+   - Cyber Kill Chain mapping: *Delivery.*
+     **Countermeasure:** Understand the upstream infrastructure, understand the people and their roles, understand what information can be accessed by which user, and frequently perform forensics reconstruction.
+     Paul is a member of the group. He befriended Vanessa a month before and has been able to visit her at her place. One of the ways to deliver malicious code is for Paul to convince Vanessa to open the document on the USB stick. Alternatively, Paul can send an email with the attachment to Vanessa and she will open it.