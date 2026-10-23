With the ability to track entities such as IP addresses or users that have been previously linked to security alerts, we can gain insight into potential security risks. Now that we have an understanding of how data is gathered and saved, we can establish monitoring guidelines.

# Analytics rules

Let us start by looking at building our own analytics rules. For the example here, I will be using the Azure portal and will provide a PowerShell example at the end.

As mentioned in Chapter 1, Ransomware Attack Vectors and the Threat Landscape, a typical entry point for ransomware attacks can be based upon RDP connections. I want to create two different analytics rules that will use two data sources to look at signs of compromise from a fictional machine that is publicly available on the internet using RDP:

+   - Security events (collected from DCRs in Azure Monitor)
+   - VM insights (collected from the dependency agent)

For the sake of this example, we will look for connection attempts where RDP has been successful and create an alert rule based on that. We will also exclude known IP addresses so that alert rules are not created for known traffic.

Start by going into the Azure portal, then Microsoft Sentinel, and then into the Analytics pane. From there, click Create Scheduled Query Rule, which will initiate the wizard.

Then, enter the following information in the first part of the wizard:

+   - **Name:** Successful Logon RDP - Security Events
+   - **Description:** Looking for signs of compromise where someone has successfully logged onto a machine with RDP
+   - **Tactics and techniques:** Initial Access (T1078) and Brute Force (T1110)
+   - **Severity:** Medium
+   - **Status:** Enabled

This information can be seen in the following screenshot: