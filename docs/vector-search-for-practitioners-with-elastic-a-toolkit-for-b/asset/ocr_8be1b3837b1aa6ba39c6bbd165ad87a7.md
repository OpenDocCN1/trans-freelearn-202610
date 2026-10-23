# Different types of phishing attacks

Let’s look at the different forms of phishing depending on the context or the persona it targets:

+   - **Spear phishing:** This is a targeted form of phishing where the attacker has done their homework. They personalize their emails with the target’s name, position, company, work phone number, and other information to make the email seem less suspicious. The goal is to trick the recipient into believing that they have a connection to the attacker.
+   - **Whaling:** This is a form of phishing attack that targets high-profile employees, such as CEOs or CFOs, to steal sensitive information from a company, as these individuals often have total access to sensitive company data.
+   - **Clone phishing:** This type of phishing attack involves taking a previously delivered email with its content and recipient address(es) and creating an almost identical or cloned email. The original attachment or link in the email is replaced with a malicious version and then sent from an email address spoofed to appear to come from the original sender.
+   - **Pharming:** Pharming is a more complex method of phishing that involves malicious code and fraudulent websites. The attacker installs malicious code on the user’s computer or server that redirects clicks made by a user in their web browser to a fraudulent website without their consent or knowledge.
+   - **Smishing and vishing:** These terms are used to describe phishing attacks that take place over SMS text messages (**smishing**) and voice calls (**vishing**).
+   - **Business Email Compromise (BEC):** This is a sophisticated scam that targets businesses working with foreign suppliers and/or businesses that regularly perform wire transfer payments. The scam is carried out by compromising legitimate business email accounts through social engineering or computer intrusion techniques to conduct unauthorized transfers of funds.

Each of these types of phishing attacks has its unique characteristics, but they all share the same goal: to trick the victim into revealing sensitive information.

While phishing can be carried out in different ways, this chapter will specifically focus on email phishing, which is one of the most common and pervasive vehicles for phishing attacks.

Email phishing is a significant threat because of its ability to reach a vast number of potential victims at once. It’s a method favored by many cybercriminals due to its simplicity and effectiveness. Attackers can send out thousands of fraudulent emails with the click of a button, and all it takes is one unsuspecting recipient to take the bait for the attack to be successful.

We will explore how they are structured, why they are effective, and most importantly, how we can use tools such as ELSER from Elastic to detect and prevent them. We will use a real-world dataset of emails, labeled as **ham** (legitimate) or **spam** (phishing), to demonstrate how these tools can be applied in practice.