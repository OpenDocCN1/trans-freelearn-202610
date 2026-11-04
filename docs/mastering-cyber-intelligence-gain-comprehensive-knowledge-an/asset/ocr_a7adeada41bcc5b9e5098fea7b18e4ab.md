Two anti-viruses have flagged the IP address as malicious. The IP address location indicates Russia as the origin. You can navigate to the RELATIONS tab on VirusTotal to explore the graph summary tool (you will need to sign up to do this).

6. Perform passive DNS queries to check other domains linked to the same IP address or vice versa. Some passive DNS providers offer application programming interfaces (APIs) for integration with other internal systems. Using free protective DNSs (PDNSs) such as https://passivedns.mnemonic.no, we get two DNS queries associated with the preceding IP address, as shown in the following screenshot:

| Query | Answer | First seen | Last seen |
|---|---|---|---|
| mail.resoh.ru | 194.150.215.153 | 2020-09-11 06:36 | 2020-09-16 22:49 |
| resoh.ru | 194.150.215.153 | 2020-09-11 06:36 | 2020-09-14 12:00 |

Figure 10.28 – Mnemonic IP address resolution

We obtain more domains linked to the 194.150.215.153 IP addresses: mail.resoh.ru and resoh.ru. The two domains are linked to spam, malicious, and phishing activities.

7. Get more information about the indicators—use the whois <domain name>, host <IP-address>, or nslookup <IP or domain> command to get more information about the indicators. Find information such as the registrant name, organization, and email address. This helps understand who owns the domain or the IP address.

We have analyzed the malicious files found in the system, retraced the email used for initial access, and pivoted through external sources to scan the newly found email indicators. In the next section, we look at gathering early indicators.

# Gathering early indicators – Reconnaissance

As CTI analysts, we need to identify the activity that led to the targeted phishing (spear-phishing) attacks. Use OSINT tools such as Maltego (https://www.maltego.com/) to determine where the organization's employee email addresses are exposed.