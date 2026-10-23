| Source | Event ID | Description |
|---|---|---|
| Active Directory security events | 4625 | Failed login attempt |
| Azure AD sign-in logs | 50126 | Invalid username or password |
| Citrix ADC Syslog | AAA LOGIN_FAILED | When the AAA module failed to log the user in |

Table 2.6 – Example of data sources to look for sign-in attempts

If you are uncertain what kind of other events you should be looking for, this list from Microsoft specifies different event IDs that you should be looking for: https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-1--events-to-monitor.

For Azure AD, Microsoft has also created a similar list of event IDs to look for: https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/security-operations-user-accounts#unusual-sign-ins.

> **Note**
> 
> Since Microsoft did not create a list of the different event IDs in Azure AD and what they mean, I have created a list here: https://github.com/msandbu/azuread/blob/main/azuread.md.

While we might have a product that can collect logs and analyze them, we may still have too much data that we need to process or understand whether it is a known attack pattern. Therefore, another important aspect of security monitoring is threat intelligence.

Threat intelligence can be looked at as a set of data that contains known attack behaviors, targets, and sources, which can often be integrated as a service as well.

To be able to collect and analyze all this data, we would need some service or tool. There are many different options to choose from, depending on your needs. While some vendors provide a cloud-only service, some vendors provide both on-premises and cloud-based services. Some examples of products and services that can provide log collection, analytics, and alerting are as follows:

+   - **Splunk**: This is one of the market leaders and can provide both on-premises and cloud-based services
+   - **Graylog**: This is open source but also has commercial models for cloud-based and on-premises services
+   - **ELK**: This is open source and more aimed at pure log collection; security alerting is an add-on
+   - **Sumologic**: A cloud-based Security Information and Event Management (SIEM) service
+   - **Humio**: A cloud-based logging service