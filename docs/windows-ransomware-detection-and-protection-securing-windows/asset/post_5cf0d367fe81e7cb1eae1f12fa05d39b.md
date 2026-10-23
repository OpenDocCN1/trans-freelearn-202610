
- Deny access to this computer from the network
- Deny logging on as a batch job
- Deny logging on as a service
- Deny logging on locally
- Deny logging on through Remote Desktop services

This is to ensure that if someone manages to compromise a domain admin account, they cannot use that account to do lateral movement across member servers using RDP or access it directly. It also ensures that someone with a domain admin account does not accidentally log on to other member servers using that account with, for instance, RDP.

> **Note**
> 
> You should also ensure that domain administrator accounts are not synchronized to Azure AD and that you have separate accounts for managing Active Directory and Azure AD.

## Security logging and monitoring

So far, we have looked at different zero-trust design principles and user life cycle management and how to ensure that users are using least-privilege access. Now, let's take a closer look at the foundation of security monitoring, which is logging.

Regardless of how many security mechanisms we have in place, it is always important to have services or tools in place to monitor activities and events within our systems, regardless of whether they are on-premises running within our data center or cloud services. Once we have that monitoring capability in place, we should be able to use the data that has been collected to look for signs of known attacks, abnormal user activity, or unusual traffic.

For instance, to get an overview of what kind of traffic is flowing in and out of a virtual machine and be able to see that in the context of what is going on inside the operating system, you need multiple log sources, such as the following:

- Data from the network (NetFlow or other flow logs)
- Data from EDR or other tools on the device to view the connection flow to services
- Data from security events or application logs

However, when building a security monitoring solution, we cannot just set up and capture all the data and events in our infrastructure since this would generate an extreme amount of data. As an example, NetFlow logs or other raw network data capture will result in an extreme amount of storage usage.

We should always design a monitoring service based on the resources we have and the number of security risks we face. In addition, we should determine what kind of attack vectors pose the most risk for our infrastructure.

Let me give an example of another customer project I was working on.

With this customer, we had a lot of external services such as Citrix, a VPN, and web applications that were available externally through a proxy service. Most services were integrated with Active Directory and Azure AD for authentication. In addition, they had plenty of different network equipment at the offices and a centralized firewall. The endpoint was also joined directly to Azure AD.

We quickly determined that the risks this organization faced would be related to external services that were publicly available, such as brute-force attempts or vulnerabilities. Secondly, it could be related to stolen credentials or credential stuffing. Therefore, based on those risks, we determined that we should collect the following set of data sources:

- Active Directory security events
- Azure AD sign-in logs and audit logs
- Syslog from the external services related to authentication attempts
- IIS error logs

These data sources were then collected into a centralized log service and used for analytics. While collecting the right data is one part of the puzzle, the other part is being able to analyze the data and knowing what to look for.

Here, you should define some use cases regarding what you should be looking for; take the following examples:

- How many incorrect authentication attempts occurred against services? (Such as multiple failed login attempts from the same device)
- Who has logged in remotely at abnormal times? (Such as multiple login attempts after midnight)
- Where have the authentication attempts come from? (Such as login attempts from different geographical locations)

Once you have defined the use cases, you need to understand what kind of activities you need to look for to map against the use cases.

For instance, if we were to look for failed login attempts across different data sources, we would need to have some built-in analytics feature to be able to query across different datasets:

| Source | Event ID | Description |
|---|---|---|
| Active Directory security events | 4625 | Failed login attempt |
| Azure AD sign-in logs | 50126 | Invalid username or password |
| Citrix ADC Syslog | AAA LOGIN_FAILED | When the AAA module failed to log the user in |

*Table 2.6 – Example of data sources to look for sign-in attempts*

If you are uncertain what kind of other events you should be looking for, this list from Microsoft specifies different event IDs that you should be looking for: https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-1--events-to-monitor.

For Azure AD, Microsoft has also created a similar list of event IDs to look for: https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/security-operations-user-accounts#unusual-sign-ins.

> **Note**
> 
> Since Microsoft did not create a list of the different event IDs in Azure AD and what they mean, I have created a list here: https://github.com/msandbu/azuread/blob/main/azuread.md.

While we might have a product that can collect logs and analyze them, we may still have too much data that we need to process or understand whether it is a known attack pattern. Therefore, another important aspect of security monitoring is threat intelligence.

Threat intelligence can be looked at as a set of data that contains known attack behaviors, targets, and sources, which can often be integrated as a service as well.

To be able to collect and analyze all this data, we would need some service or tool. There are many different options to choose from, depending on your needs. While some vendors provide a cloud-only service, some vendors provide both on-premises and cloud-based services. Some examples of products and services that can provide log collection, analytics, and alerting are as follows:

- **Splunk**: This is one of the market leaders and can provide both on-premises and cloud-based services
- **Graylog**: This is open source but also has commercial models for cloud-based and on-premises services
- **ELK**: This is open source and more aimed at pure log collection; security alerting is an add-on
- **Sumologic**: A cloud-based Security Information and Event Management (SIEM) service
- **Humio**: A cloud-based logging service
- **Exabeam**: A cloud-based SIEM service
- **Azure Sentinel**: A cloud-based SIEM service

While all these products are good options when it comes to security monitoring, in the next chapter, we will focus on the use of Azure Sentinel as a cloud-based service to do log collection and security monitoring based on the use cases listed here.

## A secure foundation within Microsoft Azure

While we have gone through a lot of content related to zero trust and identity access management, which are crucial factors related to preventing ransomware, we also need to take a closer look at building a secure network design.

The most common network model is a zone-based approach, where you split up the network into different segmented zones depending on function or severity. Traffic going between these different zones is handled through a centralized security mechanism such as a firewall.

In addition, these resources that are placed in the internal zones should not be able to speak with the internet directly. One of the main culprits we see in ransomware cases is where a server is directly available to the internet and has a public-facing service such as SSH/RDP/SMB that gets brute-forced. Another is when a server has direct access to the internet – if an attacker manages to compromise them, they can more easily communicate back to C2 servers or file services if they need to download additional payloads.

So, having proper network security mechanisms in place can at least slow down an attack. These types of security measures can also slow down any drive-by downloads if someone was unfortunate enough to try and install some malware-infected software.

Therefore, you should also have some secure hub between internal resources and the internet to ensure that traffic flow is controlled going outbound.

Services that need to be exposed to the internet, such as web applications, should always be placed behind a security mechanism. This is to ensure that servers are not directly available to the internet and that you have more control over the traffic flows and can apply better security mechanisms, such as protecting against known web-based attacks.

While many might argue that you should have network security features that can inspect traffic going in and out, such as TLS inspection, which decrypts traffic to see into the traffic flow, we are coming to the point where there are newer transport protocols such as HTTP/3.

Let’s take a closer look at how we can build a secure network foundation in Microsoft Azure. Building this in Microsoft Azure is not that much different from any other secure network design architecture since many of the design principles are the same.
