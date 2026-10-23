![](bbox=[0.31, 0.103, 0.673, 0.442])

Figure 1.3 – OAuth permission screen for a phishing attack

If the user clicks on Accept, the attacker will be able to get access to their profile in Office 365, which might also include access to emails and files, depending on what kind of permissions are granted.

# Access through brute-force attacks

One of the most common attack vectors that we see is brute-force attacks on misconfigured services, such as attacks on a Windows server that is publicly exposed with Remote Desktop Protocol (RDP) enabled. This can also be any exposed service that has weak security mechanisms, such as a lack of MFA, which RDP has by default, making it susceptible to attacks.

With one customer I was working with, the initial point of compromise was an exposed Windows Server in Azure that had a public IP address and RDP enabled. Since the machine was also domain-joined and had a weak local administrator account password, it did not take a lot of time for the attackers to guess the correct combination of usernames and passwords and gain access to the environment.

As we have also seen that in cloud-based environments, attackers often have a predefined set of credentials that they use when they are doing brute-force attacks for known IP ranges. Azure environments typically use a combination of usernames such as AZADMIN/AZUREADMIN/AZURE with different combinations of passwords. An automated attack typically starts within minutes of when the machines come online in Azure.