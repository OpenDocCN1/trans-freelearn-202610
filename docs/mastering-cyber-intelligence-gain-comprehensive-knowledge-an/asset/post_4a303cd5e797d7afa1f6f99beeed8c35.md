
![](img/d47f26598025b2da017dd41fcbb6e602_471_0.png)

Figure 15.8 – The Anomali STAXX threat intelligence platform's login page

After logging in, the welcome page displays a STIX/TAXII server configuration wizard, which we will explore in the next step.

4. Configure the STIX/TAXII server: You can configure your own STIX/TAXII server if you have one. Alternatively, you can use the default STIX/TAXII server from Anomali, LIMO. Click on USE ANOMALI LIMO and check or uncheck the proxy server configuration, depending on your system configuration. In this exercise, we will leave it unchecked, as shown in the following screenshot:

![](img/d47f26598025b2da017dd41fcbb6e602_471_1.png)

Figure 15.9 – The Anomali STAXX welcome page and the STIX/TAXII server wizard

Click on **LET'S TRY STAXX** after the successful setup of LIMO. You must accept the terms and conditions of the license agreement. Note that you can configure additional STIX/TAXII servers after the initial setup.

- 5. **View the STIX/TAXII observables**: STAXX provides an insightful dashboard with observables for a selected period. You can drill down to each indicator or observable for more information. The following figure shows the drill-down view of the dashboard:

![](img/d47f26598025b2da017dd41fcbb6e602_472_0.png)

Figure 15.10 – The Anomali STAXX drilldown view from the main dashboard

The preceding figure shows the IP addresses as observables with the severity, **Traffic Light Protocol (TLP)**, feed (or source), and so on. Click on the observable to view its details. You may be required to register for free with Anomali using the `https://bit.ly/3IJ1pox` link to view observables' details. The **ACTIVITY** page provides mechanisms to search for observables and apply filters.

6. **Configure observables**: Anomali STAXX allows a CTI analyst to share observables with the community. (1) Click on **IMPORTS** on the menu bar to bring up the observable import page. (2) Fill in the IOCs extracted from the intrusion analysis. The following IOCs are added: `125.19.103.198`, `46.158.5.140`, `resoh.ru`, and `12cf6583f5a9171a1d621ae02b4eb626`. (3) Select the threat type. Here, we select **malware**. (4) Set the confidence level to 70%. (5) Set the severity to **High** as it was an attack on the system. (6) Add one more tag for the activity. We will add the following tags: `suspicious-IP`, `suspicious-domain`, `phishing-malware`, and `victim-finance`. (7) Set the appropriate TLP color. Let's set it as **Red**. (8) Click on **Import** to import the observable information. The observable configuration is shown in the following figure:

![](img/d47f26598025b2da017dd41fcbb6e602_473_0.png)

Figure 15.11 – The Anomali STAXX observable configuration page

After importing the observable related to our analysis, the approval or rejection window appears, as shown in the following figure. Click on the Approve Import button after careful verification of the data inserted:

![](img/d47f26598025b2da017dd41fcbb6e602_474_0.png)

Figure 15.12 – The Anomali STAXX observable approval page

After approval, the status will be changed from Pending Approval to Approved, and you can push the observable to remote or public STIX/TAXII servers.

7. **Share the observables**: To push the observable to STIX/TAXII servers, (1) select all the observables or indicators you would like to share, and (2) click on the **Push Observable** button, as shown in the following figure:

![](img/d47f26598025b2da017dd41fcbb6e602_475_0.png)

Figure 15.13 – The approved observables and sharing configuration

The collection information page opens. You need to insert the name, description, pushing frequency, and most importantly, the STIX/TAXII site to which the IOCs are pushed. The PUSH OBSERVABLES window is shown in the following figure:

![](img/d47f26598025b2da017dd41fcbb6e602_476_0.png)

Figure 15.14 – The STAXX PUSH OBSERVABLES window

Our IOCs are pushed to the LIMO STIX/TAXII server, and the rest of the community can use the shared threat intelligence to enhance their security posture.

Anomali STAXX is a robust STIX-based CTI sharing platform. This section has shown the basic steps to set up the Anomali STAXX client and connect to a STIX/TAXII server for threat intelligence sharing. For more information about collecting, configuring, and administrating your Anomali STAXX client, refer to the Installation and Administration Guide document that comes with the OVA file.

Note that level-one (entry-level CTI) and level-two (intermediate-level CTI) organizations can leverage STAXX and the Malware Information Sharing Platform (MISP) for cost-effective threat intelligence applications.

## Use case three – sharing intelligence through a platform

Sharing threat intelligence is crucial in the global fight against cyberattacks. CTI analysts share IOCs to expose threat actors' activities. Threat intelligence sharing platforms are vital for effective intelligence collaboration. They also facilitate process automation.

**Use case scenario:** For the intrusion analysis performed, you are requested to publish the findings to one or more platforms to empower the security community. It will allow the community to enrich and update threat and IOC databases.

**Use case objective:** The objective of the use case is to demonstrate threat intelligence sharing through open-source intelligence-sharing platforms. We will only select one sharing platform. However, the CTI analyst needs to investigate others.

The IOCs that we have to share include two IP addresses, one domain, and one MD5 hash key: `125.19.103.198` and `46.158.5.140`, `resoh.ru`, and `12cf6583f5a9171a1d621ae02b4eb626` respectively.

**Platform used:** AlienVault **Open Threat Exchange (OTX)**

AlienVault OTX was introduced in *Chapter 12, SIEM Solutions and Intelligence-Driven SOCs*. A CTI analyst can leverage it to share threat and intrusion analyses as *pulses*. The following steps are used to solve this use case. Note that some of the steps will not be repeated, as we assume that the CTI analyst completed them in *Chapter 12, SIEM Solutions and Intelligence-Driven SOCs*:

- 1. **Log in to AlienVault OTX:** Navigate to https://otx.alienvault.com and log in to OTX using your valid credentials. If you do not have valid credentials, open an account for free on the provided link.
- 2. **Create a new pulse:** (1) Click on **Create Pulse** on the menu bar to create a new pulse. The pulse creation page opens. Select one of the methods to add IOCs to the pulse. You can (2) enter text or insert a URL, (3) attach files, or (4) manually add indicators. The pulse creation page is shown in *Figure 15.15*.

For this use case, we will choose the text input type. Copy and paste all the IOCs into the field (4), as shown in the figure. The OTX extraction tool will automatically extract the IOCs. After adding the text, (5) click on the **Extract Indicators** button to populate the IOCs.

Figure 15.15 is truncated for visual clarity purposes. Only the relevant part is shown for this point:

![](img/d47f26598025b2da017dd41fcbb6e602_478_0.png)

Figure 15.15 – The AlienVault OTX pulse creation page – the input fields

3. Validate the populated IOCs: Scroll down to the IOC section to view the populated indicators on the same page. The output is shown in the following figure:

![](img/d47f26598025b2da017dd41fcbb6e602_478_1.png)

| TYPE | INDICATOR | ROLE | TITLE |
|---|---|---|---|
| IPv4 | 46.158.5.140 | scanning_host |  |
| FileHash-MD5 | 12cf6583f5a9171a1d621ae02b4eb626 |  | MS_Visual_Cpp_2005 |
| FileHash-SHA1 | 61ed2778d7669d6838523369fd04278626303362 |  | MS_Visual_Cpp_2005 |
| FileHash-SHA256 | 5b136147911b04f1f012ce82dfd24c4e2c79553b65d3240ecea2dcab4452dcb5 |  | MS_Visual_Cpp_2005 |
| IPv4 | 125.19.103.198 |  |  |
| Domain | resoh.ru |  |  |

Figure 15.16 – The AlienVault OTX IOC view for the populated IOCs

Some interesting facts to consider are that we input four IOCs, but OTX populated six IOCs. It automatically added the SHA1 and SHA256 keys of the malicious file. The hash keys are automatically given titles: MS_Visual_Cpp_2005, and the platform suggested seven other IOCs related to the extracted IOCs, as shown in the following figure. OTX automatically correlates the IOCs with threat intelligence shared by other CTI analysts:

| TYPE | INDICATOR |
|---|---|
| URL | http://125.19.103.198:8080/zb/v_01_c/in/ |
| URL | http://125.19.103.198:8080/zb/v_01_a/in/ |
| URL | http://125.19.103.198:8080/zb/v_01_a/in |
| URL | http://125.19.103.198:8080/ |
| Domain | semelyontour.ru |
| Domain | dpasssjufjkaksss.ru |
| Domain | copsdifbnsasdf.ru |

Figure 15.17 – Additional IOCs automatically correlated with the extracted IOCs

Two additional domains and four URLs are linked to our IOCs. You should hover on each suggested IOC to add to the extracted IOCs. You can also use the newly found IOCs to scan the ABC Company's system for additional malicious activities.

- 4. **Fill in the pulse details**: OTX allows you to add details such as the TLP color, the title of the pulse, the description, the adversary, tags, groups, industries, targeted countries, malware families, and the ATT&CK techniques' IDs.
