
So, where are they coming from? How did the attackers find the user account that they are trying to log in to?

In many cases, attackers have different scrapers and scripts that crawl through websites to collect all the email addresses they can find. This can also include email addresses that were collected from an existing data breach.

A good way to see where credentials have been stolen from is by checking the affected email address at https://haveibeenpwned.com. The following screenshot shows the result where the email address was not breached:

![](img/7b49a27a3c2adc52d097f2e77f3bf394_37_0.png)

Figure 1.8 – Information showing that no user information was found

However, if the information is found in one of the data breaches that the service has access to, the following result will appear:

![](img/7b49a27a3c2adc52d097f2e77f3bf394_38_0.png)

Figure 1.9 – Information showing that user information was found in a breach

In some cases, the service will not display that passwords have been collected but that it only has email information collected. This is likely because the data source is not available at haveibeenpwned.com or the attackers have bots that scrape or crawl websites for information such as email addresses.

There are even free services online that can be used to extract emails from a URL, such as Specrom Analytics (https://www.specrom.com/extract-emails-from-url-free-tool/) or using a simple Python script that can do the same as well. Then, we can compare whether the user accounts where we are getting multiple authentication attempts are easily searchable from the public website.

One way to reduce the amount of spam and brute-force attacks against users’ identities is by limiting the amount of public information that is available.

For instance, if your corporate website is published behind a **Web Application Firewall (WAF)**, you can block traffic based on user agents.

A user agent is a form of identity where the software (the browser) identifies itself to the web server. Most common browsers today use a user agent, for example, Mozilla/5.0 (Windows NT 10.0; Win64; x64), AppleWebKit/537.36 (KHTML, like Gecko), Chrome/97.0.4692.99, Safari/537.36, and Edge/97.0.1072.76.

## How are vulnerabilities utilized for attacks?

So, now that we have taken a closer look at some of the ways that attackers try to collect information about our end users either from scraping, phishing, or credential stuffing, we are going to take a closer look at some of the vulnerabilities that some of the different ransomware operators have been known to use in their attacks. Later in this section, I will go through how you can monitor vulnerabilities against your services.

Many of the vulnerabilities that we will go through are either utilized for initial compromise or to gain elevated access to a compromised machine and, lastly, lateral movement. The reason is to give you some understanding of how easy it can be to compromise a machine or a service and that the time before a high-severity vulnerability is known before ransomware operators start to leverage it is pretty short.

So, we are going to focus on the following vulnerabilities:

- **PrintNightmare:** CVE-2021-34527
- **Zerologon:** CVE-2020-1472
- **ProxyShell:** It consists of three different vulnerabilities that are used as part of a single attack chain: CVE-2021-34473, CVE-2021-34523, and CVE-2021-31207
- **Citrix NetScaler ADC:** CVE-2019-19781

## PrintNightmare

Let’s start with PrintNightmare, which was a vulnerability that was published in July 2021. Using this vulnerability, an attacker could run arbitrary code with system privileges on a remote system and local system, so long as the Print Spooler service was enabled. So, in theory, you could utilize this vulnerability to make the domain controllers run arbitrary code, so long as the Print Spooler service was running. This is because of the functionality within a feature called Point and Print, which allows a user to automatically download config information about the printers directly from the print server to the client.

All Microsoft **Common Vulnerabilities and Exposures (CVEs)** get published on MSRC with dedicated support articles, highlighting which systems are affected and recommendations in terms of workaround and other countermeasures, as seen here for PrintNightmare: `https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-34527`.

In regard to PrintNightmare, there were multiple scripts that the InfoSec community made that could easily be used; as an example, here’s a simple PowerShell payload that exploited the vulnerability, which did not require administrator access rights and comes with a predefined DDL file that creates a local admin account on the machine: `https://github.com/calebstewart/CVE-2021-1675`.

Benjamin Delpy, the creator of the popular tool called **Mimikatz**, also created a proof of concept by setting up a public *print server* that you could then use from an endpoint to connect to that public server, which would then automatically create a CMD pane running as a local system context.

It took Microsoft many weeks before they managed to provide patches and recommendations on how to fix this. In the middle of August, only 1 month later, there were already news articles about ransomware operators that were exploiting the PrintNightmare vulnerability to compromise organizations.

Microsoft provided recommendations when the vulnerability was known, which was to disable the Print Spooler service until they managed to provide a security fix. It also allowed many administrators to realize that the Print Spooler service is not required to run on servers that are not end user facing, such as Citrix/RDS servers.

> **Important note**
> A general best practice is to ensure that only required services are running on a service – for example, the Print Spooler service should *not* be running on a domain controller. This guidance document from Microsoft provides a list of the different services and recommendations for each of them: https://docs.microsoft.com/en-us/windows-server/security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server.

## Zerologon

Next, we have Zerologon, another high-severity CVE that exploits a vulnerability in the Netlogon process in Active Directory, which allows an attacker to impersonate any computer, including a domain controller.

To be able to leverage this vulnerability, the attack needed to be able to communicate with the domain controllers, such as having a Windows client that is joined to the Active Directory domain.

Then, the attackers would spoof another domain controller in the infrastructure and use the MS-NRPC protocol to change the password for the machine account in Active Directory, which is as simple as sending a simple TCP frame with the new password:

![](img/7b49a27a3c2adc52d097f2e77f3bf394_42_0.png)

Figure 1.11 – Zerologon attack process

Once the new password had been accepted, the attackers could then use that new account to start new processes with an Active Directory domain controller context, which was then used to compromise the remaining infrastructure. Zerologon has been used in many ransomware attacks to, through lateral movement, compromise Active Directory and gain access to the domain controllers.

This vulnerability was fixed in a patch from Microsoft in August 2020. In September 2020, the security researchers from Secura who discovered the vulnerability issued their research, and within a week, there were already different proofs of concept published on how you can leverage the exploit. You can find the link to the initial whitepaper on the vulnerability here: https://www.secura.com/uploads/whitepapers/Zerologon.pdf.

In the months after, many organizations were hit by Ruyk, where they used the Zerologon vulnerability. On average, most security researchers state that it takes between 60 and 150 days (about 5 months) for an average organization to install a patch once it has been released by the vendor.

## ProxyShell

Then, we have ProxyShell, which is a vulnerability consisting of three different CVEs used as part of a single attack chain that affected Microsoft Exchange 2013/2016/2019, which allowed attackers to do pre-authenticated RCE.

The main vulnerabilities lie in the Client Access Service (CAS) server component in Exchange, which is exposed to the internet by default to allow end users to access email services externally.

In short, the ProxyShell exploit does the following:

- Sends an Autodiscover request to leak the user's LegacyDN information with a known email address.
- Sends a MAPI request to the CAS servers to leak the user's SID using the LegacyDN.
- Constructs a valid authentication token from the CAS service using the SID and email address.
- Authenticates to the PowerShell endpoint and executes the code using the authentication token. The example code can be found on GitHub at https://github.com/horizon3ai/proxyshell.

Horizon3.ai released a Python script to showcase how easy it is to exploit this vulnerability (https://github.com/horizon3ai/proxyshell), where you just need to run the script and point it to an Exchange CAS server.

All these vulnerabilities were patched in April 2021, but the information was published publicly in June 2021.

In February 2022, it was discovered that a significant number of organizations had failed to update their Exchange services, even though it was urgently required. More precisely, 4.3% of all Microsoft Exchange services that were publicly accessible were still unpatched for the ProxyShell vulnerability. Out of those that did apply the ProxyShell patch, 16% of organizations did not install the subsequent patches that were released from July 2021 onward, which left them open to attacks. As a result, many organizations had still not fully eliminated the vulnerability, even after six months had passed. As seen in the following Shodan screenshot from February 2022, there were still quite a high amount of public-facing Exchange servers that had the vulnerability present:
