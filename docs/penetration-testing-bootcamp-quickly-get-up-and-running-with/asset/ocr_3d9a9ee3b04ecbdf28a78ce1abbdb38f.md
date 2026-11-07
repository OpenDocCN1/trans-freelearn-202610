After this step, I refine all the vulnerability evidence one more time. This time into the actual vulnerability. For instance, taking one of the preceding categories, client-side vulnerabilities, I would break it down into an actual issue at hand:

+   - Web application vulnerabilities
+   - Client-side vulnerabilities
+   - Reflected XSS vulnerabilities
+   - Stored XSS vulnerabilities

Then, I will take all the relevant evidence and put it in the preceding classifications. This will allow me to group common vulnerabilities if they exist in more than one place. A great example of this would be that I have multiple different web applications in different locations all susceptible to a stored XSS attack.

Once all the evidence is organized, you will see that it truly makes the process of generating the report much easier.

# Importance of defining risk

Defining risk is pretty much the entire reason why penetration tests are performed. Risk defines how impactful or unaffecting a particular vulnerability is within a defined environment.

Typically, the risk is defined in accordance with NIST SP 800-30. This document is a great guideline for conducting risk assessments. It contains all the fundamentals of risk information, including the risk management process, risk assessment, important risk concepts, and applications for risk assessments. It then dives into the actual risk assessment process from preparing to conducting, to communicating, and finally maintaining the risk assessment. Keep in mind, risk can be specific to an individual, a company, or situation. For example, if a customer has a default instance of MySQL with default passwords running on a development box somewhere that is segmented from everything else, it will not be the same risk level as that same instance running in production.