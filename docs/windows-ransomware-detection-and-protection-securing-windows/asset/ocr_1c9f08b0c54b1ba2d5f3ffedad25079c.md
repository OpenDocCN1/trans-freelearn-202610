+   - **States an important level of urgency** – Mentioning that the user needs to take urgent action such as ensuring that their account will not get locked.
+   - **Links within the email that redirect you to unfamiliar websites and domains** – While they might be redirected to a website that might look familiar, the URL is from a different domain.
+   - **Unsolicited attachments** – These might be executables or Word documents that contain malicious payload to initialize the malware bootloader.
+   - **Changes in email addresses** – Either the email is not official or contains smaller changes compared to the official domain name. These might be minor changes such as Microso tt.com instead of Microsoft.com.
+   - **Spelling or grammar mistakes** (especially if they are forged in your native language) – There are many cases where attackers use generic text and then use Google Translate to translate the email for different recipients within different countries.

Over the last few years, attackers have also become better at crafting emails that appear to come from the same domain name. This can be done by spoofing the sender’s address. Alternatively, they are using known URLs to send malicious content such as Microsoft OneDrive or known services such as Dropbox or Google drive. In addition, you also have more targeted phishing attacks, also known as spear phishing, where the attackers send specialized phishing emails to specific users or organizations.

Once I was working with a customer who was attacked with a spear-phishing attack where the CFO was compromised. This customer was running Office 365, so the attacker had predefined scripts that were used for collecting and downloading all emails from their account and then added an auto-forward script to forward all emails to an external address.

After a couple of weeks of gaining access, the attacker had good knowledge about the CFO and how they interacted with other colleagues via email. The attacker then used that information to create a new email with a malicious payload that was sent to the finance controller who ran the malicious payload on their machine. This started a ransomware attack on their infrastructure.

Before we go into the details, I want to cover the overall attack vectors and weaknesses we have in email:

+   - Spoofed emails (more specifically, the MAIL FROM info) or coming from domain names that might be like the company domain name.
+   - Phishing emails from malicious domains. Most of these domains are new and used only for phishing campaigns for a brief period.
+   - Phishing emails sent from known domains such as Office 365 or Gmail.
+   - Phishing emails containing malicious content or links to malicious URLs.

Overall, the main goal when it comes to email security is to ensure that we can verify the sender, sender domain, body of the email, URLs, and any attachments.